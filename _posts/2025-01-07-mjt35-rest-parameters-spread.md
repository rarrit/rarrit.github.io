---
title: "[mjt] 나머지 매개변수와 전개 구문 사용법 및 차이점을 알아보자"
date: 2025-01-07
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - rest parameters
  - spread
author_profile: true
sidebar_main: true
---

## :ledger: 나머지 매개변수와 전개 구문 사용법 및 차이점을 알아보자

JavaScript에서는 **나머지 매개변수(Rest Parameters)**와 **전개 구문(Spread Syntax)**을 사용하여 함수 호출, 배열, 객체 등을 다룰 때 더욱 간결하고 강력한 표현이 가능하다. 이 두 개념의 사용법과 차이점에 대해 알아보자.

### :one: 나머지 매개변수 (Rest Parameters)

나머지 매개변수는 함수의 매개변수 목록에서 여러 인수를 하나의 배열로 묶어 처리할 수 있게 해준다.
문법은 `...`을 사용한다.

#### :pushpin: 1-1) 기본 문법

1. 함수의 매개변수 목록 중 **마지막 위치에 작성된 `...`\***는 모든 나머지 인수를 배열로 수집한다.
2. 배열로 변환된 인수를 쉽게 반복하거나 조작할 수 있다.

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 출력: 10
```

#### :pushpin: 1-2) 주의사항

나머지 매개변수는 반드시 마지막에 위치해야 한다.

```javascript
function invalidFunc(a, ...rest, b) {
  // SyntaxError: 나머지 매개변수는 마지막이어야 한다.
}
```

### :two: 전개 구문 (Spread Syntax)

전개 구문은 배열이나 객체를 개별 요소로 분해하거나 병합할 때 사용하며, `...`을 사용하고 사용 맥락에 따라 동작이 달라진다.

#### :pushpin: 2-1) 배열에서의 전개

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const merged = [...arr1, ...arr2];
console.log(merged); // 출력: [1, 2, 3, 4, 5, 6]
```

#### :pushpin: 2-2) 객체에서의 전개

```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

const combined = { ...obj1, ...obj2 };
console.log(combined); // 출력: { a: 1, b: 2, c: 3, d: 4 }
```

### :three: 나머지 매개변수와 전개 구문의 차이점

| 개념            | 사용 위치       | 역할                                              |
| --------------- | --------------- | ------------------------------------------------- |
| 나머지 매개변수 | 함수의 매개변수 | 여러 인수를 배열로 수집                           |
| 전개 구문       | 배열/객체 배열  | 또는 객체를 개별 요소로 분해하거나 병합 또는 복제 |

### :four: 활용 예제

#### :pushpin: 4-1) 함수 호출에 전개 구문 사용

```javascript
function greet(greeting, name) {
  console.log(`${greeting}, ${name}!`);
}

const args = ["Hello", "Alice"];
greet(...args); // 출력: Hello, Alice!
```

#### :pushpin: 4-2) 배열 복사

```javascript
const original = [1, 2, 3];
const copy = [...original];
copy.push(4);

console.log(original); // 출력: [1, 2, 3]
console.log(copy); // 출력: [1, 2, 3, 4]
```

#### :pushpin: 4-3) 객체 병합

```javascript
const defaultSettings = { theme: "light", fontSize: 12 };
const userSettings = { fontSize: 14 };

const settings = { ...defaultSettings, ...userSettings };
console.log(settings); // 출력: { theme: 'light', fontSize: 14 }
```

### :fire: 회고

나머지 매개변수와 전개 구문은 JavaScript에서 데이터 처리와 함수 호출을 더 유연하게 만들 수 있다. 적절히 활용하면 코드의 가독성과 유지보수성을 높일 수 있다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 나머지 매개변수와 전개 구문](https://ko.javascript.info/rest-parameters-spread)
