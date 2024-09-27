---
title: "[Next.js] Server & Client Component 알아보기"
date: 2024-09-26
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - development
tags:
  - next js
  - next seo
  - metadata
author_profile: true
sidebar_main: true
---

## :ledger: Next.js Server & Client Component 알아보기
Next.js는 Server Components와 Client Components를 통해 보다 최적화된 렌더링을 제공한다.

### :one: Server Component란?
`Server Component`는 Next.js에서 서버 측에서 렌더링되는 컴포넌트이며, 브라우저는 클라이언트 측 자바스크립트를 사용하지 않고도 HTML과 데이터를 가져오며, 페이지의 초기 로드 속도와 성능이 향상된다.

- 기본적으로 app 폴더 하위의 모든 컴포넌트는 `서버 컴포넌트`이다.

#### :pushpin: 1-1) Server Component의 특징
1. **서버에서만 동작** 
  - 서버 측에서만 실행되며 클라이언트로 자바스크립트가 전송되지 않음. (브라우저에 console.log 안찍혀~)
2. **더 빠른 렌더링** 
  - 자바스크립트 번들 크기를 줄이고, 초기 로드 시간을 단축할 수 있음.
3. **데이터 가져오기 최적화** 
  - 데이터를 서버에서 직접 가져올 수 있어, 클라이언트-서버 간의 데이터를 주고받는 시간을 줄일 수 있음.
4. **인터랙티브한 기능 제한** 
  - 클라이언트에서의 상태 관리나 이벤트 처리가 필요 없는 부분에 적합.

#### :pushpin: 1-2) Server Component 예시
앞서 말했듯 app 폴더 하위 모든 컴포넌트는 `서버 컴포넌트`이다.

- 아래에서 console.log가 실행되면 브라우저가 아닌 터미널에서 console.log가 출력된 걸 확인할 수 있다.
- 서버 컴포넌트에서 alert을 출력하려하면 `Error: alert is not defined` 에러를 출력함

```tsx
// src>app>page.tsx
export default function Home() {
  console.log("여기는 어디일까요?");

  return (
    <div className="p-8">
      안녕하세요! Next.js 입니다!
    </div>
  );
}
```

### :two: Client Component란?
`Client Component`는 브라우저에서 클라이언트 측에서 실행되는 컴포넌트이며, 인터랙션, 상태 관리, 이벤트 처리 등 클라이언트의 동적 동작이 필요할 때 주로 사용된다.

#### :pushpin: 2-1) Client Component의 특징
1. **브라우저에서 동작**
  - 자바스크립트가 클라이언트에 번들되어 전송되고 실행됨.
2. **상태 및 이벤트 처리**
  - React의 useState, useEffect 같은 훅을 사용할 수 있어, 동적인 UI 구현이 가능.
3. **상호작용을 위한 컴포넌트**
  - 버튼 클릭, 사용자 입력, 드롭다운 메뉴 등 사용자와 상호작용이 필요한 부분에서 주로 사용.

#### :pushpin: 2-2) Client Component 예시
상단에 `'use client'` 선언하면 해당 컴포넌트는 클라이언트에서만 렌더링되도록 지정됨

- useState, useEffect 사용 가능~!

```tsx
"use client";

import { useEffect } from "react";

const ClientExample = () => {
  useEffect(() => {
    console.log("난 클라이언트 컴포넌트!");
  }, [])
  return (
    <div>
      클라이언트 컴포넌트입니다!
    </div>
  )
}

export default ClientExample

```

### :three: 차이점을 알아보자!
- `Server Component`는 성능 최적화에 중점을 둔다.
- `Client Component`는 인터랙티브한 기능에 중점을 둔다.

| ---- | ------------------| -----------------|
| 특징	| Server Component	| Client Component | 
| 실행 위치	| 서버에서만 실행	| 클라이언트(브라우저)에서 실행 | 
| 자바스크립트 번들	| 브라우저로 자바스크립트 번들이 전송되지 않음	| 자바스크립트가 클라이언트에 번들되어 전송됨 | 
| 인터랙션	| 인터랙션 불가능	| 상태 관리 및 이벤트 처리 가능 | 
| 성능	| 초기 로드 성능 최적화	| 동적 UI 구현에 적합 | 
| 사용 사례	| 정적 콘텐츠, 서버에서 데이터를 처리하는 페이지	| 사용자 입력 처리,상태 변화가 필요한 동적 페이지 | 

### :four: 그럼 언제 사용해야 할까?
1. **Server Component** 
  - 서버에서 데이터 처리와 정적 콘텐츠 렌더링이 주 목적일 때 적합. 
  - 데이터베이스에서 데이터를 불러와 정적인 페이지로 표시하거나 SEO 최적화가 필요할 때 유리함.

2. **Client Component** 
  - 사용자의 상호작용이 중요한 동적 UI 구현에 적합. 
  - 예를 들어, 폼 입력, 클릭 이벤트, 실시간 상태 업데이트가 필요한 부분에서 사용됨.

### :five: 둘 다 쓰려면 어떻게 해?
Server & Client Component 둘 다 사용하려면 방법은 아래와 같은 예시로 적용이 가능하다.

```tsx
// src > app > page.tsx
import Button from "@/components/Button";
export default function Home() {
  return (
    <div className="p-8">
      안녕하세요! 넥스트강의 입니다!
      <section>
        <h1>제목</h1>
        <p>내용</p>
        <ul>
          <li>항목1</li>
          <li>항목2</li>
          <li>항목3</li>
        </ul>
      </section>
      <Button />
    </div>
  );
}

// src > components > Button.tsx
"use client";

import React from "react";

const Button = () => {
  return (
    <button
      onClick={() => {
        alert("안녕하세요!");
      }}
    >
      클릭
    </button>
  );
};

export default Button;
```


### :fire: 마무리
`Next.js`의 Server Component와 Client Component에 대해 알아보았다. 각 컴포넌트의 사용법을 이해하고 프로젝트를 진행할 때 적절히 결합해서 사용하도록 해야겠다!