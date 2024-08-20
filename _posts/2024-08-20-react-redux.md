---
title: "리액트(react.js) Redux이란 무엇인가?"
date: 2024-08-20
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
  - redux
author_profile: true
sidebar_main: true
---

## :ledger: 리액트의 리덕스(Redux)에 대해 알아보자

리액트의 전역상태 라이브러리인 `Redux`에 대해 알아보자!

### :one: 리덕스가 필요한 이유

#### :pushpin: 1-1) 상태 관리

리액트에서 `상태(state)`는 중요한 데이터나 UI 상태 등을 의미한다. 컴포넌트 내부에서 관리되며 여러 컴포넌트에서 상태를 공유해야하는 경우가 많음 이럴 경우 상태관리가 어려워질 수 있고 전역에서 관리해주기 위한 `Redux`를 사용해볼 수 있음

#### :pushpin: 1-2) useState의 불편함

어떤 컴포넌트에서 생성한 `state`를 다른 컴포넌트로 보고자 할 때 <u>props를 통해 부모 컴포넌트에서 자식 컴포넌트</u>로 값을 보내준다. 그런데 관리해야할 상태는 한 개이고 그것을 바라보는 컴포넌트가 정말 많을 경우엔? 아래의 예시를 보자.

1. 컴포넌트에서 컴포넌트로 `state`를 보내기 위해서는 반드시 **_부모-자식_** 관계가 되어야 한다.
2. 조부모 컴포넌트에서 손자 컴포넌트로 값을 보낼 때 반드시 부모 컴포넌트를 거쳐야하며, <u>부모 컴포넌트에 값이 필요없어도 불필요하게 거쳐</u>가야함.
3. 자식 컴포넌트에서 부모 컴포넌트로 값을 보낼 수 없음
4. 컴포넌트가 많아질 경우 `props drilling`을 경험할 수 있음

#### :pushpin: 1-3) Local State, Global State

지금까지 지역상태를 많이 사용해왔지만 Redux를 통해 전역상태를 사용하여 컴포넌트가 어디에 위치하든 상관없이 State를 불러와서 사용할 수 있음

- **_Local State (지역상태)_**
  - 컴포넌트에서 useState를 이용해서 생성한 state이다. 좁은 범위 안에서 생성된 state
- **_Global State (전역상태)_**
  - Global state는 컴포넌트에서 생성되지 않음. 중앙화 된 특별한 곳에서 State들이 생성됨 (`중앙 state 관리소`)

#### :pushpin: 1-4) Context API 차이

- **_성능 최적화_**
  - `Context API`는 Provider 하위 모든 컴포넌트를 `리렌더링`하게 할 수 있으며 상태가 변경될 때 마다 관련된 모든 컴포넌트가 불필요하게 업데이트 하는 것을 막기 위해 최적화가 필요함. `Redux`는 <u>상태 변경 시 관련된 컴포넌트만 선택적으로 업데이트</u>할 수 있어 성능 관리가 용이함
- **_상태 로직의 중앙화 일관성_**
  - `Redux`는 애플리케이션의 상태를 하나의 저장소(`store`)에 저장함. 이로 인해 상태 로직이 중앙에서 관리되어 더 일관성 있고 예측 가능한 상태로 변경이 가능해짐
- **_강력한 미들웨어와 개발 도구_**
  - `Redux`는 다양한 미들웨어를 지원하여 비동기 작업,로깅,상태 변경에 대한 추가 처리 등 복잡한 기능을 구현할 수 있음

### :two: Redux의 주요 개념

#### :pushpin: 2-1) Store

- `Store`는 애플리케이션의 전역 상태를 저장하는 공간이다. 하나의 스토어만 존재하며, 이 스토어에 있는 상태는 <u>앱의 모든 컴포넌트에 접근이 가능</u>함
- `Store`는 세 가지 주요 메서드를 제공함
  1. `GetState()` : 현재 스토어의 상태를 변경함
  2. `dispatch(action)` : 상태를 변경하기 위해 액션을 리듀서에 전달함
  3. `subscribe(listener)` : 상태가 변경될 때마다 호출될 콜백 함수를 등록함

#### :pushpin: 2-2) Action

- `Action`은 상태에 변화를 일으키기 위해 발생하는 이벤트임. **_Type_**, **_Payload_**로 이루어져 있으며, 상태를 어떻게 변경할 지 설명함

```jsx
{
  type: 'INCREMENT',
  payload: 1
}
```

#### :pushpin: 2-3) Reducer

- `Reducer`의 인자는 state, action으로 들어가져 있음
- `Reducer`는 Action을 처리하여 상태를 실제로 변경하는 함수이다. 이전 상태와 액션을 입력으로 받아서, 새로운 상태를 변환함

