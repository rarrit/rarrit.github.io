---
title: "Throttling과 Debouncing에 대해 알아보자."
date: 2024-09-20
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - development
  - til
  - react
tags:
  - Optional Chaining
author_profile: true
sidebar_main: true
---

## :ledger: Throttling과 Debouncing을 이용한 버튼 이벤트 처리를 구현해보자.
웹 개발에서 자주 사용되는 두 가지 개념인 `Throttling`과 `Debouncing`을 간단히 구현해보고, 이를 통해 API 요청을 제어하는 방법을 살펴보자.

### :one: Throttling이란?
`Throttling`은 특정 시간 간격 동안에 한 번만 함수를 호출하도록 제한하는 방법이다. 사용자가 여러 번 빠르게 클릭하거나 스크롤할 때, 불필요한 이벤트 처리를 방지할 수 있음. 

- 특정 시간 간격동안 한 번만 함수를 호출하도록 제한하는 방법
- 연속해서 발생하는 이벤트들을 일정시간 단위(delay)로 그룹화하여 처음 또는 마지막 이벤트 핸들러만 호출되도록 하는 것을 말하며 주로 무한스크롤에서 사용한다.

| --- | --- | --- |
| 타입 | 설명 | 예시 |
| `Leading Edge` | 이벤트가 처음 발생할 때 핸들러가 실행됨. 이후 주어진 시간 동안은 이벤트가 무시됨. | 사용자가 스크롤을 시작할 때 처음에만 API 호출이 이루어지고, 일정 시간 동안 추가 호출이 무시됩니다. |
| `Trailing Edge` | 이벤트가 반복적으로 실행될 때, 주어진 시간(delay)이 지나면 마지막 이벤트를 처리 | Leading Edge와 비슷하지만 주어진 시간의 마지막 이벤트에 API 호출이 이루어짐 |
| `Leading & Trailing Edge` | 주어진 시간에 대해 이벤트가 처음 발생할 때 핸들러가 실행되고, 주어진 시간이 지나면 마지막 이벤트도 처리 | 사용자가 버튼을 여러 번 클릭할 때 처음 클릭 시 바로 API 호출이 이루어지고, 주어진 시간의 마지막 이벤트에도 API 호출이 이루어짐 |


### :two: Debouncing이란?
`Debouncing`은 사용자가 연속적으로 이벤트를 발생시킬 때, 마지막 이벤트가 발생하고 일정 시간이 지난 후에만 함수를 호출하는 방식이다.

- 연속해서 이벤트가 발생하면 이벤트 핸들러를 호출하지 않다가 마지막 이벤트로부터 일정 시간(delay)이 경과한 후에 한 번만 호출하도록 하는 것을 말하며 주로 입력값 실시간 검색, 화면 resize 이벤트 등에서 사용된다.

### :three: 코드로 알아보자!

#### :pushpin: 3-1) 예시 코드
```jsx
import { useEffect } from "react";
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();
  let timerId = null;

  // Throttling 함수
  const throttle = (delay) => {
    if (timerId) {
      // 이미 setTimeout이 진행 중이면 return
      return;
    }
    console.log(`API 요청 실행! ${delay}ms 동안 추가 요청 안 받음~`);
    timerId = setTimeout(() => {
      console.log(`${delay}ms 지남. 추가 요청 받음!`);
      timerId = null;
    }, delay);
  };

  // Debouncing 함수
  const debounce = (delay) => {
    if (timerId) {
      clearTimeout(timerId);
    }
    timerId = setTimeout(() => {
      console.log(`마지막 요청으로부터 ${delay}ms 지났으므로 API 요청 실행`);
      timerId = null;
    }, delay);
  };

  // 페이지 이동 함수
  const handleMove = () => {
    navigate("/company");
  };

  // 컴포넌트가 언마운트될 때 타이머 정리
  useEffect(() => {
    return () => {
      if (timerId) clearTimeout(timerId);
    };
  }, []);

  return (
    <div>
      <h2>Button 이벤트 예제</h2>
      <button onClick={() => throttle(2000)}>쓰로틀링 버튼</button>
      <button onClick={() => debounce(2000)}>디바운싱 버튼</button>
      <div>
        <button onClick={handleMove}>페이지 이동</button>
      </div>
    </div>
  );
};

export default Home;
```

#### :pushpin: 3-2) Throttle 함수 설명 
이 함수는 설정한 `delay`동안 추가적인 요청을 ***무시***하고, `setTimeout`을 이용해 요청 주기를 제어함

```jsx
const throttle = (delay) => {
  if (timerId) {
    // 이미 타이머가 존재하면 새로운 요청을 무시
    return;
  }
  console.log(`API 요청 실행! ${delay}ms 동안 추가 요청 안 받음~`);
  timerId = setTimeout(() => {
    console.log(`${delay}ms 지남. 추가 요청 받음!`);
    timerId = null;
  }, delay);
};
```

#### :pushpin: 3-2) Debounce 함수 설명 
이 함수는 사용자가 이벤트를 발생시킬 때마다 타이머를 리셋하여 마지막 이벤트가 발생한 후 일정 시간이 지나야만 함수가 호출되도록 한다.

```jsx
const debounce = (delay) => {
  if (timerId) {
    clearTimeout(timerId);
  }
  timerId = setTimeout(() => {
    console.log(`마지막 요청으로부터 ${delay}ms 지났으므로 API 요청 실행`);
    timerId = null;
  }, delay);
};
```

#### :Pushpin: 3-3) 메모리 누수 방지
타이머가 걸린 상태에서 컴포넌트가 엄마운트될 경우, 메모리 누수를 방지하기 위해 타이머를 정리하는 역할을 한다.
```jsx
useEffect(() => {
  return () => {
    if (timerId) clearTimeout(timerId);
  };
}, []);
```

### :fire: 마무리
`Throttling`과 `Debouncing`은 이벤트의 과도한 호출을 방지하고 성능을 최적하하는데에 유용하다.