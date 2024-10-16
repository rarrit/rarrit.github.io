---
title: "[Next.js] Supabase Server-Side Auth 적용"
date: 2024-10-15
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
  - supabase 
tags:
  - supabase
  - auth
author_profile: true
sidebar_main: true
---

## :ledger: Supabase Server-Side Auth를 사용해보자!
Next.js와 Supabase를 사용해서 서버 측에서 인증 정보를 다루는 방법을 익히기 위해 [공식문서 - Setting up Server-Side Auth for Next.js](https://supabase.com/docs/guides/auth/server-side/nextjs?queryGroups=router&router=app)를 참고해서 구현해보았다.

### :one: 프로젝트 세팅

#### :pushpin: 1-1) supabase package install
`Next.js` 프로젝트를 생성하고, Supabase 패키지를 추가한다.

```bash
yarn add @supabase/supabase-js @supabase/ssr
```

#### :pushpin: 1-2) 환경변수 설정
루트 디렉토리에 `.env.local` 파일을 만들고 아래와 같이 설정한다.

```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

### :two: 유틸리티 함수 작성
`src/utils/supabase` 디렉토리를 생성하고 아래의 파일을 추가한다.

#### :pushpin: 2-1) 클라이언트
CSR에서 실행함

- src/utils/supabase/client.ts

```tsx
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

#### :pushpin: 2-1) 서버
서버에서 실행함

- src/utils/supabase/server.ts

```tsx
import { createServerClient, type CookieOptions } from '@supabase/ssr'
import { cookies } from 'next/headers'

export function createClient() {
  const cookieStore = cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // The `setAll` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        },
      },
    }
  )
}
```

### :three: 미들웨어 연결

#### :pushpin: 3-1) 루트 디렉토리에 설정
`src/middleware.ts` 파일을 프로젝트 생성 후 아래와 같이 코드를 작성한다.<br/>
- 서버 컴포넌트는 쿠키를 작성할 수 없어서 만료된 이증 토큰을 새로 고치고 저장하기 위한 미들웨어가 필요함

```tsx
import { type NextRequest } from 'next/server'
import { updateSession } from '@/utils/supabase/middleware'

export async function middleware(request: NextRequest) {
  return await updateSession(request)
}

export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     * Feel free to modify this pattern to include more paths.
     */
    '/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',
  ],
}
```

#### :pushpin: 3-2) 유틸리티 폴더에 추가
`src/utils/supabase/middleware.ts` 파일을 생성 후 아래와 같이 코드를 작성한다.

```tsx
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function updateSession(request: NextRequest) {
  let supabaseResponse = NextResponse.next({
    request,
  })

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return request.cookies.getAll()
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value, options }) => request.cookies.set(name, value))
          supabaseResponse = NextResponse.next({
            request,
          })
          cookiesToSet.forEach(({ name, value, options }) =>
            supabaseResponse.cookies.set(name, value, options)
          )
        },
      },
    }
  )

  // IMPORTANT: Avoid writing any logic between createServerClient and
  // supabase.auth.getUser(). A simple mistake could make it very hard to debug
  // issues with users being randomly logged out.

  const {
    data: { user },
  } = await supabase.auth.getUser()

  if (
    !user &&
    !request.nextUrl.pathname.startsWith('/login') &&
    !request.nextUrl.pathname.startsWith('/auth')
  ) {
    // no user, potentially respond by redirecting the user to the login page
    const url = request.nextUrl.clone()
    url.pathname = '/login'
    return NextResponse.redirect(url)
  }

  // IMPORTANT: You *must* return the supabaseResponse object as it is. If you're
  // creating a new response object with NextResponse.next() make sure to:
  // 1. Pass the request in it, like so:
  //    const myNewResponse = NextResponse.next({ request })
  // 2. Copy over the cookies, like so:
  //    myNewResponse.cookies.setAll(supabaseResponse.cookies.getAll())
  // 3. Change the myNewResponse object to fit your needs, but avoid changing
  //    the cookies!
  // 4. Finally:
  //    return myNewResponse
  // If this is not done, you may be causing the browser and server to go out
  // of sync and terminate the user's session prematurely!

  return supabaseResponse
}
```


### :four: 로그인,회원가입 페이지 생성하기

#### :pushpin: 4-1) actions 파일 생성 
`app/login/actions.ts` 해당 위치에 파일을 생성 후 아래와 같이 작성해준다.

- Supabase가 Action에서 호출되므로, 서버와 클라이언트에 맞게 사용한다.
  - @/utils/supabase/server.ts
  - @/utils/supabase/client.ts

```tsx
'use server'

import { revalidatePath } from 'next/cache'
import { redirect } from 'next/navigation'

import { createClient } from '@/utils/supabase/server'

export async function login(formData: FormData) {
  const supabase = createClient()

  // type-casting here for convenience
  // in practice, you should validate your inputs
  const data = {
    email: formData.get('email') as string,
    password: formData.get('password') as string,
  }

  const { error } = await supabase.auth.signInWithPassword(data)

  if (error) {
    redirect('/error')
  }

  revalidatePath('/', 'layout')
  redirect('/')
}

export async function signup(formData: FormData) {
  const supabase = createClient()

  // type-casting here for convenience
  // in practice, you should validate your inputs
  const data = {
    email: formData.get('email') as string,
    password: formData.get('password') as string,
  }

  const { error } = await supabase.auth.signUp(data)

  if (error) {
    redirect('/error')
  }

  revalidatePath('/', 'layout')
  redirect('/')
}
```

#### :pushpin: 4-2) error 페이지 생성
공식문서에는 "use client"가 없어 추가해준다.

```tsx
"use client"

export default function ErrorPage() {
  return <p>Sorry, something went wrong</p>
}
```

#### :pushpin: 4-3) 로그인,회원가입 페이지 생성
`app/login/page.tsx` 로그인, 회원가입 기능을 사용할 수 있는 페이지를 생성한다.

- formAction을 통해 actions.ts의 함수를 사용한다.

```tsx
import { login, signup } from './actions'

export default function LoginPage() {
  return (
    <form>
      <label htmlFor="email">Email:</label>
      <input id="email" name="email" type="email" required />
      <label htmlFor="password">Password:</label>
      <input id="password" name="password" type="password" required />
      <button formAction={login}>Log in</button>
      <button formAction={signup}>Sign up</button>
    </form>
  )
}
```

#### :pushpin: 4-5) 로그아웃 적용
로그아웃 기능은 클라이언트 컴포넌트로 설정해서 사용해야한다.

- `app/_components/LogoutButton.tsx` 생성 후 아래와 같이 작성
- 유틸 함수에서 `client.ts`로 import해서 사용함

```tsx
'use client'; // 클라이언트 컴포넌트로 설정

import { useState } from 'react';
import { createClient } from '@/utils/supabase/client';

export default function LogoutButton() {
  const [loading, setLoading] = useState(false);
  const supabase = createClient();

  const handleSignOut = async () => {
    setLoading(true);
    await supabase.auth.signOut();
    window.location.href = '/login'; // 로그아웃 후 로그인 페이지로 리디렉션
  };

  return (
    <button onClick={handleSignOut} disabled={loading}>
      {loading ? 'Loading...' : '로그아웃'}
    </button>
  );
}
```

#### :pushpin: 4-6) 사용자 이메일 확인 생략
[공식문서](https://supabase.com/docs/guides/auth/server-side/nextjs?queryGroups=router&router=app)에서 회원가입 시 이메일 인증 확인을 위한 route handler를 생성해주지만, 테스트 목적으로 사용해서 이 부분은 생략해주었다. 

- 공식문서 7번 내용 생략
- 수퍼베이스의 좌측 사이드바 project settings -> Authentication 메뉴 클릭 -> SMTP Settings 에서 체크를 해제해준다.

#### :pushpin: 4-6) 로그인 여부 확인
아래와 같이 로그인을 판별하여, 로그아웃 버튼과 로그인 유저의 email을 확인할 수 있다.

```tsx
import { createClient } from '@/utils/supabase/server'
import Link from "next/link";
import LogoutButton from './_components/LogoutButton';


export default async function Home() {
  const supabase = createClient()
  console.log(supabase.auth);
  const { data } = await supabase.auth.getUser();

  return (
    <div> 
      {
        data.user 
        ? (
          <>
            <p>Hello {data.user.email}</p>
            <LogoutButton/>
          </>
        )
        : <Link href={"/login"}>로그인</Link>
      }        
    </div>
  );
}
```

### :fire: 마무리
[공식문서 - Setting up Server-Side Auth for Next.js](https://supabase.com/docs/guides/auth/server-side/nextjs?queryGroups=router&router=app)를 통해 Supabase와 Next.js의 Server-Side Auth를 연동함으로써 매우 간단하고 효율적으로 인증 시스템을 구축할 수 있었다. 특히 Supabase의 미들웨어와 유틸리티 함수를 적절히 활용하면, 클라이언트와 서버 간의 세션 관리를 손쉽게 처리할 수 있다는 점이 인상적이었다.

- [https://github.com/rarrit/next-supabase-auth](https://github.com/rarrit/next-supabase-auth)