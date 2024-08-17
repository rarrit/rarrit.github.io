---
title: "리액트(react.js) useEffect란 무엇인가?"
date: 2024-08-17
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
  - useEffect
  - react hooks
author_profile: true
sidebar_main: true
---


## :ledger: 리액트의 useEffect에 대해 알아보자
`useEffect`는 리액트에서 컴포넌트의 생명주기와 비동기 작업을 처리하기 위해 중요한 역할을 하고 컴포넌트가 랜더링 된 이후마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

### :one: useEffect란 무엇인가?
- `useEffect`는 리액트의 함수형 컴포넌트에서 부수적인 작용(`side effect`)을 처리할 수 있는 훅이다.  
- 컴포넌트가 렌더링된 후 실행되며, 특정 상태(state)나 props가 변경될 때에도 재실행될 수 있음

#### :pushpin: 1-1) side effect란?
`side effect`란 컴포넌트가 렌더링될 때, <u>컴포넌트의 화면에 그려지는 것 외에 일어나는 모든 작업</u>을 의미함. 리액트에서 컴포넌트가 랜더링될 때 일반적으로 화면에 표시되는 내용만 처리하지만 랜더링 외에 데이터를 가져오거나 브라우저 특정 부분을 직접 조작하는 등의 작업을 해야할 때가 있는데 이러한 작업을 바로 `side effect`로 볼 수 있음

#### :pushpin: 1-2) useEffect 코드로 이해하기
브라우저에서 App 컴포넌트가 화면에 렌더링 된 후 `useEffect`안에 있는 `console.log`가 실행된다. 즉 <u>컴포넌트가 렌더링 된 후 실행된다.</u> 이것이 `useEffect`의 핵심 기능이다.
```jsx
// src/App.js
import React, { useEffect } from "react";
const App = () => {
  useEffect(() => {
		// 이 부분이 실행된다.
    console.log("hello useEffect");
  });
  return <div>Home</div>;
}
export default App;
```

### :two: useEffect는 언제 사용할까?
`useEffect`는 리액트 컴포넌트가 렌더링 된 이후마다 특정 작업을 수행하도록 설정할 수 있는 훅이며 쉽게 말해 <u>어떤 컴포넌트가 화면에 보여졌을 때 내가 무언가를 실행하고 싶다면?</u> 또는 <u>어떤 컴포넌트가 화면에서 사라졌을 때 무언가를 실행하고 싶다면?</u> `useEffect`를 사용하는데, 위의 내용은 나의 생각엔 `useState`의 값이 변경될 때나 특정 로직이 수행될 때? 인 것 같다.

### :three: useEffect의 사용법
- `useEffect`는 두 개의 인자를 받는다.
  - ***첫 번째 인자***: 부작용(부수적인 작용)을 실행할 함수
  - ***두 번째 인자***: 의존성 배열(optional). 이 배열에 포함된 값이 변경될 때 마다 `useEffect`가 다시 실행됨

#### :pushpin: 3-1) 기본 사용법
```jsx
// useState처럼 상단에서 useEffect를 작성해줘야함
import React, { useEffect } from 'react';

function 컴포넌트임() {
  useEffect(() => {
    // 첫 번째 인자임

    // [부수적인 작용] side effect 발동!
    console.log('side effect 발동! 컴포넌트가 렌더링됨!');

    // (Optional) 이 부분은 컴포넌트가 언마운트될 때 실행되는 클린업 함수임.
    return () => {
      console.log('컴포넌트가 언마운트되거나 effect가 다시 실행됨!');
    };
  });

  return <div>컴포넌트입니닷.</div>;
}
```

#### :pushpin: 3-2) 의존성 배열
`useEffect`는 두 번째 인자로 <u>의존성 배열</u>이란 것을 받음. 이 배열에 들어있는 값들이 변경될 때만 `useEffect`가 실행됨
1. 빈 배열 ([]) 
  ```jsx
  useEffect(() => {
    console.log("두 번째 인자에 []가 들아가져 있어서 처음 렌더링에만 실행됨");
  }, []); // 빈 배열을 넣으면, 컴포넌트가 처음 렌더링 될 때만 실행함
  ```
2. 특정 값이 변경될 때만 실행
  ```jsx
  function addCountHandler({ count }){
    useEffect(() => {
      console.log(`현재 count의 값은 ${count}로 변경됨`);
    }. [count]); // count가 변경될 때 이 코드가 실행됨
    return <div>{count}</div>;
  }
  ```
3. 의존성 배열 생략
  ```jsx
  useEffect(() => {
    console.log("생략되면 렌더링될 때 마다 계속 실행됨");
  }) // 생략됨
  ```

#### :pushpin: 3-3) clean up
컴포넌트가 `unmount`될 때 동작하는 `cleanup`함수에 대해 알아보자. 

1. 클린 업이 란?
  - 컴포넌트가 나타났을 때(렌더링 및 컴포넌트 실행) 동작하는 것은 `useEffect`의 `effect`함수다. 반대로 컴포넌트가 사라졌을 때 무언가를 어떻게 실행하는 그 과정을 `clean up`이라고 표현함
2. 클린업을 하는 방법
  - 클린업을 하는 방법은 `useEffect`안에서 return 을 해주고 이 부분에 실행되길 원하는 함수를 넣으면 된다.
  ```jsx
  useEffect(() => {
    // 렌더링될 때 실행되는 함수
    return () => {
      // 클린업 위치임 컴포넌트가 사라질 때 실행하는 함수를 넣으면 됌
    }
  }, []); // 빈 배열이니 한번만 실행함
  ```


### :four: useEffect의 장단점
#### :pushpin: 4-1) 장점
- ***간결한 코드***
  - 클래스형 컴포넌트에서 생명주기 메서드를 사용하는 것보다 훨씬 간결하고 직관적임.
- ***효율적인 자원 관리***
  - 클린업 함수를 통해 타이머나 이벤트 리스너 등을 쉽게 정리할 수 있음

#### :pushpin: 4-2) 단점
- ***의존성 관리의 어려움***
  - 복잡한 컴포넌트에서는 의존성 배열을 올바르게 관리하지 않으면 의도치 않은 재렌더링이 발생할 수 있음
- ***비동기 코드 처리의 복잡성***
  - `useEffect`내에서 비동기 코드를 처리할 때, 적절한 클린업을 하지 않으면 메모리 누수나 불필요한 재렌더링이 발생할 수 있음

### :fire: 마무리
리액트의 훅인 `useEffect`를 알아보았다. 컴포넌트가 렌더링될 때 `side effect`를 실행할 수 있게 도와주는 도구이며 <u>의존성 배열</u>을 사용하여 언제 실행될 지 ***제어***할 수 있으며, `cleanup`함수를 사용하여 컴포넌트가 사라질 때 필요한 정리작업도 할 수 있다. 역시 코딩을 할 때 어떤 느낌인지 더 이해할 수 있으므로 요번 개인과제를 통해 어떻게 활용할 지 고민을 해봐야겠다.