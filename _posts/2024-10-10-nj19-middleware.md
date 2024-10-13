---
title: "[Next.js] MiddleWare(미들웨어)란 무엇인지 알아보자"
date: 2024-10-10
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - lib
tags:
  - middleware
author_profile: true
sidebar_main: true
---

## :ledger: Next.js의 MiddleWare란 무엇인지 알아보자
Next.js의 `MiddleWare`는 요청(request)을 가로채고 처리할 수 있는 서버 측 기능이다. 이를 통해 요청이 특정 페이지에 도달하기 전에 추가적인 로직을 실행할 수 있어, 인증이나 리다이렉트와 같은 작업을 수행할 때 유용하다. 사용법에 대해 알아보자

### :one: MiddleWare

#### :pushpin: 1-1) MiddleWare 사용법
- 미들웨어 파일은 프로젝트 src(루트폴더) 하위에 단 1개만 생성이 가능하다. 
- `src > middleware.ts` 생성하고 아래의 코드를 넣어준다. 코드는 [공식문서](https://nextjs.org/docs/app/building-your-application/routing/middleware)에 있음
- `middleware`: config에서 matcher를 설정한 위치에서 실행됨
- `config`: 어느 위치에서 미들웨어를 실행시킬 지 matcher를 통해 설정함

```tsx
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  // return NextResponse.redirect(new URL('/home', request.url))
  console.log(request);
}
 
// See "Matching Paths" below to learn more
export const config = {
  // matcher: '/about/:path*',
  // matcher: ['/about/:path*','/dashboard/:path*']; 배열로 넣어줄 수 있음 (about,dashboard 뒤에 모든 값)
  matcher: '/',
  
}
```

#### :pushpin: 1-2) request.headers
`request.headers`는 들어오는 요청의 헤더 정보를 제공하는 객체이다. 헤더는 클라이언트가 서버로 전송할 때 함께 포함하는 메타데이터로, 사용자 에이전트(User-Agent)나 인증 토큰, 콘텐츠 유형(Content-Type)과 같은 정보를 포함할 수 있다.

- 헤더를 사용하여 클라이언트의 브라우저 유형에 따라 다른 동작을 수행하거나, 인증 토큰을 읽어 인증 여부를 확인할 수 있음

```tsx
export function middleware(req: NextRequest) {
  const userAgent = req.headers.get('user-agent');
  console.log(userAgent); // 클라이언트의 브라우저 정보 출력

  return NextResponse.next();
}
```

#### :pushpin: 1-3) request.nextUrl
`request.nextUrl`은 요청된 URL에 대한 정보를 포함하는 객체이다. 
이를 통해 요청된 페이지의 경로나 쿼리 파라미터 등을 확인할 수 있으며, 이 URL 정보를 바탕으로 특정 조건에 따라 리다이렉트나 다른 처리 로직을 작성할 수 있다.

- 이 기능은 사용자가 접근하려는 URL을 기반으로 조건부 로직을 추가할 때 유용하다. 예를 들어, 쿼리 파라미터에 따라 페이지 접근을 제어할 수 있음.

```tsx
export function middleware(req: NextRequest) {
  const { pathname, searchParams } = req.nextUrl;
  console.log('요청된 경로:', pathname);
  console.log('쿼리 파라미터:', searchParams.toString());

  return NextResponse.next();
}
```

#### :pushpin: 1-4) request.cookies
`request.cookies`는 요청과 함께 전송된 쿠키 정보를 포함하는 객체이다. 사용자의 인증 상태나 세션을 관리할 때, 쿠키는 자주 사용됨.

- 이 예시에서처럼 쿠키를 통해 인증 토큰을 확인하고, 토큰이 없을 경우 로그인 페이지로 리다이렉트할 수 있음.

```tsx
export function middleware(req: NextRequest) {
  const token = req.cookies.get('token'); // 쿠키에서 'token'을 가져옴
  if (!token) {
    return NextResponse.redirect(new URL('/login', req.url));
  }

  return NextResponse.next();
}
```


### :fire: 마무리
Next.js의 미들웨어의 기능을 통해 <u>인증, 라우팅 제어, 페이지 보호 등</u>의 다양한 동작을 효율적으로 처리할 수 있다.