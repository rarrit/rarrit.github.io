---
title: "[Next.js] HTML 파싱 라이브러리 html-react-parser"
date: 2024-10-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - lib
tags:
  - html-react-parser
author_profile: true
sidebar_main: true
---

## :ledger: Next.js에서 데이터를 안전하게 렌더링하려면 어떤 방법이 있는지 찾아보던 중 html-react-parser를 알게되었다.
이전에 React로 프로젝트를 진행하며, `dangerouslySetInnerHTML`를 사용했는데, 튜터님께서 XSS공격에 취약해서 다른 방법이 있는지 찾아보고 사용해보라고 하셨던 기억이 있다. 이번 Next.js 프로젝트에서 API를 활용하여 데이터를 바인딩하는 과정에서 tag가 있어 안전하게 렌더링 하려면 어떤 방법이 있는지 찾아보게 되었고 `html-react-parser`라이브러리를 알게되었다.

### :one: html-react-parser 설치 방법

```bash
# npm 
npm install html-react-parser

# yarn
yarn add html-react-parser
```

### :two: 사용 예시
코드는 아래와 같다. 주의할 점은 CSR 환경에서 사용해야하며, `useEffect`를 사용하지 않으면 `hydration` 관련 에러를 경험할 수 있음

```tsx
"use client";
import { useEffect, useState } from 'react'
import parse from 'html-react-parser';

type ItemDescProps = {
  description: string;
}

const ItemDesc: React.FC<ItemDescProps> = ({ description }) => {
  const [isDesc, setIsDesc] = useState("");
  useEffect(() => {
    setIsDesc(description);
  }, [])
  return <p className="item-desc max-m:text-[13px]">{parse(isDesc)} </p>
}

export default ItemDesc;

```

### :fire: 마무리
- `html-react-parser` 같은 라이브러리를 사용하는 방법이 `dangerouslySetInnerHTML` 없이 HTML을 렌더링하는 안전한 대안임을 알게되었다.
- 수동으로 데이터를 JSX로 변환하는 방법은 가능하지만 복잡한 데이터를 처리할 때는 비효율적일 수 있다고 함.
- `DOMPurify`도 대안으로 좋다고 하는데, 어떤 차이가 있는지 알아볼 예정이다.