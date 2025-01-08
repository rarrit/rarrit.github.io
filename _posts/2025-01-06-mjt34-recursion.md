---
title: "[mjt] 재귀와 스택에 대해 알아보자"
date: 2025-01-06
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - 재귀
author_profile: true
sidebar_main: true
---

## :ledger: 재귀와 스택에 대해 알아보자

모던 자바스크립트 함수 심화학습의 첫 번째 주제인 재귀와 스택이다. 무엇인지 알아보자

### :one: 재귀란?

문제 해결을 할 때 함수에서 다른 함수를 호출하는 경우가 있다. 이때 함수가 자기 자신을 호출할 수 있는데 이를 `재귀`라고 부른다.

- 큰 목표 작업 하나를 동일하면서 간단한 작업 여러개로 나눌 수 있을 때 유용한 프로그래밍 패턴이다.
- 목표 작업을 간단한 동작 하나와 목표 작업을 변형한 작업으로 단순화 시킬 때 사용한다.
- 특정 자료구조를 다뤄야 할 때도 재귀가 사용된다.

#### :pushpin: 1-1) 예시를 통한 이해

`x`를 `n`제곱해 주는 함수 `pow(x,n)`을 만든다. `pow(x,n)`는 `x`를 `n`번 곱해주기 때문에 아래와 같은 결과를 만족해야 한다.

```javascript
pow(2, 2) = 4
pow(2, 3) = 8
pow(2, 4) = 16
```

구현하는 방법은 두 가지가 있다.

```javascript
// # 1. 반복적인 사고를 통한 방법: `for` 루프
function pow(x, n) {
  let result = 1;
  for (let i = 0; i < n; i++) {
    result *= x;
  }
  return result;
}
alert(pow(2, 3)); // 8

/* # 2. 재귀적인 사고를 통한 방법: 작업을 단순화하고 자기 자신을 호출함
 ** ## 2.1. n == 1 일 경우: 모든 절차가 간단해 진다.
 * 명확한 결괏값을 즉시 도출하므로 이를 재귀의 베이스 라고 한다.
 *
 * ## 2.2. n == 2 아닐 경우: pow(x, n)은 x * pow(x, n -1)로 표현이 가능하다.
 * 이를 재귀 단계라고 부르며, 목표 작업 pow(x, n)을 간단한 동작(x를 곱하기)과 목표 작업을
 * 변형한 작업 (pow(x, n - 1))으로 분할했다. 재귀 단계는 n이 1이 될 때까지 계속 이어진다.
 */

function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}
/*
 * pow (2, 4)를 계산하려면 아래와 같은 재귀 단계가 차례대로 이어진다.
 * pow(2, 4) = 2 * pow(2, 3)
 * pow(2, 3) = 2 * pow(2, 2)
 * pow(2, 2) = 2 * pow(2, 1)
 * pow(2, 1) = 2
 */
```

#### :pushpin: 1-2) 재귀를 사용한 코드는 짧다.

재귀를 사용한 코드는 반복적 사고에 근거하여 작성한 코드보다 대개 잛다.

`if`대신 조건부 연산자 `?`를 사용하면 `pow(x, n)`를 더 간결하고 읽기 쉽게 만들 수 있다.

```javascript
function pow(x, n) {
  return n == 1 ? x : x * pow(x, n - 1);
}
```

#### :pushpin: 1-3) 재귀 깊이

가장 처음 하는 호출을 포함한 중첩 호출의 최대 개수는 재귀 깊이라고 한다. `pow(x, n)`의 재귀 깊이는 `n`이다.

- 자바스크립트 엔진은 최대 재귀 깊이를 제한한다. 만개 정도까진 확실히 허용하고 엔진에 따라 더 많은 깊이를 허용하는 경우도 있다.
- 대다수의 엔진이 십만까지는 다루지 못한다.
- 재귀 깊이 제한 때문에 재귀를 실제 적용하는데 제약이 있지만, 재귀를 사용하면 간결하고 유지보수가 쉬운 코드를 만들 수 있기에 여전히 많이 사용된다.

