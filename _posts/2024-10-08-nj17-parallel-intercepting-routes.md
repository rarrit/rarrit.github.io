---
title: "[Next.js] Parallel Routes, Intercepting Routes 이란?"
date: 2024-10-09
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
  - routes
  - parallel routes
  - intercepting routes
author_profile: true
sidebar_main: true
---

## :ledger: Next.js의 가장 핵심적인 기능인 Caching에 대해 알아보자
여러가지 정보를 한번에 보여주는 탭이나 같은 레이아웃에서 다른 레이아웃을 보여주는 모달의 경우 사용되는 `Parallel Routes`, `Intercepting Routes`의 개념과 원리에 대해 알아보자!

### :one: Parallel Routes (평행 라우트)
Parallel Routes는 여러 경로를 동시에 렌더링할 수 있는 기능이다.

#### :pushpin: 1-1) 동작 원리
1. 여러 컴포넌트를 동시에 렌더링
  - `Parallel Routes`를 사용하면 한 페이지 안에서 여러 다른 컴포넌트를 동시에 렌더링할 수 있다. 
  - 예를 들어, 대시보드 페이지에서 사용자 정보와 통계 데이터를 동시에 보여줄 수 있다.
2. 경로의 유연성
  - 경로를 병렬로 설정할 수 있어, 같은 페이지에서 서로 다른 콘텐츠를 처리할 수 있다. 
  - 이를 통해 페이지를 보다 동적이고 반응적으로 만들 수 있다.

### :two: Intercepting Routes
`Intercepting Routes`는 사용자가 특정 경로로 접근할 때, 중간에 다른 페이지로 리디렉션하거나 다른 콘텐츠를 보여줄 수 있는 기능이다. 

#### :pushpin: 2-2) 동작 원리
1. 특정 조건에 따른 라우팅
  - Intercepting Routes는 사용자가 특정 경로에 접근할 때, 사전 정의된 조건에 따라 다른 페이지로 리디렉션할 수 있다. 
  - 예를 들어, 사용자가 로그인하지 않은 경우 로그인 페이지로 이동시키는 것이 가능하다.

2. 유연한 네비게이션 경험
  - 사용자에게 필요한 정보나 콘텐츠를 제공하기 위해 라우트를 쉽게 변경할 수 있다. 
  - 이를 통해 페이지 전환 시 더욱 매끄러운 경험을 제공한다.

### :three: Parallel & Intercepting Routes 사용 예시 (Modal)

#### :pushpin: 2-1) [Parallel] 디렉토리 세팅
아래와 같은 구조에서 모달을 예시로 만들어보자.

- app/(root) 세그먼트 생성
  - app/(root)/product
  - app/(root)/cart

#### :pushpin: 2-2) [Parallel] Modal 생성
app/(root)/@modal/page.tsx 생성  

```tsx
const ModalTest = () => {
  return <div>ModalTest</div>;
}
export default ModalTest;
```

#### :pushpin: 2-3) [Parallel] layout.tsx 설정
- app/(root)/layout.tsx 에서 타입 추가
- 아래의 설정을 완료하면, app/(root) 경로에서 ModalTest 컴포넌트를 확인할 수 있음

```tsx
// app/(root)/layout.tsx
export default function Layout ({
  children,
  modal, // modal 추가
}: Readonly<{
  children: React.ReactNode;
  modal: React.ReactNode; // modal 추가
}>){
  return (
    <>      
      { children }

      {/* modla 추가 */}
      { modal } 
    </>
  )
}
```

#### :pushpin: 2-4) [Parallel] 기존 Product page와 평행시키기, default 설정

