---
title: "리액트(react.js) State란 무엇인가?"
date: 2024-08-16
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
  - state
author_profile: true
sidebar_main: true
---


## :ledger: 리액트의 State에 대해 알아보자
State는 아무래도 동적으로 변하는 데이터를 저장하고 업데이트 하다보니 리액트에서 생명을 불어넣는 가장 중요한 개념이 아닐까 싶다! Props, Component, State 입문에서 배웠지만 이 3가지의 개념을 확실히 이해해야 앞으로 문제가 없을 것 같은 느낌아님느낌's'..!

### :one: State란 무엇인가?
- `state`는 리액트 컴포넌트 내에서 ***동적***으로 변하는 데이터를 저장하고 관리하는 객체다.
- `state`를 통해 컴포넌트는 사용자의 입력,API 호출 결과 등 외부 요인에 의해 ***변경될 수 있는 데이터***를 관리할 수 있다.
- `state`가 변경되면, 리액트는 해당 컴포넌트와 하위 컴포넌트들을 ***자동으로 다시 랜더링***하여 UI가 변경된 데이터를 반영한다.

### :two: State의 특징
- `state`는 특정 컴포넌트에 로컬하며, <u>해당 컴포넌트에서만 직접 접근하고 변경</u>할 수 있다. 부모 컴포넌트에서 자식 컴포넌트로 `props`를 통해 전달할 수 있지만, 직접적인 접근은 불가능함.
- `state`의 변경은 ***비동기적***으로 이루어진다. 따라서 `state`가 바로 업데이트되지 않을 수 있으며, <u>리액트는 여러 상태 변경을 최적화하여 한 번에 처리</u>한다.
- `state`는 직접 변경할 수 없으며, 항상 `setState` 함수를 사용하여 업데이트 해야 한다. 직접 변경하려고 시도하면 리액트가 이를 감지하지 못해 UI가 재랜더링되지 않는다.

### :three: State 사용 방법
리액트의 함수형 컴포넌트에서 `State`를 사용하기 위해서는 `useState`훅을 사용하는데, 초기 상태값을 배개변수로 받아서, 현재 상태값과 상태를 업데이트하는 함수를 반환한다. 이 내용은 리액트 훅을 공부하며 다시한번 글을 작성해보려한다. 아래의 코드는 vite로 리액트를 생성하면 기본적으로 생성되어있어 예시로 작성해보았다.

- useState를 사용하려면 해당 컴포넌트 내 상단에 `import {useState} from 'react'`를 작성해줘야한다.
- 아래의 코드에서 `useState(0)`은 `count`의 초기 상태를 `0`으로 설정한다.
- 버튼을 클릭(onClick)할 때 마다 <u>setState</u>인 `setCount`함수를 사용하여 `count`를 1씩 증가시키고 해당 컴포넌트는 `count`의 값을 반영하여 다시 재랜더링한다.
```jsx
import { useState } from 'react'

function App() {
  const [count, setCount] = useState(0)

  return (
    <>
      <button onClick={() => setCount((count) => count + 1)}>
        count is {count}
      </button>
    </>
  )
}

export default App
```

### :four: State 초기 상태값 설정
- `useState`는 기본적으로 초기 상태값을 설정할 수 있으며, 이 값은 첫 번째 랜더링 시 컴포넌트의 `state`에 저장된다.
- 초기 상태값으로 문자열,숫자,객체,배열 등 모든 자바스크립트 데이터를 사용할 수 있음
```jsx
const [name, setName] = useState('rarrit'); // 문자열
const [age, setAge] = useState(30); // 숫자
const [user, setUser] = useState({name: 'rarrit', age: 30}); // 객체
const [child, setChild] = useState(["child 1", "child 2"]); // 배열
```

### :five: State 업데이트 규칙
#### :pushpin: 5-1) 상태 불변성 유지
상태를 업데이트할 때는 기존 상태를 직접 변경하지 않고, <u>새로운 상태를 만들</u>어야 한다. 이는 리액트가 상태 변경을 감지하고 올바르게 업데이트 할 수 있도록 함
```jsx
const [user, setUser] = userState({ name: 'rarrit', age: 30});
// 불변성 유지
setUser({ ...user, age: 31});

// 잘못된 방법 (불변성 유지 x)
user.age = 31; 
```

