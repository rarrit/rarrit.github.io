---
title: "[Next.js] 404 Not found 페이지 만들기"
date: 2024-09-26
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
  - next special file
  - 404 not found
author_profile: true
sidebar_main: true
---

## :ledger: Next.js에서 Not Found 페이지 만들기
웹사이트를 운영할 때 사용자가 존재하지 않는 페이지에 접근하면 **404 Not Found** 페이지를 보여주는 것이 일반적이다. `Next.js`에서는 <u>아주 쉽게 404 페이지를 커스터마이징 할 수 있다.</u> 

### :one: 커스텀 404 Not Found 페이지 생성
next.js에서 별도 설정을 하지 않아도 기본 스타일이 된 Not Found 에러 페이지를 제공하지만 커스텀으로 사용하려면 아래와 같이 사용할 수 있다.

```tsx
// src > app > not-found.tsx
import Link from 'next/link';

const NotFound = () => {
  return (
    <div>
      <h1>404 - 페이지를 찾을 수 없습니다</h1>
      <p>존재하지 않는 페이지입니다.</p>
      <Link href="/">홈으로</Link> 
    </div>
  )
};

export default NotFound;
```

### :fire: 마무리
Next.js의 App Router를 사용하여 404 Not Found 페이지를 만드는 방법을 알아보았다. 404 페이지는 웹 사이트에서 사용자 경험을 향상시키는 중요한 요소 중 하나이며 다양하게 커스텀도 가능할 것 같다.