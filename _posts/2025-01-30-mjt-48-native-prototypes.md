---
title: "[mjt] 내장 객체의 프로토타입에 대해 알아보자"
date: 2025-01-30
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - prototype
author_profile: true
sidebar_main: true
---

## :ledger: 내장 객체의 프로토타입에 대해 알아보자

`prototype`프로퍼티는 자바스크립트 내부에서도 광범위하게 사용되며, 모든 내장 생성자 함수에서 `prototype` 프로퍼티를 사용한다.

### :one: Object.prototype

빈 객체가 있다고 가정해보자

```javascript
let obj = {};
alert(obj); // "[object Object]" ?
```

- `"[object Object]"` 문자열을 생성하는 코드는 어디에 있을까? `toString` 메서드에서 이 문자열을 생성하는 것은 앞서 배워 알고 있지만 `obj`는 비어있는데, 코드는 어디에 있는지 알 수 없다.
- `obj = new Object()`를 줄이면 `obj = {}`가 된다. 여기서 `Object`는 내장 객체 생성자 함순데 이 생성자 함수의 `prototype`은 `toString`을 비롯한 다양한 메서드가 구현되어있는 거대한 객체를 참조한다.
- `new Object()`를 호출하거나 리터럴 문법 `{...}`을 사용해 객체를 만들 때, 새롭게 생성된 객체의 `[[Prototype]]`은 바로 앞 챕터에서 언급한 규칙에 따라 `Object.prototype`을 참조한다.

#### :pushpin: 1-1) 예시를 통해 이해하기

```javascript
let obj = {};

alert(obj.__proto__ === Object.prototype); // true

alert(obj.toString === obj.__proto__.toString); // true
alert(obj.toString === Object.prototype.toString); // true

// Object.prototype 위쪽엔 [[Prototype]] 체인이 없다는 점을 주의해야 한다.
alert(Object.prototype.__proto__); // null
```

### :two: Array.prototype

- 배열(`[]`)은 `Array.prototype`을 상속받는다.
- `Array.prototype`에는 `map()`, `filter()`, `forEach()` 등의 유용한 메서드가 정의되어 있다.

```javascript
console.log(Array.prototype);
```

### :three: Function.prototype

- 모든 함수는 `Function.prototype`을 상속받는다.
- `call()`, `apply()`, `bind()` 등의 메서드가 포함되어 있다.

```javascript
console.log(Function.prototype);
```

### :four: 프로토타입 체인 확인하기

모든 객체는 `__proto__`를 통해 자신의 프로토타입을 참조할 수 있다.

```javascript
const arr = [];
console.log(arr.__proto__ === Array.prototype); // true
console.log(arr.__proto__.__proto__ === Object.prototype); // true
console.log(arr.__proto__.__proto__.__proto__); // null (최상위)
```

- `Array`는 `Array.prototype`을 상속받는다.
- `Array.prototype`은 `Object.prototype`을 상속받는다.
- `Object.prototype`의 프로토타입은 `null`이며, 최상위 객체다.

### :fire: 정리

| 내장 객체            | 주요 프로퍼티 및 메서드                       |
| -------------------- | --------------------------------------------- |
| `Object.prototype`   | `toString()`, `hasOwnProperty()`, `valueOf()` |
| `Array.prototype`    | `map()`, `filter()`, `forEach()`              |
| `Function.prototype` | `call()`, `apply()`, `bind()`                 |

#### :pushpin: 참고 문서

- [ko.javascript.info - 내장 객체의 프로토타입](https://ko.javascript.info/native-prototypes)
