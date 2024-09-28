---
title: "[Next.js] Error UI 생성 및 사용방법에 대해 알아보자."
date: 2024-09-28
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
tags:
  - next js
  - error ui
  - error handling
author_profile: true
sidebar_main: true
---

## :ledger: Next.js의 Error UI 및 Error Handling
Next.js에서는 에러가 발생할 경우, 기본적으로 해당 에러를 보여주는 페이지가 제공된다. 하지만, 이 에러 페이지를 커스텀하여 사용자에게 더 나은 경험을 제공할 수 있으며, 이번 포스트에서는 Next.js에서 어떻게 커스텀 Error UI를 만들고, 에러 핸들링 전략을 적용할 수 있는지 알아보자!

### :one: Error UI란 무엇인가?
에러 UI는 말 그대로 애플리케이션이 예상치 못한 오류를 만났을 때 사용자에게 보여주는 화면이다. 사용자는 이 화면을 통해 무엇이 잘못되었는지, 어떻게 문제를 해결할 수 있을지 알게 된다.

#### :pushpin: 1-1) 기본적인 에러 화면
- Next.js는 자동으로 에러 화면을 보여주지만, 이는 개발 환경에서만 나타나는 화면이다. 
- 실제 프로덕션 환경에서는 기본적으로 에러 메시지가 숨겨져 있기 때문에, 사용자 친화적인 커스텀 에러 페이지가 필요함

#### :pushpin: 1-1) json-server delay 
`package.json` 파일을 열고 `json-server` 스크립트에 딜레이를 추가한다. (5초 지연 = --delay 5000)

### :two: Error UI 생성
Error UI를 생성할 때 중요한 점은 `Error Component 는 꼭 클라이언트 컴포넌트`여야 한다는 점이다.

- 클라이언트 컴포넌트는 최상단에 `'use client'` 선언
- 아래는 error 컴포넌트 코드이다.

```tsx
// ~/error.tsx
'use client'; // 중요! 중요!

import { useEffect } from 'react';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

#### :pushpin: 2-1) Error 컴포넌트는 왜 클라이언트 컴포넌트여야 할까?
error.js는 자동으로 중첩된 자식 세그먼트 또는 page.js 컴포넌트를 감싸는 `React Error Boundary`를 생성한다.

1. **React Error Boundaries**
  - React의 오류 경계는 클라이언트 측에서 동작함. 
  - 이는 React 컴포넌트 트리 내에서 발생하는 오류를 잡아내고, 폴백 UI를 렌더링 하는 역할을 한다. 
  - 서버 사이드에서는 이러한 경계가 작동하지 않으므로, 클라이언트 측에서 오류를 처리하기 위해서는 오류 컴포넌트가 클라이언트 컴포넌트로 정의되어야 한다.
2. **동적 상호작용** 
  - 오류 발생 시 사용자에게 즉시 피드백을 제공하고, 오류를 복구할 수 있는 인터페이스를 제공하기 위해 클라이언트 측에서 동적으로 상호작용이 필요하다. 
  - 예를 들어, `reset` 함수를 사용하여 오류를 다시 시도하게 하는 기능은 클라이언트 측에서만 가능함.
3. **상태 관리**
  - 클라이언트 컴포넌트는 오류 발생 시 상태를 관리하고, 그 상태에 따라 적절한 UI를 렌더링할 수 있다. 
  - 이는 사용자 경험을 향상시키고, 오류 발생 시에도 애플리케이션이 부분적으로 계속 작동할 수 있게 한다.

#### :pushpin: 2-2) Error Boundary란 무엇일까?
- `에러 바운더리`는 하위 컴포넌트 트리의 어디에서든 <u>자바스크립트 에러를 기록</u>하며 깨진 컴포넌트 트리 대신 `Fallback UI`를 보여주는 React 컴포넌트로 `에러 바운더리`는 렌더링 도중 생명주기 메서드 및 그 아래에 있는 전체 트리에서 에러를 잡아낸다.
- 과거에는 컴포넌트 내부의 자바스크립트 에러를 정상적으로 처리할 수 있는 방법이 없었는데, 에러 바운더리를 설정해주면 바운더리 내의 컴포넌트와 그 하위 컴포넌트에서 발생하는 에러를 감지해서 `fallback UI`를 띄어줄 수 있게됨

#### :pushpin: 2-3) Error Boundary의 한계
Error Boundary는 이러한 오류를 캐치하지 못하고 다른 오류들만 캐치하게 된다.

- 이벤트 핸들러
- 비동기적 코드 (예: setTimeout 혹은 requestAnimationFrame 콜백)
- 서버 사이드 렌더링
- 자식에서가 아닌 에러 경계 자체에서 발생하는 에러

### :three: Layout Error 핸들링
- `Next.js`에서 Error는 가까운 상위 에러 바운더리로 오류가 되며, `error.tsx` 파일은 동일한 라우트 세그먼트의 `layout.tsx` 또는 `template.tsx` 컴포넌트에서 발생하는 오류를 처리하지 않는다. 
- 이러한 오류를 처리하려면 레이아웃 부모 세그먼트에 `error.tsx` 파일을 추가해야한다.

#### :pushpin: 3-1) 전역 error 생성
- `global-error.tsx`를 루트에 생성해서 처리해보자

```tsx
// app > global-error.tsx
"use client";

