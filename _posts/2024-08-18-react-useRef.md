---
title: "리액트(react.js) useRef란 무엇인가?"
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
  - useRef
  - react hooks
author_profile: true
sidebar_main: true
---

## :ledger: 리액트의 useRef에 대해 알아보자

`useRef`는 리액트에서 <u>DOM 요소에 접근</u>하거나, <u>렌더링 간에 유지되어야 하는 값을 관리</u>하는데 유용한 Hook이다. 그럼 자세히 알아보자.

### :one: useRef란 무엇인가?

- `useRef`는 리액트의 함수형 컴포넌트에서 참조(ref)를 생성하는 훅이다. 이 훅은 컴포넌트가 렌더링될 때 마다 동일한 참조 객체를 반환함
- 주로 DOM 요소에 직접 접근하거나, 컴포넌트의 재렌더링을 유발하지 않는 변수로 사용할 수 있음

### :two: useRef의 주요 특징

- **_값의 유지_**
  - `useRef`를 사용하면, 컴포넌트가 다시 렌더링될 때도 값이 유지됨
- **_DOM 접근_**
  - `useRef`는 특정 DOM 요소에 접근하여 그 요소의 속성에 직접 접근하거나 조작이 가능함
- **_렌더링 비유발_**
  - `useRef`로 관리하는 값이 변경되어도 컴포넌트는 재렌더링이 되지 않음

### :three: useRef는 언제 사용할까?

특정 DOM 요소에 접근해 포커스를 설정하거나, 스크롤 위치를 조절하는 등의 작업이 필요할 때나 렌더링이 반복되더라도 초기화되지 않고 유지되어야 하는 값을 관리할때 사용된다.

### :four: useRef의 사용법

`useRef`는 초기값을 설정할 수 있으며, 이 초기값은 `useRef`가 반환하는 객체의 `current`속성에 저장함.

#### :pushpin: 4-1) 기본 사용법

아래의 코드에서 `useRef`는 Input 요소에 대한 참조를 생성하고 버튼을 클릭했을 때 `inputRef.current`를 통해 input요소에 포커스를 설정함

```jsx
// useState처럼 상단에서 useRef를 작성해줘야함
import { useRef } from "react";

function 컴포넌트임() {
  // useRef
  const inputRef = useRef(null);

  const foucsInput = () => {
    // useRef로 참조한 Input요소에 직접 접근해 포커스를 설정함
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}></button>
    </>
  );
}
```

#### :pushpin: 4-2) 렌더링시 값 유지

`userRef`를 사용해 `count`를 관리하며, 값이 변경되더라도 컴포넌트는 재렌더링되지 않음. 하지만 `renderCount`는 `useState`로 관리되어 값이 변경될때 마다 컴포넌트가 재렌더링됨

```jsx
const Counter = () => {
  const countRef = useRef(0);
  const [renderCount, setRenderCount] = useState(0);

  const addCountHandler = () => {
    countRef.current += 1;
  };

  const reRender = () => {
    setRenderCount(renderCount + 1);
  };

  return (
    <>
      <p>useRef로 관리하는 Count : {countRef.current}</p>
      <p>렌더링 수 : {renderCount}</p>
      <button onClick={addCountHandler}>카운트 증가</button>
      <button onClick={reRender}>리렌더링</button>
    </>
  );
};
```

#### :pushpin: 4-3) 예제 1: 화면 렌더링 시 input에 focus 주기

- `idRef = useRef(null)` 초기값을 null로 적용
- `<input ref={isRef} />` 설정하여, 해당 Input이 `idRef`에 연동되도록함
  - 즉, `idRef.current`는 해당 `<input>` 요소를 가르키게 됨
- `useEffect`를 사용하여 컴포넌트가 처음 렌더링 된 후 `idRef.current.focus()`를 호출함
  - `if`문을 통해 isRef.current가 DOM 요소를 참조하는지 확인
  - `useEffect`의 두 번째 인자에는 `[]`빈배열로 설정하여 첫 렌더링일 때만 실행되도록함

```jsx
// useRef 사용하여 포커싱 주기
const Login = () => {
  // 초기값을 null로 지정
  const idRef = useRef(null);

  useEffect(() => {
    if (idRef.current) {
      // isRef.current 가 DOM 요소를 참조하는지 확인, DOM이 렌더되기 전엔 null임
      idRef.current.focus();
    }
  }, []); // 렌더링되면 한 번만 작동됨

  return (
    <>
      <InputForm>
        <InputBox>
          <dt>아이디</dt>
          <dd>
            <input type="text" ref={idRef} />
          </dd>
        </InputBox>
        <InputBox>
          <dt>비밀번호</dt>
          <dd>
            <input type="password" />
          </dd>
        </InputBox>
      </InputForm>
    </>
  );
};
```

#### :pushpin: 4-3) 예제 2: id 10자리 입력 시 password로 focus 주기

- id의 value 값을 담아주기 위해 `useState` 사용
- password에 focus를 주기위해 `pwRef` 설정
- `focusInputHandler`를 통해 id value에 값을 상태에 업데이트해줌
- `useEffect`에서 `if`문으로 처리되는 이유는 DOM이 렌더링 된 이후에 작업이 수행되어야 하기 때문임
  - 두 번째 인자에 [inputId] 값을 넣어 상태가 업데이트될 때 마다 판별하여 수행함

```jsx
// input value 상태 값을 저장하기 위해 선언함
const [inputId, setInputId] = useState("");

// [A] useRef 사용하여 포커싱 주기
const idRef = useRef(null);
const pwRef = useRef(null);

const focusInputHandler = (e) => {
  setInputId(e.target.value);
};

useEffect(() => {
  if (inputId.length >= 10) {
    pwRef.current.focus();
  }
}, [inputId]);
```

### :five: useRef 장단점

#### :pushpin: 5-1) 장점

- **_DOM 조작_**
  - 간편하게 DOM 요소에 접근, 조작이 가능함
- **_렌더링 효율성_**
  - `userRef`로 관리하는 값은 컴포넌트를 재렌더링하지 않으므로 성능 최적화에 도움을 줌
- **_초기화되지 않은 값_**
  - 렌더링 간에 초기화되지 않아야 하는 값을 손쉽게 관리할 수 있음

#### :pushpin: 4-2) 단점

- DOM 조작은 리액트의 선언적 접근 방식과 반대되므로 남용하면 코드의 일관성이 깨질 수 있음

### :fire: 마무리

자바스크립트에서 DOM 을 조작하는 방법은 익숙해졌는데 리액트에선 어떻게 관리하는지 궁금했었다. 리액트에서 DOM 요소에 접근하거나 렌더링 사이에 값을 유지하는 데 유용한 훅인 `useRef`을 다양한 상황에서 사용해 볼 수 있도록 준비해보자.
