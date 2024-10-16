---
title: "[Next.js] Supabase와 Middleware로 어드민 사용자 전용 페이지 만들기"
date: 2024-10-16
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

## :ledger: supabase와 middleware를 사용해서 어드민 유저만 접속 가능한 페이지를 만들어보자.
1차로 [superbase server-side auth 적용](https://rarrit.github.io/til/next/supabase/nj24-next-supabase/) 회원가입, 로그인 페이지 기능을 적용 후 admin 유저만 접속 가능한 페이지를 만들어 보도록했다.

### :one: middleware.ts 수정
`app/utils/supabase/middleware.ts` 파일에서 값을 설정해준다.

#### :pushpin: 1-1) 기존 코드에서 사용할 부분만 사용
기존에 작성된 미들웨어 코드에서 `user` 데이터는 필요해서 해당 부분을 제외하고 주석처리를 해주었다.

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

  const {
    data: { user },
  } = await supabase.auth.getUser()

  // 기존 조건문 주석 처리
  // if (
  //   !user &&
  //   !request.nextUrl.pathname.startsWith('/login') &&
  //   !request.nextUrl.pathname.startsWith('/auth')
  // ) {
  //   // no user, potentially respond by redirecting the user to the login page
  //   const url = request.nextUrl.clone()
  //   url.pathname = '/login'
  //   return NextResponse.redirect(url)
  // }

  return supabaseResponse
}
```

#### :pushpin: 1-2) 조건 처리
현재 products 페이지를 들어가려면 조건은 `로그인`이 되어있어야하고, 회원 정보가 admin user여야 한다는 것이다. 기존에 주석으로 처리한 조건문 위치에 아래의 코드를 넣어주었다.

```tsx
// 로그인하지 않은 유저가 접근하면 로그인 페이지로 리디렉션함
if(!user && request.nextUrl.pathname.startsWith('/products')) {
  const url = request.nextUrl.clone()  
  url.pathname = '/login'
  return NextResponse.redirect(url)
}
// 로그인 상태일 때 실행
if (user) {
  // 로그인된 사용자의 is_admin 값 확인
  const { data: profileData } = await supabase
    .from('profiles')
    .select('is_admin')
    .eq('id', user.id) // 로그인된 사용자의 프로필 확인
    .single();

  // console.log("profileData?.is_admin ==>", profileData?.is_admin);
  // is_admin이 false인 경우 (admin user가 아닌 경우) 메인 페이지로 리디렉션
  if (profileData?.is_admin === false && request.nextUrl.pathname.startsWith('/products')) {
    const url = request.nextUrl.clone();
    url.pathname = '/';
    return NextResponse.redirect(url);
  }
}
```


### :fire: 마무리
`Supabase`와 `Next.js`의 미들웨어를 활용하여 어드민 사용자 전용 페이지를 구현해보았따. 이를 통해 사용자 인증 및 권한 관리의 중요성을 다시 한번 확인할 수 있었으며, 어드민 페이지에 추가적인 관리 기능을 구현해볼 예정이다.

- [https://github.com/rarrit/next-supabase-auth](https://github.com/rarrit/next-supabase-auth)