#### :pushpin: 5-2) 이전 상태 의존
상태를 업데이트할 때 이전 상태값에 의존해야 한다면 `setState`에 함수를 전달하는 것이 좋다. 이 함수는 이전 상태값을 인자로 받아 새로운 상태를 반환함.
1. 이전 상태값에 의존하는 경우란?
  - 상태 업데이트가 이전 상태에 따라 달라질 때, 이전 상태값에 의존한다고 말한다. 예를 들어 [3] State 사용방법의 `count`를 구현할 때 버튼을 클릭할 때마다 `count`를 증가시키는 경우가 대표적이다. 이때 새로운 상태는 이전 `count` 값에 의존하여 걸정된다.

2. `setState` 함수에 함수를 전달하는 이유
  - 리액트의 상태 업데이트는 비동기적으로 처리 될 수 있으며 상태가 즉시 업데이트되지 않을 수 있고, 여러 <u>상태 업데이트가 동시에 발생할 경우 그 순서나 타이밍이 보장되지 않을</u> 수 있음. 그러므로 상태 업데이트가 이전 상태에 의존할 때는 함수 형태로 `setState`를 사용하여 이러한 문제를 방지할 수 있음

3. 그래서 어떻게 함수를 전달해?
  - 이전 상태값에 의존하여 상태를 업데이트를 하려면 setState에 함수를 전달하고 그 함수는 이전 상태를 인자로 받아 새로운 상태를 반환한다. 이렇게 해야하는 이유는 아래와 같다.
```jsx
const [count, setCount] = useState(0);
// 리액트의 상태 업데이트는 비동기적이기 때문에 아래의 코드에서 두 번 호출해도 씩식 증가함
function handleCountAdd(){
  setCount(count + 1);
  setCount(count + 1);
}

// 해결 방법: 이전 상태(prevCount = count)를 함수 호출 후 상태가 업데이트됨 고로 2씩 증가함
function handleCountAdd(){
  setCount(prevCount => prevCount + 1);
  setCount(prevCount => prevCount + 1);
}
```
### :six: State와 Props의 차이
- `props`
  - 부모 컴포넌트로부터 전달되는 <u>읽기 전용 데이터</u>이며, 자식 컴포넌트는 이를 변경할 수 없음
- `state`
  - 컴포넌트 내부에서 관리되며, 해당 컴포넌트에서만 직접 업데이트할 수 있음

### :seven: State의 비동기적 특성
5-3)의 함수를 전달하는 방법에서 알아봤듯이 리액트의 `state`는 ***비동기***적으로 이루어지므로, 리액트는 여러 상태 업데이트를 모아서 한번에 처리하기 위해(최적화) 때문이다. 그래서 상태가 업데이트 직후에 상태값을 확인하면 예상과 다를 수 있음. 상태 업데이트가 완료된 후에 값을 확인하려면 `useEffect`훅을 사용할 수 있음
```jsx
function Counter() {
  const [count, setCount] = useState(0);

  const handleCountAdd = () => {
    setCount(count + 1);
    console.log(count); // 이 시점에서는 이전 상태값이 출력됨 => 0
  };

  return <button onClick={handleClick}>Increment</button>;
}
```

### :eight: State를 사용하며 주의사항 정리
- <u>여러 개의 상태를 관리하면서 너무 복잡</u>해지면 상태 관리 라이브러리를 찾아보고 사용해본다.
- <u>직접 상태를 변경하지 않도록 주의</u>한다.
- `setState`가 조건 없이 반복적으로 호출되면 컴포넌트가 무한 루프에 빠질 수 있음 이럴땐 `useEffect`에서 조건부로 상태를 업데이트를 하도록 한다.

### :fire: 마무리
입문에서 배우는 Component, Props, State! 입문에서 배우지만 리액트에서는 핵심이고 정말 너무 중요한 것들이다. `State`는 리액트 컴포넌트의 동적인 특성을 유지하는데 중요한 역할을 하고 어떻게 관리하느냐에 따라 프로젝트를 얼마나 쉽게 관리할 수 있을지 판별이 될 것 같다. 앞으로도 경험을 계속 쌓아가면서 좋은 코드를 작성할 수 있도록 노력해봐야겠다.