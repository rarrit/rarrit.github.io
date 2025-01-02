---
title: "[mjt] 반복 가능한 객체 iterable 객체를 알아보자"
date: 2025-01-02
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - iterable
  - object
author_profile: true
sidebar_main: true
---

## :ledger: 반복 가능한 객체 iterable 객체를 알아보자

반복 가능한(iterable) 객체는 `for...of`와 같은 반복문에서 사용할 수 있는 객체를 의미한다. 실무에서 자주 접하는 `배열`, `문자열`, `Map`, `Set` 등이 대표적인 **이터러블 객체**입니다.

### :one: 이터러블 핵심

- `조건`: Symbol.iterator 메서드를 구현하고 있어야 한다.
- `이터레이터와의 관계`: Symbol.iterator 메서드는 이터레이터를 반환한다. 이터레이터는 next() 메서드를 사용해 순회 상태를 관리한다.

```javascript
const iterableObject = {
  [Symbol.iterator]() {
    let step = 0;
    return {
      next() {
        step++;
        if (step <= 3) {
          return { value: step, done: false };
        } else {
          return { value: undefined, done: true };
        }
      },
    };
  },
};

for (const value of iterableObject) {
  console.log(value); // 1, 2, 3
}
```

### :two: 주요 이터러블 객체

- 배열(Array)
- 문자열(String)
- Map 및 Set
- DOM 컬렉션(NodeList 등)

#### pushpin: 2-1) for..of

`for...of`를 사용하면 간단히 데이터를 순회할 수 있다. 또한 DOM 컬렉션이나 API 응답 데이터 등에서도 자주 활용된다.

```javascript
// 배열
const arr = [1, 2, 3];
for (const num of arr) {
  console.log(num); // 1, 2, 3
}

// 문자열
const str = "hello";
for (const char of str) {
  console.log(char); // h, e, l, l, o
}

// Map과 Set
const map = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}
```

#### :pushpin: 2-2) DOM 컬렉션(NodeList) 순회

DOM 조작 시 반환되는 `NodeList`는 이터러블이므로, for...of로 쉽게 순회할 수 있다.

```javascript
const elements = document.querySelectorAll("div");
for (const el of elements) {
  console.log(el.textContent);
}
```

### :pushpin: 2-3) 제너레이터로 유연한 이터러블 생성

반복 로직이 복잡하거나 동적 데이터를 처리할 때 유용하다. 실무에서는 대량 데이터 처리나 무한 스크롤 구현 등에 활용된다.

```javascript
// 제너레이터 함수를 정의한다.
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    // yield
    // 함수의 실행을 일시 중지하고 호출자에게 값을 반환한 후, 함수의 실행을 재개한다.
    // 이를 통해 함수는 이전 상태를 기억하고 당므 호출 때 이어서 실행할 수 있다. 주로 반복 가능한 객체를 생성할 때 사용됨
    yield i;
  }
}

for (const num of range(1, 5)) {
  console.log(num); // 1, 2, 3, 4, 5
}
```

### :fire: 회고

실무에서는 내장 이터러블 객체와 for...of를 활용한 데이터 순회가 가장 일반적인 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - iterable 객체](https://ko.javascript.info/iterable)