import { useEffect } from "react";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  
  useEffect(() => {
    console.error(error);
  }, [error]);

  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```

#### :pushpin: 3-2) Error 복구하기
사용자가 페이지를 새로고침해서 다시 불러오게 할 수 있겠지만 오류가 난 페이지를 다시 시도하면 문제가 해결될 수 있다. 이럴 경우 사용자가 오류를 복구하도록 시도할 수 있게 `reset()`함수를 제공해준다.

```tsx
"use client";

// error 처리
import { useRouter } from "next/navigation";
import { startTransition, useEffect } from "react";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  const { refresh } = useRouter();
  // react 18 에서 생긴 메서드 => startTransition()
  useEffect(() => {
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong!</h2>
      <p>{error.message}</p>
      <button onClick={() => 
      startTransition(() => {
        refresh();
        reset();
      })}>Try again</button>
    </div>
  );
}
```

### :four: 서버 에러는 어떻게 처리함?
에러 바운더리는 무조건 클라이언트 컴포넌트여야 한다는데, 서버 컴포넌트에서 에러가 난 내용들은 어떡할까?

- 서버 컴포넌트에서 오류가 발생하면 Next.js는 해당 오류 객체를 가장 가까운 `error.js` 파일에 `error prop`으로 전달한다. 
- 프로덕션 환경에서는 <u>오류 정보가 민감(Sensitive) 하지 않은 상태</u>로 전달됨.

#### :pushpin: 4-1) 민감한 오류란?
프로덕션 환경에서는 클라이언트로 전달되는 오류 객체에 일반적인 메시지와 digest 속성만 포함되며, 이는 오류에 포함될 수 있는 잠재적으로 민감한 세부 정보를 클라이언트에 노출하지 않도록 하기 위한 보안 조치이다.

- **message 속성:** 오류에 대한 일반적인 메시지를 포함함.
- **digest 속성:** 서버 측 로그에서 해당 오류를 찾는 데 사용할 수 있는 자동 생성된 해시를 포함함.

개발 환경에서는 클라이언트로 전달되는 오류 객체가 직렬화되어 원본 오류의 메시지를 포함하며, 이는 디버깅을 쉽게 하기 위함이다.

### :fire: 마무리
`Next.js`에서는 서버 사이드 및 클라이언트 사이드에서 에러를 처리하는 다양한 방법을 제공하며, 커스텀 `Error UI`를 통해 사용자 경험을 향상시키고, `Error Boundary`를 통해 클라이언트 에러를 안전하게 처리하며, 외부 도구를 사용해 에러 모니터링을 적극적으로 활용할 수 있다

#### :pushpin: 테스트
- [rarrit github - /rarrit/next-js-study-app/tree/feat/error](https://github.com/rarrit/next-js-study-app/tree/feat/error)