---
title: "리액트(react.js) memoization란 무엇인가?"
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
  - memoization
author_profile: true
sidebar_main: true
---

## :ledger: 리액트의 Memoization에 대해 알아보자

함수의 성능을 향상시키기 위한 기법이자 이미 계산된 결과를 저장하여 동일한 입력이 있을 때 다시 계산하지 않고 저장된 결과를 반환하는 방법인 `Memoization`에 대해 자세히 알아보자!

### :one: memoization이란 무엇인가?

이 칭구는 비싼 계산 작업을 반복적으로 수행할 때 유용한 친구임. 즉 동일한 입력에 대해 반복적으로 계산하지 않도록 하는 최적화 기법이란 것! 주로 자주 호출되는 함수나 계산 비용이 큰 함수에서 성능을 개선할 때 사용함!

- 계산 비용이 크다란걸 예시로 봤었는데.. for문을 사용하여 100억번 반복되는 함수가 있었음.. ㅋㅋㅋ

### :two: React.memo

`React.memo`는 컴포넌트를 감싸서 해당 <u>컴포넌트의 props가 변경되지 않는 한 리렌더링을 방지</u>할 수 있는 고차함수이다.

#### :pushpin: 2-1) 사용법

`컴포넌트임`은 `count`가 변경되지 않으면 리렌더링이 되지 않는다. `React.memo`를 사용하면 최적화가 가능함.

```jsx
// props가 변경되지 않는 한 리렌더링 되지 않음
const 컴포넌트임 = memo({ value }) => {
  console.log('렌더링됨!');
  return <div>{ value }</div>;
}

function App(){
  const [count, setCount] = useState(0);
  return (
    <>
      <컴포넌트임 value={count}/>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </>
  )
}
```

#### :pushpin: 2-2) 커스텀 비교 함수

`React.memo`는 기본적으로 shallow comparison을 사용하지만, 커스텀 비교 함수를 제공하여 props 비교 방식을 조정할 수 있음

```jsx
const 컴포넌트임 = memo(({ value})) => {
  console.log('렌더링됨!');
  return <div>{value}</div>;
},(prevProps, nextProps) => {
  // 커스텀 비교 함수
  return prevProps.value === nextProps.value;
}
```

### :three: useMemo

`useMemo`는 함수의 계산 결과를 메모이제이션하여, <u>의존성 배열에 지정된 값이 변경될 때만</u> 재계산되도록 한다. 이를 통해 불필요한 연산을 방지할 수 있음

#### :pushpin: 3-1) 기본 사용법

아래의 코드에서 `useMemo`를 사용하여 `많은계산`을 메모제이션 한다. `count`가 변경되지 않는 한 계산 결과는 바뀌지 않음

```jsx
function 계산컴포넌트({ count }) {
  const 많은계산 = useMemo(() => {
    console.log("계산 중");
    // 엄청 많은 계산 작업
    return count * 2;
  }, [count]);

  return <div>{많은계산}</div>;
}

function App() {
  const [count, setCount] = useState(0);
  return (
    <>
      <계산컴포넌트 count={count} />
      <button onClick={() => setCount(count + 1)}>증가</button>
    </>
  );
}
```

### :four: useCallBack

`useCallback`은 함수를 메모이제이션하여, <u>의존성 배열에 지정된 값이 변경될 때만</u> 새 함수가 생성되도록 함. 이를 통해 불필요한 함수 재생성을 방지할 수 있음

#### :pushpin: 4-1) 기본 사용법

아래의 코드에서 `useCallback`을 사용하여 `handleClick`함수를 메모이제이션한다. 의존성 배열이 비어있으므로 함수는 컴포넌트가 처음 렌더링될 때만 생성된다.

```jsx
function Button({ onClick }) {
  console.log("버튼 렌더링");
  return <button onClick={onClick}>클릭</button>;
}

function App() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => {
    console.log("버튼 클릭함!");
  }, []);

  return (
    <div>
      <Button onClick={handleClick} />
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

### :five: Memoization 장단점

#### :pushpin: 5-1) 장점

- **_성능 최적화_**
  - 비싼 계산을 반복하지 않거나, 불필요한 렌더링을 방지할 수 있음
- **_효율적인 상태 관리_**
  - 컴포넌트가 불필요하게 재렌더링되지 않도록 도와준다.

#### :pushpin: 5-2) 단점

- **_메모리 사용 증가_**
  - 메모이제이션된 데이터가 메모리를 차지할 수 있음
- **_복잡성 증가_**
  - 코드의 복잡성을 증가시킬 수 있으며, 불핑료한 메모이제이션 사용은 오히려 성능 저하를 초래함

### :fire: 마무리

메모이제이션은 어떻게 쓰느냐에 따라 성능이 최적화될 수 있고 저하될 수 있는 것 같다. 잘 사용하면 좋겠지만 오히려 성능을 저하시킬 수 있으니 실제로 사용할 때 성능이 향상되는지 확인하며 작업하는 것이 좋을 것 같다!
