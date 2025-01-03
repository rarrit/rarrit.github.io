---
title: "[mjt] 구조 분해 할당에 대해 알아보자"
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
  - 구조 분해 할당
author_profile: true
sidebar_main: true
---

## :ledger: 구조 분해 할당에 대해 알아보자

`객체`와 `배열`은 자바스크립트에서 가장 많이 쓰이는 자료 구조다. 개발을 하다 보면 함수에 객체나 배열을 전달해야 하는 경우가 생기곤 하는데, 배열에 저장된 데이터 일부만 필요한 경우가 생기기도 한다. 이럴 때 객체나 배열을 변수로 `분해`할 수 있게 해주는 특별한 문법인 구조 분해 할당을 사용할 수 있다. 사용법과 개념을 알아보도록 하자.

### :one: 기본 문법

#### :pushpin: 1-1) 배열 구조 분해

```javascript
const [a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

#### :pushpin: 1-2) 객체 구조 분해

```javascript
const { key1, key2 } = { key1: "value1", key2: "value2" };
console.log(key1); // value1
console.log(key2); // value2
```

### :two: 배열 구조 분해 예시

배열에서 특정 위치의 값을 변수에 쉽게 할당할 수 있다.

#### :pushpin: 2-1) 기본 배열 구조 분해

```javascript
const colors = ["red", "green", "blue"];
const [first, second, third] = colors;

console.log(first); // red
console.log(second); // green
console.log(third); // blue
```

#### :pushpin: 2-2) 기본값 설정하기

```javascript
const numbers = [10];
const [a, b = 20] = numbers;

console.log(a); // 10
console.log(b); // 20
```

#### :pushpin: 2-3) 나머지 값 할당하기

```javascript
const fruits = ["apple", "banana", "cherry", "date"];
const [first, ...rest] = fruits;

console.log(first); // apple
console.log(rest); // ["banana", "cherry", "date"]
```

### :three: 객체 구조 분해 예시

객체에서 원하는 속성 값을 추출하여 변수에 쉽게 할당할 수 있다.

#### :pushpin: 3-1) 기본 객체 구조 분해

```javascript
const user = {
  name: "rarrit",
  age: 25,
  city: "Seoul",
};

const { name, age } = user;

console.log(name); // rarrit
console.log(age); // 25
```

#### :pushpin: 3-2) 기본값 설정하기

```javascript
const user = {
  name: "rarrit",
};

const { name, age = 30 } = user;

console.log(name); // rarrit
console.log(age); // 30
```

#### :pushpin: 3-3) 속성 이름 변경하기

```javascript
const user = {
  name: "rarrit",
  age: 30,
};

const { name: userName, age: userAge } = user;

console.log(userName); // rarrit
console.log(userAge); // 40
```

### :four: 함수 매개변수 사용 예시

구조 분해 할당은 함수 매개변수에서도 유용하게 사용할 수 있다.

#### :pushpin: 4-1) 객체 매개변수 구조 분해

```javascript
function greet({ name, age }) {
  console.log(`Hello, ${name}. You are ${age} years old.`);
}

const user = { name: "rarrit", age: 30 };
greet(user); // Hello, rarrit. You are 30 years old.
```

#### :pushpin: 4-2) 배열 매개변수 구조 분해

```javascript
function sum([a, b]) {
  return a + b;
}

const numbers = [5, 10];
console.log(sum(numbers)); // 15
```

### :fire: 회고

구조 분해 할당은 코드의 간결성과 가독성을 높이는데 유용하며 특히 중첩된 데이터 구조를 다루거나 함수 매개변수를 처리할 때 매우 유용하다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 구조 분해 할당](https://ko.javascript.info/destructuring-assignment)
