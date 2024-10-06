---
title: "[Next.js] Unhandled Runtime Error 해결 "
date: 2024-10-07
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - hydration
author_profile: true
sidebar_main: true
---

## :ledger: next.js에서 props로 데이터를 전달하는 과정에서 생긴 에러
next.js 리그오브레전드 정보 앱을 만들며 props를 통해 데이터를 전달하고 사용했을 때 경험한 에러다.

### :one: 개요
item의 데이터를 전달 받았을 때 태그가 들어가져있어서 `html-react-parser` 라이브러리를 사용했다. 그런데 해당 라이브러리는 CSR 환경에서 적용이 가능하여, 컴포넌트로 분리해서 적용 후 props를 통해 해당 데이터를 노출했고 그 과정에서 아래와 같은 에러가 노출되었다.

![Unhandled Runtime Error](https://github.com/user-attachments/assets/591f44e9-3bbe-4263-8f69-8a58e56aee34)

### :two: 원인
에러는 친절하게 [공식문서 - https://nextjs.org/docs/messages/react-hydration-error](https://nextjs.org/docs/messages/react-hydration-error)를 통해 확인이 가능한데, 공식문서로 들어가서 번역을 해보면 `텍스트 내용이 서버에서 렌더링된 HTML과 일치하지 않습니다.` 라고 타이틀이 적혀있고 상세 내용에서 <u>애플리케이션을 렌더링할 때, 서버에서 사전 렌더링된 React 트리와 브라우저에서 처음 렌더링(수화) 중에 렌더링된 React 트리 사이에 차이가 있습니다.</u> 확인할 수 있었다.

### :three: 해결방법
고치는 방법도 [공식문서 - https://nextjs.org/docs/messages/react-hydration-error](https://nextjs.org/docs/messages/react-hydration-error) 확인이 가능했는데, 해결 방법에 적혀있듯이, `useEffect`를 적용했더니 에러가 없어졌다.

#### :pushpin: 3-1) 기존 코드

```tsx
"use client";
import React from 'react'
import parse from 'html-react-parser';

type ItemDescProps = {
  description: string;
}

const ItemDesc: React.FC<ItemDescProps> = ({ description }) => {
  return <p className="item-desc max-m:text-[13px]">{parse(description)} </p>
}

export default ItemDesc;

```

#### :pushpin: 3-2) 수정 코드

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
이번 강의에서 hydration 을 배웠고, Q&A 세션에서 분명 난 틀렸었다.. 그리고 다시 마주친 이번 에러또한 hydration과 매우 밀접해 있었다. 이번 과제기간이 얼마 남지 않아서 공식문서와 다른 사람의 블로그를 통해 수정했는데 과제 제출 후 꼭 이해하고 넘어갈 수 있도록 해보자

- 참고 블로그 01: [https://velog.io/@hyeonq/Next.js-Hydration-failed-error](https://velog.io/@hyeonq/Next.js-Hydration-failed-error)
- 참고 블로그 02: [https://blog.hwahae.co.kr/all/tech/13604](https://blog.hwahae.co.kr/all/tech/13604)