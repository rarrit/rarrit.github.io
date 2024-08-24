---
title: "React Router Hook과 Context를 함께 사용할 때 주의할점"
date: 2024-08-24
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - troubleshooting
  - til
tags:
  - context API
  - react router
  - useLocation
  - useNavigate
author_profile: true
sidebar_main: true
---

## :ledger: useLocation, useNavigate를 context에서 관리하려했을 때 알게된 내용이다.

context API 와 Router hook을 사용하던 중 발생한 에러와 주의해야할 점을 정리한 글입니다.

### :one: 개요

이번에 prop drilling으로 프로젝트를 진행 후 context API를 사용하여 리팩토링을 진행하던 중 router hook을 사용하게 되었다. `useNavigate()`를 사용하여, path값과 선택된 데이터의 상태값을 전달하는 과정에만 사용했지만 이후 복습하던 중 `useNavigate`도 context에서 관리해서 재사용 가능하게 사용하면 어떨까란 생각에 테스트 중 에러를 겪게 되었다.

### :two: Router 훅은 Router 컴포넌트 내에서만 사용할 수 있다.

첫 번째 테스트는 컴포넌트 내에서 사용하고 있던 `useNavigate`, `useLocation`을 context에 옮겼을 때 에러가 노출되었다.

#### :pushpin: 2-1) 작성 코드

```jsx
import { createContext } from "react";
import { useLocation, useNavigate } from "react-router-dom";

export const pokemonContext = createContext();

export const PokemonProvider = ({ children }) => {
  const isLocation = useLocation();
  const isNavigate = useNavigate();

  return (
    <pokemonContext.Provider value={{ isLocation, isNavigate }}>
      {children}
    </pokemonContext.Provider>
  );
};
```

#### :pushpin: 2-2) 에러 노출

아래의 에러를 직역하자면 "useLocation()은 `<Router>` 구성 요소의 컨텍스트에서만 사용할 수 있습니다." 이다.
![router-error-message](https://github.com/user-attachments/assets/bf63c955-bd26-412a-8352-4173aae8cc7f)

#### :pushpin: 2-3) 에러 원인

에러의 원인은 `useLocation`과 `useNavigate`는 React Router가 제공하는 훅으로, `<Router>` 컴포넌트가 렌더링된 트리 내에서만 작동이 가능하다는 것이다. context API를 사용하며 Provider가 Router를 감싸고 있어서 사용이 불가능했던 것. 쉽게 생각하면 라우터 훅인데 왜 라우터 밖에서 쓰려함? 이었다. ㅋㅋ... 아래는 App.jsx의 코드다.

```jsx
function App() {
  return (
    <PokemonProvider>
      <Router />
    </PokemonProvider>
  );
}
```

#### :pushpin: 2-4) 해결 과정

그렇다면 단순하게 생각해서 Provider가 Router 내부에 있으면 되는 것 아닌가? 라고 생각하며 구조를 변경했는데, 또 다른 의문점이 생겼었다. 의문점는 아래와 같다.

1. 라우터가 `Provider` 상위에 있게되면, 라우터의 변경사항이 `Provider`를 여러번 렌더링하게 할 수 있게되어버림 그렇게되면 `Context`값이 불필요하게 재설정될 수 있을 것 같았음
2. 라우터의 상태나 URL 변경에 따라 `Provider`가 변경되면 컨텍스트 값의 일관성이 깨져버리는(?) 공부하며 배웠던 [SSOT](https://rarrit.github.io/development/til/dev-ssot/)원칙에 위배되는 느낌이었다. 즉 라우터가 변경될 때 `Context`의 상태도 영향을 받는다면, 상태의 일관성이 깨지는 것임
3. 매번 라우터의 URL이 변경될 때 마다 `Provider`가 영향을 받는다면 성능적으로도 문제가 생길 것 같았다.

#### :pushpin: 2-5) 결론

또 다른 방법으로 Provider를 하나 더 생성했는데, 이 모든 과정의 목표가 재사용 가능하게 사용하려했던 거라 Provider를 하나 더 생성함으로써 app.jsx에서 사용될 컴포넌트를 감싸는 번거로움이 생겨 이것 또한 일을 하나 더 만드는 느낌이 되어버렸다. 결과적으로 선택한 방법은 **_Router Hook은 필요한 상황에 직접 컴포넌트 내에서 정의하여 사용하자_** 였다.

### :fire: 마무리

context API를 배우면서 재사용할 수 있는 코드를 한 곳에 합치려다 생겼던 일인데, 본의 아니게 Router Hook과 context에 대해 공부하게 된 것 같다. 위의 과정은 단지 어떤 방식으로 생각하다가 생긴 오류들과 해결하려했던 내용을 주저리주저리 작성했지만 요약하면 **_Router Hook은 필요한 상황에 직접 컴포넌트 내에서 정의하여 사용하자_**였다. (오늘은 주말이라 담주에 튜터님께 물어보고 업데이트 예정) 다양한 에러들을 접하면서 누군가에게는 당연할 수 있어도 아직 허접인 나는 이유를 계속 물어보게 되는 것 같다. 앞으로도 파이팅...
