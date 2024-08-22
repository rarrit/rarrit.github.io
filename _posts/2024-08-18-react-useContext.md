---
title: "리액트(react.js) useContext란 무엇인가?"
date: 2024-08-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
tags:
  - react
  - useContext
  - react hooks
author_profile: true
sidebar_main: true
---

## :ledger: 리액트의 useContext에 대해 알아보자

리액트의 훅 중 하나인 `useContext`는 컴포넌트 트리에서 전역적인 상태나 데이터에 접근할 때 유용한 훅이다. 자세히 알아보자!

### :one: useContext란 무엇인가?

`useContext`는 리액트에서 컴포넌트 간에 데이터를 쉽게 공유할 수 있게 해주는 훅이다. `Context`를 사용하면 부모 컴포넌트로부터 깊이 중첩된 자식 컴포넌트까지 데이터를 직접 전달할 수 잇음 (prop drilling 방지!)

- 리액트에서 `Context` 객체를 쉽게 사용할 수 있도록 해주는 훅이다.
- `Context`는 글로벌하게 데이터를 관리하고, 이 데이터를 컴포넌트 트리 안의 어느 컴포넌트에서든지 접근이 가능하도록 해줌

#### :pushpin: 1-1) Context란?

`Context`는 리액트에서 <u>전역 상태를 관리하기 위한 도구</u>이며 일반적으로 상태를 관리할 때는 부모 컴포넌트에서 자식 컴포넌트로 `props`를 통해 데이터를 전달하지만 이 방식은 컴포넌트 트리가 깊어질수록(prop drilling) 복잡해진다. `Context`를 사용하면 컴포넌트 트리에서 특정 데이터를 전역적으로 관리할 수 잇음

#### :pushpin: 1-2) context API 필수 개념

- `createContext` : context 생성
- `useContext` : context를 구독하고 해당 context의 현재 값을 읽음
- `Provider` : context를 하위 컴포넌트에게 전달함

### :two: useContext는 언제 사용함?

- **_전역 상태 관리_**
  - 애플리케이션의 전역 상태를 관리할 때 유용함.
- **_프롭 드릴링 ㅂㅂ_**
  - 중첩된 컴포넌트들 사이에서 데이터를 전달해야 할 때, `useContext`를 사용하면 중간에 있는 컴포넌트들이 `props`로 데이터를 전달할 필요 없이 간단하게 구현이 가능함

### :three: useContext의 사용법

#### :pushpin: 3-1) 기본 사용법

`useContext`를 사용하기 위해선 먼저 `Context`객체를 생성해야함 그 후 `Context`를 제공(`Provider`)하고, 자식 컴포넌트에서 `useContext`훅을 사용해 `Context`값을 가져옴 아래의 순서를 살펴보자.

- Context 생성

```jsx
const FirstContext = createContext();
const FirstProvider({ children }) {
  const value = "My First Context";
  return (
    <>
      <FirstContext value={value}>
        {children}
      </FirstContext>
    </>
  )
}
```

- Context 사용

```jsx
function 컴포넌트임() {
  // useContext 훅을 사용하여 Context 값을 가져옴
  const contextValue = useContext(FirstContext);
  return <div>{contextValue}</div>;
}

function App() {
  return (
    <MyProvider>
      <컴포넌트임 />
    </MyProvider>
  );
}

export default App;
```

### :four: useContext 장단점

#### :pushpin: 4-1) 장점

- **_간편한 전역 상태 관리_**
  - `Context`를 통해 컴포넌트 트리 전체에 걸쳐 전역 상태를 쉽게 관리할 수 있음
- **_Props Drilling 방지_**
  - `useContext`를 사용하면 중간 컴포넌트를 거쳐 `props`를 전달할 필요가 없어 코드를 간결하게 유지할 수 있음
- **_간결한 코드_**
  - 상태나 데이터를 쉽게 공유하고 접근할 수 있어 코드의 가독성이 향상됨

#### :pushpin: 4-2) 단점

- **_재사용성 저하_**
  - `Context`는 특정 컴포넌트 트리에 종속될 수 있어, 해당 컴포넌트의 재사용성이 떨어질 수 있음
- **_테스트가 어려울 수 있음_**
  - 전역 상태를 사용하기 때문에, 해당 컴포넌트를 독립적으로 테스트하기 어려울 수 있음
- **_복잡성 증가_**
  - 애플리케이션이 커지면 `Context`의 관리가 복잡해질 수 있음. 이럴때는 상태 관리 라이브러리를 고려해야함

### :fire: 마무리

`useContext`를 알아보았는데 쉽게 이해해보면 큰 규모의 프로젝트라 치고 컴포넌트 in 컴포넌트도 정말 많다고 가정했을 때 state,props 등 관리하기 어려울 것 같다는 생각이 들었다. 이럴 경우 `useContext`를 사용하여 전역에서 데이터를 관리하고 사용하는 컴포넌트에서만 사용한다면 중첩된 컴포넌트에서 데이터를 간편히 전달할 수 있을 것 같다. `useContext = 전역에서 관리함 ㅅㄱ` 이렇게 이해하고 이후에 상태 관리 라이브러리(예: redux)를 배우게 될 때 어떤 차이점이 있는지 알아봐야겠다.
