---
title: "[Next.js] Next.js 페이지 이동 및 라우터(Router) 메서드를 알아보자"
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
  - routing
author_profile: true
sidebar_main: true
---

## :ledger: Next.js에서 페이지 이동 및 라우터 메서드를 알아보자
react에서 배운 RRD와 같이 페이지를 이동하는 기능이 `Next.js` 에서도 제공이 된다. 또한 Router의 메서드에 대해 알아보도록 하자!

### :one: 페이지 이동 방법
Next.js에서 페이지를 이동시킬 수 있는 방법에 대해 알아보자!

#### :pushpin: 1-1) a tag

- 순수 HTML 요소
- 완전한 새 페이지로 전환을 원할 때 사용 (페이지는 완전히 새로고침)
- 빈 화면이 보일 수 있으므로 주의해야함
- 사용하지말고 Link, Router를 사용하면 됨

#### :pushpin: 1-2) Link

1. Link 태그는 a 태그를 만들어내기에 SEO에 유리함
2. 클릭 즉시 페이지가 이동됨
3. SPA 처럼 동작함

#### :pushpin: 1-3) Router
3. `Router (useRouter)`
  - 코드 최상단에 `"use client"` 삽입해야함
  - onClick 같은 이벤트 핸들러에서 사용함
  - 클릭 후 로직의 순서에 따라 실행하므로 즉시 이동되지 않음

```tsx
"use client";

import { useRouter } from "next/navigation";

type Props = {
  params: {
    contactId: number;
  }
}

const ContactPage = ({ params } : Props) => {
  const router = useRouter();
  const pageId: number = Number(params.contactId);

  const handleMovePrevPage = () => {    
    if(pageId <= 1) alert("이전 페이지가 없습니다.");      
    else router.push(`/contact/${pageId - 1}`);
  }
  const handleMoveNextPage = () => {
    router.push(`/contact/${pageId + 1}`);
  }

  return (
    <div>
      현재 페이지는 Contact Page-{pageId} 입니다.
      <hr/>
      <button onClick={handleMovePrevPage}>이전 페이지로 이동</button> | 
      <button onClick={handleMoveNextPage}>다음 페이지로 이동</button>
    </div>
  )
}

export default ContactPage
```

### :two: 라우터 메서드
라우터 메서드를 알기 위해서는 웹 브라우저의 `history stack`을 이해해야한다.

- history stack은 방문자의 페이지 방문 순서를 기록하는 시스템임
- 웹 사이트 내에서 페이지를 이동할 때, 페이지의 URL이 history stack에 추가됨
- 예를들면 뒤로 가기, 앞으로 가기 했을 때 이동할 수 있는 이유임

#### :pushpin: 2-1) router.push 
- 새로운 URL을 히스토리 스택에 추가함
- 사용자가 router.push로 페이지를 이동하면, 이동한 페이지의 URL이 히스토리 스택의 맨 위에 쌓임
- 이후에 사용자가 뒤로 가기 버튼을 클릭하면, 스택에서 가장 최근에 추가된 URL로부터 이전 페이지(URL)로 돌아감

```tsx
"use client";
import { useRouter } from 'next/router';

const HomePage = () => {
  const router = useRouter();

  const handleMoveAboutPage = () => {
    router.push('/about');
  };

  return <button onClick={handleMoveAboutPage}>About 페이지로 이덩</button>;
};
```

#### :pushpin: 2-2) router.replace
- 현재 URL을 히스토리 스택에서 새로운 URL로 대체함
- 현재 페이지의 URL이 새로운 URL로 교체되며, 뒤로 가기 버튼을 클릭했을 때 이전 페이지로 이동하지만, 교체된 페이지로는 돌아갈 수 없음
- 현재 페이지를 히스토리에서 완전히 대체함

```tsx
"use client";
import { useRouter } from 'next/router';

const HomePage = () => {
  const router = useRouter();

  const handleMoveAboutPage = () => {
    // 함수가 실행되면 about 페이지로 이동 후 뒤로 가기를 하면 홈으로 가짐
    router.replace('/about');
  };

  return <button onClick={handleMoveAboutPage}>About 페이지로 이덩</button>;
};
```

#### :pushpin: 2-3) router.back
- 사용자를 히스토리 스택에서 한 단계 뒤로 이동시킴
- 브라우저의 `뒤로 가기` 버튼과 동일한 효과를 내며, 이전 방문했던 페이지로 돌아감

```tsx
"use client";
import { useRouter } from 'next/router';

const HomePage = () => {
  const router = useRouter();

  const handleMoveAboutPage = () => {    
    router.push('/about');
  };

  const handleMoveBack = () => {
    router.back()
  }

  return (
    <>
      <button onClick={handleMoveBack}>뒤로가기</button>
      <button onClick={handleMoveAboutPage}>About 페이지로 이덩</button>
    </>
  );
};
```

#### :pushpin: 2-4) router.reload 
- 현재 페이지를 새로 고침함
- 히스토리 스택에 영향을 미치지 않음. 페이지의 `데이터를 최신 상태로 업데이트` 하고 싶을 때 사용함

```tsx
"use client";
import { useRouter } from 'next/router';

const HomePage = () => {
  const router = useRouter();

  const handleRefreshPage = () => {    
    router.reload();
  };

  return (
    <>
      <button onClick={handleMoveBack}>최신 데이터 확인</button>
    </>
  );
};
```


### :fire: 마무리
Next.js의 페이지 이동하는 방법과 `history stack`에 대해 알 수 있었다. Next.js는 이런 유용한 기능을 제공하고 있어서 초기 세팅할 때 편리한 것 같다. 이번에 배운 내용을 기반으로 적재적소에 알맞게 사용할 수 있도록 하자!