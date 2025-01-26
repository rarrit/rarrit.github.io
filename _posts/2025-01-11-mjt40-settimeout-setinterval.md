---
title: "[mjt] setTimeout, setInterval, clearTimeout에 대해 알아보자"
date: 2025-01-11
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - setTimeout
  - setInterval
  - clearTimeout
author_profile: true
sidebar_main: true
---

## :ledger: setTimeout, setInterval, clearTimeout에 대해 알아보자

### :one: 호출 스케줄링이란?

일정 시간이 지난 후에 원하는 함수를 예약 실행(호출)할 수 있게 하는 것을 `호출 스케줄링(scheduling a call))`이라고 한다. 호출 스케줄링을 구현하는 방법은 아래와 같이 두 가지 방법이 있다.

- `setTimeout`: 일정 시간이 **지난 후**에 함수를 실행하는 방법
- `setInterval`: 일정 시간 **간격을 두고** 함수를 실행하는 방법

### :two: setTimeout

문법은 아래와 같다.

- `func|code`
  - 실행하고자 하는 코드로, 함수 또는 문자열 형태다. 대개는 이 자리에 함수가 들어가며, 하위 호완성을 위해 문자열도 받을 수 있게 되어있지만 추천하는 방법은 아니다.
- `delay`
  - 실행 전 대기 시간으로, 단위는 밀리초(millisecond, 1000밀리초 = 1초)이며 기본값은 0이다.
- `arg1, arg2 ...`
  - 함수에 전달할 인수들로 IE9 이하에선 지원하지 않는다.

```javascript
// 기본 문법
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)

// 예시 1: 1초 후에 sayHi() 호출
function sayHi(){
  alert('안녕하세요.');
}
setTimeout(sayHi, 1000);

// 예시 2: 함수에 인수를 넘겨줌
function sayHi(who, phrase) {
  alert( who + ' 님, ' + phrase );
}

setTimeout(sayHi, 1000, "김민규", "안녕하세요."); // 김민규 님, 안녕하세요.
```

#### :pushpin: 2-1) setTimeout 주의할 점

`setTimeout`에 함수를 넘길 때, 함수뒤에 `()`를 붙이는 실수를 할 수 있다. 아래의 예시를 확인해보자.

```javascript
// 잘못된 코드
setTimeout(sayHi(), 1000);
```

1. `setTimeout`은 함수의 참조 값을 받도록 정의되어 있음
2. `sayHi()`를 인수로 전달하면 함수 실행 결과가 전달되어 버림
3. `sayHi()`에는 반환문이 없으므로 호출 결과는 `undefined`가 됨
4. `setTimeout`은 스케줄링할 대상을 찾지 못해, 원하는 대로 코드가 동작하지 않는다.

#### :pushpin: 2-2) 중첩 setTimeout

무언가를 일정 간격을 두고 실행하는 방법에는 크게 2가지가 있다.

1. `setInterval`을 이용하는 방법
2. 중첩 `setTimeout`을 이용하는 방법

중첩 `setTimeout`을 이용하는 방법은 `setInterval`을 사용하는 방법보다 유연하며 호출 결과에 따라 다음 호출을 원하는 방식으로 조정해 스케줄링 할 수 있기 때문이다.

```javascript
// setInterval을 이용하지 않고 아래와 같이 중첩 setTimeout을 사용함
// let timerId = setInterval(() => alert('째깍'), 2000);

let timerId = setTimeout(function tick() {
  alert("째깍");
  timerId = setTimeout(tick, 2000); // (*) 실행이 종료되면 다음 호출을 스케줄링한다.
}, 2000);
```

#### :pushpin: 2-3) 대기 시간이 0인 setTimeout

`setTimeout(func, 0)`이나 `setTimeout(func)`를 사용하면 `setTimeout`의 대기 시간을 0으로 설정할 수 있다.

- 대기 시간을 0으로 설정하면 `func`을 `가능한 한 빨리 실행`할 수 있다.
- 이때 스케줄러는 현재 실행 중인 스크립트의 처리가 종료된 이후에 스케줄링한 함수를 실행한다.

```javascript
// 아래의 코드를 실행하면, 'Hello'와 'World'가 순서대로 출력된 것을 알 수 있음
setTimeout(() => alert("World"));
alert("Hello");
```

### :three: clearTimeout으로 스케줄링 취소하기

`setTimeout`을 호출하면 '타이머 식별자(timer identifier)'가 반환된다. 스케줄링을 취소하고 싶으면 이 식별자(아래 예시에서 timerId)를 사용하면 된다.

```javascript
// 스케줄링 취소하기
let timerId = setTimeout(...);
clearTimeout(timerId);
```

#### :pushpin: 3-1) 사용 예시

함수 실행을 계획해 놓았다가 중간에 계획해 놓았던 것을 취소한 상황을 코드로 표현

```javascript
let timerId = setTimeout(() => alert("아무런 일도 일어나지 않는다."), 1000);
alert(timerId); // 타이머 식별자

clearTimeout(timerId);
alert(timerId); // 위 타이머 식별자와 동일함 (취소 후에도 식별자의 값은 null이 되지 않는다.)
```

- 예시를 실행하면 `alert`창이 2개가 뜨는데, 이 alert창을 통해 브라우저 환경에선 타이머 식별자가 숫자라는 것을 알 수 있다.
- 다른 호스트 환경에선 타이머 식별자가 숫자형 이외의 자료형일 수 있다.
- `Node.js`에서 `setTimeout`을 실행하면 타이머 객체가 반환된다.

### :four: setInterval

`setInterval` 메서드는 `setTimeout`과 동일한 문법을 사용한다.

- `setTimeout`과 인수가 동일하지만 한번만 실행되지 않고 `setInterval`은 함수를 주기적으로 실행하게 만든다.
- 함수를 중단하려면 `clearInterval(timerId)`을 사용하면 된다.

```javascript
let timerId = setInterval(func | code, [delay], [arg1], [arg2], ...)
```

#### :pushpin: 4-1) alert 창이 떠 있더라도 타이머는 멈추지 않는다.

크롬, 파이어폭스를 포함한 대부분 브라우저는 `alert/confirm/prompt` 창이 떠 있는 동안에도 내부 타이머를 멈추지 않는다.

- 첫 번째 `alert`창이 떴을 때 몇 초간 기다렸다가 창을 닫으면, 두 번째 `alert`창이 바로 나타나는 것을 보고 확인이 가능하다. 이러한 이유로 얼럿 창은 명시한 지연 시간인 2초보다 더 짧은 간격으로 뜨게 된다.

```javascript
// 2초 간격으로 메시지를 보여줌
let timerId = setInterval(() => alert("째깍"), 2000);

// 5초 후에 정지
setTimeout(() => {
  clearInterval(timerId);
  alert("정지");
}, 5000);
```

### :four: 그럼 언제 사용하면 좋을까?

- 런타임에 사용자 입력을 기반으로 함수를 정의해야 할 때
- JSON 또는 다른 형식으로 저장된 코드를 실행해야 할 때
- 스크립트 생성 툴이나 코드 빌더를 개발할 때

### :fire: 회고

`new Function`은 JavaScript의 강력한 기능 중 하나로, 동적으로 함수를 생성할 수 있는 유연성함이 특징이다. 하지만 보안, 성능, 디버깅 측면에서 단점도 존재하므로 신중하게 사용해야 된다.

#### :pushpin: 참고 문서

- [ko.javascript.info - new Function 문법](https://ko.javascript.info/new-function)
