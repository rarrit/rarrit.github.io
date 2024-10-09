---
title: "[Next.js] 서버 컴포넌트에서 ERROR UI가 노출이 되지 않은 이유를 알아보자."
date: 2024-10-03
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - next js
  - error
author_profile: true
sidebar_main: true
---

## :ledger: [Next.js] ERROR UI가 노출이 되지 않은 이유를 알아보자.
이번 Next.js를 사용한 [리그오브레전드 정보 프로젝트](https://rarrit.github.io/mini/til/next-lol06/)에서 겪었던 에러입니다.

### :one: 개요
Next.js는 기본적으로 에러 처리를 위해 `error.js` 파일을 제공한다. 그리고 커스텀 에러 페이지를 만들어 사용자에게 에러를 안내할 수 있는데, 이번 프로젝트를 진행하면서 error ui 가 노출되지 않는 상황을 겪었고 원인과 해결방법에 대해 알아보았다.

### :two: 원인
먼저 Rotation (CSR) 페이지에서는 정상적으로 노출되는 것을 확인했다. 하지만 서버 컴포넌트인 Champion List 페이지와 Item List 페이지에서는 `global-error.tsx` 커스텀한 Ui가 노출되지 않았다. 이유는 아래와 같다.

- `app/global-error.tsx` 파일만 있을 경우에는 <u>클라이언트 사이드 렌더링(CSR)환경에서 발생하는 에러</u>에 대해서만 UI가 제공된다. 즉, 브라우저에서 발생하는 에러를 처리할 수 있음
- 서버 컴포넌트에서 발생하는 에러에 대해서도 UI를 제공하고 싶다면, `app/error.tsx` 파일을 추가해서 전역으로 관리해주거나 특정 라우트에서 서버에서 발생한 에러를 처리할 수 있도록 라우트마다 에러 페이지를 설정해야한다.

### :three: 해결방법
서버컴포넌트에서도 CSR환경과 동일한 ui를 보여주기 위해서 `app/error.tsx`를 생성하고 GlobalError(global-error)컴포넌트를 사용해서 같은 error ui를 보여줄 수 있도록 적용했다.

```tsx
// app/error.tsx
"use client";

import GlobalError from "./global-error";

type ErrorProps = {
  error: Error & { digest?: string }; // error의 타입 정의
  reset: () => void; // reset 함수의 타입 정의
};

export default function Error({ error, reset }: ErrorProps) {
  return (
    <GlobalError error={error} reset={reset} />
  );
}
```

### :fire: 마무리

![error-ui](https://github.com/user-attachments/assets/59c13abf-1f4f-4b8d-a46a-eba58be59d56)

서버와 클라이언트에서 모두 에러 UI를 처리하려면, 전역 클라이언트 에러 핸들링을 위한 `global-error.tsx`와 각 라우트 또는 페이지별 서버 에러 핸들리을 위한 `error.tsx` 파일을 사용하면 된다.

- CSR 에러: global-error.tsx
- 서버 컴포넌트 에러: 각 경로의 error.tsx