- 기존에 생성한 `app/(root)/product/[id]/page.tsx`와 동일하게 `app/(root)/@modal/product/[id]/page.tsx`을 생성한다.
- 위와 같이 세팅할 경우 기존 /product/[id]/page.tsx 경로에 들어갓을 때 @modal/... 컴포넌트를 확인할 수 있다.
- 현재 product만 세팅해줘서 cart 경로에 들어가면 404 페이지가 나오는 걸 확인할 수 있다.
  - product는 평행 라우트로 연결했지만, cart는 연결하지 않아서 위와 같이 적용됨
  - 위와 같은 문제를 해결하기 위해 @modal/default.tsx 를 생성해서 해결할 수 있음

```tsx
// app/(root)/@modal/product/[id]/page.tsx
const ProductModalTest = () => {
  return <div>Product Modal Test</div>
}
export default ProductModalTest;

// app/(root)/@modal/default.tsx
export default function Default(){
  return null;
}
```

#### :pushpin: 2-5) [Parallel] 중간 점검
1. 초기 app/(root)/@modal/page.tsx 생성하고, layout에서 설정해주어서 (root) 세그먼트에 있는 페이지에서는 기존 @modal/page.tsx의 컴포넌트를 확인할 수 있음
2. 평행 라우팅을 해준 product 페이지에서는 `app/(root)/@modal/product/[id]/page.tsx`의 컴포넌트를 확인할 수 있음
3. 아무 설정을 해주지 않은 cart 페이지에서는 `app/(root)/@modal/default.tsx` 디폴트 설정을 해주어 아무런 컴포넌트를 확인할 수 없음
  - 디폴트 설정을 해주지 않으면 404 에러 노출됨


위와 같이 layout.tsx 에서 세팅하면 (root) 세그먼트 내에서 modal을 확인할 수 있다.

#### :pushpin: 2-6) [Intercepting] 라우트 인터셉트
1. 기존 인터셉트 하고 있는 라우트 `app/(root)/@modal/product`를 `app/(root)/@modal/(.)product` 으로 변경한다.
2. 위와 같이 적용할 경우 product 라우트 세그먼트 안으로 들어가는 라우팅을 인터셉트한다.
3. 설정을 완료하고 product/[id] 경로로 들어가면(제품 클릭) url이 변경되는걸 확인할 수 있다.
  - 기존 `@modal/page.tsx`의 컴포넌트가 노출되었는데, `@modal/(.)product/[id]/page.tsx` 컴포넌트만 변경된걸 확인할 수 있음
  - 즉, 기존의 컴포넌트를 인터셉트해서 치환함
  - 이후 새로고침을 하면, product/[id] 경로의 페이지로 이동되는 것을 확인할 수 있음
4. [3] 에서 새로고침을 해야 상세페이지로 이동되는데 이럴 경우 모달에서 상세페이지로 이동할 수 있게 버튼을 생성해줘야함
  - product/[id]/view/page.tsx 생성해줌
  - redirect() 함수를 사용해서 버튼을 클릭했을 때 해당 경로에 맞는 상세페이지로 이동할 수 있게해줌
  - 그렇지만 사용자가 새로고침을 직접 하게 할 수 없기에 유도하기 위해 새로고침 버튼을 생성해줌
5. (.)product/[id]/view/reload-button.tsx 파일을 생성함
  - `"use client"` 선언
  - button onClick={`window.location.reload()`} 를 넣어줘서 새로고침 버튼을 연결해줌
6. (.)product/[id]/view/ProductView.tsx
  - 해당 컴포넌트에서는 `return redirect(`/product/${id}`)` 을 통해 해당 상세페이지로 이동하게 해줌


```tsx
// product/[id]/view/page.tsx
import { redirect } from "next/navigation";

type Props = {
  params: {
    id: string;
  }
}

const ProductView = ({ params } : Props ) => {
  const id = parseInt(params.id, 10);
  return redirect(`/product/${id}`); // product/[id]로 리다이렉트해줌
}

export default ProductView;

// 리스트에서 설정해줘야할 것 => 기존 링크의 href값에 view를 추가해줌
<Link href={`/product/${product.id}/view`}/> 

```

### :fire: 마무리
어렵다.. 어려워..