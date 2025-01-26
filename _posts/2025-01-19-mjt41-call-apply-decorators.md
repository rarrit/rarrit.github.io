---
title: "[mjt] call/apply와 데코레이터, 포워딩에 대해 알아보자"
date: 2025-01-19
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - call
  - apply
  - decorators
author_profile: true
sidebar_main: true
---

## :ledger: call/apply와 데코레이터, 포워딩에 대해 알아보자

함수 간에 호출을 어떻게 `포워딩(forwarding)`하는지, 함수를 어떻게 `데코레이팅(decorating)`하는지에 대해 알아보자.

### :one: call과 apply로 함수 호출하기

JavaScript에서는 `call`과 `apply`를 사용하여 함수의 **this**와 매개변수를 동적으로 설정하며 호출할 수 있다. 두 메서드는 비슷하지만, 매개변수를 전달하는 방식이 다르다.

#### :pushpin: 1-1) call 메서드

`call`은 매개변수를 쉼표(,)로 나열하여 전달한다.

```javascript
// 문법
func.call(thisArg, arg1, arg2, ...);

// 예시
function greet(greeting, name) {
  console.log(`${greeting}, ${name} !`);
}

greet.call(null, "hi", "ming"); // hi, ming
```

#### :pushpin: 1-2) apply 메서드

`apply`는 매개변수를 배열 형태로 전달한다.

```javascript
// 문법
func.apply(thisArg, [arg1, arg2, ...]);

// 예시
function sum(a, b) {
  return a+ b;
}
const result = sum.apply(null, [3, 5]);
console.log(result); // 8
```

#### :pushpin: 1-3) call과 apply의 차이

| 메서드  | 매개변수 전달 방식        | 사용 예시                 |
| ------- | ------------------------- | ------------------------- |
| `call`  | 쉼표로 나열(`arg1, arg2`) | `func.call(obj, a, b)`    |
| `apply` | 배열(`arg1, arg2`)        | `func.apply(obj, [a, b])` |

### :two: 함수 포워딩

#### :pushpin: 2-1) 포워딩이란?

포워딩은 **기존 함수의 동작을 변경하지 않고**, 다른 함수에 호출을 전달하는 것을 의미한다. 이를 통해 함수의 기본 기능을 유지하면서, 새로운 동작을 추가할 수 있다.

```javascript
// 포워딩 예시
function originalFunction(a, b) {
  return a + b;
}

function forwardFunction(...args) {
  // originalFunction을 그대로 호출
  return originalFunction(...args);
}

console.log(forwardFunction(3, 5)); // 8
```

#### :pushpin: 2-2) call을 활용한 포워딩

```javascript
function originalFunction(a, b) {
  console.log(`Called with: ${a}, ${b}`);
}

function forwardFunction(...args) {
  originalFunction.call(this, ...args);
}

forwardFunction(1, 2); // Called with: 1, 2
```

### :three: 데코레이터 (Decorator)

`데코레이터`는 기존 함수에 추가적인 기능을 덧붙이는 패턴이다. 함수의 원래 동작을 변경하지 않고, 실행 전후에 추가 작업을 수행할 수 있음

```javascript
function logDecorator(func) {
  return function (...args) {
    console.log(`Called with arguments: ${args}`);
    return func(...args);
  };
}

function sum(a, b) {
  return a + b;
}

const decoratedSum = logDecorator(sum);
console.log(decoratedSum(3, 5)); // Called with arguments: 3,5 -> 8
```

#### :pushpin: 3-1) 주요 용도

1. `로깅`: 함수 호출 기록을 남기기
2. `권한 확인`: 실행 전 조건 검사
3. `캐싱`: 결과를 저장해 성능 최적화

### :four: 포워딩과 데코레이터의 조합

데코레이터와 포워딩은 함께 사용되기도 한다. 아래 예시는 함수 호출을 포워딩하면서, 호출 횟수를 세는 데코레이터이다.

```javascript
function countDecorator(func) {
  let count = 0;

  return function (...args) {
    count++;
    console.log(`Called ${count} times`);
    return func(...args); // 함수 호출 포워딩
  };
}

function greet(name) {
  return `Hello, ${name}!`;
}

const decoratedGreet = countDecorator(greet);

console.log(decoratedGreet("Alice")); // Called 1 times -> Hello, Alice!
console.log(decoratedGreet("Bob")); // Called 2 times -> Hello, Bob!
```

### :fire: 요약

- `call`과 `apply`: 함수 호출 시 this와 매개변수를 동적으로 설정.
- `포워딩`: 기존 함수의 동작을 유지하며 호출을 전달.
- `데코레이터`: 함수에 새로운 동작을 추가하는 패턴.

#### :pushpin: 참고 문서

- [ko.javascript.info - call/apply와 데코레이터, 포워딩](https://ko.javascript.info/call-apply-decorators)