#### :pushpin: 1-4) 재귀 예제

팩토리얼을 재귀적으로 계산하는 예제

```javascript
function factorial(n) {
  if (n === 0) return 1; // 기저 조건: 재귀를 종료할 조건
  return n * factorial(n - 1); // 재귀적 단계: 문제를 더 작은 문제로 쪼개어 자기 자신을 호출
}

console.log(factorial(5)); // 120
```

### :two: 실행 컨텍스트와 스택

함수 내부 동작에 대해 살펴보며 재귀 호출이 어떻게 동작하는지 알아보자. <br/>
실행 중인 함수의 실행 절차에 대한 정보는 해당 함수의 **실행 컨텍스트에 저장**된다.

1. 실행 컨텍스트는 함수 실행에 대한 세부 정보를 담고 있는 내부 데이터 구조이며, 제어 흐름의 현재 위치, 변수의 현재 값 `this`의 현재 값 등 상세 내부 정보가 실행 컨텍스트에 저장된다.
2. 함수 호출 1회당 정확히 하나의 실행 컨텍스트가 생성된다.
3. 함수 내부에 중첩 호출이 있을 때는 아래와 같은 절차가 수행된다.

- 현재 함수의 실행이 일시 중지된다.
- 중지된 함수와 연관된 실행 컨텍스트는 **실행 컨텍스트 스택**이라는 특별한 자료 구조에 저장된다.
- 중첩 호출이 실행된다.
- 중첩 호출 실행이 끝난 이후 실행 컨텍스트 스택에서 일시 중단한 함수의 실행 컨텍스트를 꺼내오고, 중단한 함수의 실행을 다시 이어간다.

#### :pushpin: 2-1) 예제를 통한 이해

1. `countdown(3)` → 스택에 `countdown(3)` 저장
2. `countdown(2)` → 스택에 `countdown(2)` 저장
3. `countdown(1)` → 스택에 `countdown(1)` 저장
4. `countdown(0)` → 기저 조건 충족, 스택에서 함수 제거 후 반환

```javascript
function countdown(n) {
  if (n === 0) {
    console.log("Done!");
    return;
  }
  console.log(n);
  countdown(n - 1); // 재귀적 호출
}

countdown(3);
```

### :three: 재귀적 구조

`재귀적 구조`는 **전체 구조가 자기 자신과 비슷한 작은 구조로 구성된 형태**를 말하며, `재귀적 사고`는 이러한 구조를 파악하고 작은 단위에서 문제를 해결한 뒤 이를 결합하는 방식으로 접근한다.

#### :pushpin: 3-1) JSON과 재귀적 구조

재귀적 구조의 대표적인 예시로 JSON을 볼 수 있다. JSON 객체는 중첩된 객체나 배열로 구성될 수 있다.

```javascript
const data = {
  name: "Alice",
  children: [
    {
      name: "Bob",
      children: [
        { name: "Carol", children: [] },
        { name: "David", children: [] },
      ],
    },
  ],
};

function printNames(node) {
  console.log(node.name);
  if (node.children) {
    node.children.forEach((child) => printNames(child));
  }
}

printNames(data);
// 출력:
// Alice
// Bob
// Carol
// David
```

### :fire: 회고

재귀와 스택, 재귀적 순회, 재귀적 구조는 프로그래밍에서 중요한 개념이라 하는데 너무 어렵게 느껴진다. 재귀적 사고를 기르기 위해 연습할만한 문제를 정리해봤다.

1. 피보나치 수열
2. 하노이의 탑
3. 이진 트리 순회
4. 배열 합계 계산
5. 문자열 뒤집기
6. JSON 데이터 탐색

#### :pushpin: 참고 문서

- [ko.javascript.info - 재귀와 스택](https://ko.javascript.info/recursion)