```jsx
const counterReducer = (state = 0, action) => {
  if (action.type === "INCREMENT") {
    return state + action.payload;
  }
  return state;
};
```

#### :pushpin: 2-4) Dispatch

- `Dispatch`는 Action을 Reducer에 전달하여 상태를 업데이트하도록 하는 함수이다. `dispatch(action)`을 통해 액션을 리듀서에 전달함

#### :pushpin: 2-5) Selector

- `Selector`는 Store에서 상태를 가져오는 함수임. 컴포넌트에서 필요한 상태를 선택해서 사용할 수 있음

```jsx
const count = useSelector((state) => state.count);
```

#### :pushpin: 2-6) Action Creators

- `Action Creators`는 특정 액셔 객체를 생성하는 함수로 액션 객체는 type,payload와 같은 형식으로, 액션 타입과 데이터를 포함한다.

```jsx
const increment = (value) => {
  return {
    type: "INCREMENT",
    payload: value,
  };
};
```

#### :pushpin: 2-7) Action Values

- `Action Values`는 액션의 `type`값으로, 보통 상수로 정의된다. 이렇게 하면 오타로 인한 오류를 줄일 수 있으며, 여러 곳에서 동일한 액션 타입을 사용할 때 유용함

```jsx
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });
```

#### :pushpin: 2-8) Payload

- `Payload`는 액션 객체에서 상태를 변경하기 위해 전달되는 데이터이며 액션과 함께 전달되어 리듀서에서 사용된다.

```jsx
const addTodo = (text) => ({
  type: "ADD_TODO",
  payload: { text },
});
```

### :three: Redux 동작 과정

- **_Dispatch_**
  - 컴포넌트에서 특정 이벤트가 발생하면 해당 이벤트에 맞는 액션을 디스패치함
- **_Reducer_**
  - 디스패치된 액션을 리듀서가 받아 현재 상태와 액션 타입에 따라 새로운 상태를 계산함
- **_store 업데이트_**
  - 리듀서가 반환한 새로운 상태가 스토어에 저장됨
- **_컴포넌트 업데이트_**
  - 상태가 변경되면, 스토어에 구독된 컴포넌트들이 변경된 상태를 받아 UI를 다시 렌더링함

### :four: Middleware

- `Middleware`는 Action이 Reducer에 도달하기 전에 중간에서 가로채서 특정 작업을 수행할 수 있게 해줌
- 대표적인 예로 redux-thunk와 redux-saga가 있음
- 비동기 작업을 관리하거나 액션을 로깅하는 등의 작업할 때 용이함

### :five: Redux의 장단점

#### :pushpin: 5-1) 장점

- 상태 관리를 체계적이고 예측 가능하게 만들어줌
- 컴포넌트 간 상태 공유가 쉬워짐
- 디버깅이 용이함

#### :pushpin: 5-2) 단점

- 복잡한 설정이 필요할 수 있음
- 작은 애플리케이션에서는 오히려 과도한 도입일 수 있음

### :six: Ducks 패턴

- `Ducks 패턴`은 Redux 코드를 모듈화하는 방법 중 하나로, 관련된 액션 타입, 액션 생성자, 리듀서 등을 하나의 파일에 모아 관리하는 패턴이다. 이는 코드의 조직화와 유지보수를 쉽게 만들어줌
- Ducks 패턴의 주요 규칙
  - 모든 관련 코드를 한 파일에 모아둠: 액션 타입, 액션 생성자, 리듀서를 한 곳에서 관리하여 파일이 독립적으로 동작할 수 있게 한다.
  - 디폴트 익스포트로 리듀서를 내보냄: 파일에서 리듀서를 기본 내보내기로 설정하여 다른 곳에서 쉽게 사용할 수 있게 한다.
  - 액션 생성자와 액션 타입을 명시적으로 내보냄: 필요한 경우 액션 생성자나 액션 타입을 다른 파일에서 사용할 수 있도록 내보낸다

### :fire: 마무리

리액트를 공부하면서 접한 `Redux`는 가장 이해가 어려운 부분이었다. 예제를 진행하면서 파일도 많이 왔다갔다 한 것 같고.. 혼자 기억하고 작성하려 하면 대부분 기억이 나지 않아서 다시 강의를 보게되는.. `Redux`를 사용하는 이유에 대해는 이해는 했지만 아직 손에 안익는다. 다음 공부 내용은 기존 `Redux`구조를 좀 더 쉽게 해주는 `Redux Toolkit`인데, 일단 배울 수 있는걸 배우고 사용해보면서 손에 익혀야겠다..
