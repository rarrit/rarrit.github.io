---
title: "JavaScript의 단축 속성명, 전개 구문, 템플릿 리터럴"
date: 2024-07-29
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - js
tags:
  - 단축 속성명
  - 전개 구문
  - 템플릿 리터럴
  - 나머지 매개변수
author_profile: true
sidebar_main: true
---

## :ledger: 자바스크립트의 최신 문법을 알아보자!
자바스크립트의 다양한 최신 문법을 알아보자!

### :one: 단축 속성명
단축 속성명은 객체 리터럴에서 변수명과 속성명이 동일할 때, 속성명을 생략하는 문법이다.

```javascript
// key - value 같을 때 생략
const name = 'rarrit';
const newAge = 30;
const obj = {
  name, // name: name, key-value 값이 같아 생략
  age: newAge,
}
console.log(obj); // {name : 'rarrit', age: 30}
```
 
### :two: 전개 구문 (Spread Operator)
전개 구문은 배열이나 객체를 펼칠 때 사용함

#### :pushpin: 2-1) 배열에서의 전개 구문
```javascript
let arr1 = [1,2,3];
console.log(arr1); // [1,2,3]
console.log(...arr1); // 1,2,3

let arr2 = [...arr1, 4];
console.log(arr1); // [1,2,3]
console.log(arr2); // [1,2,3,4]
```

#### :pushpin: 2-1) 객체에서의 전개 구문
```javascript
let user1 = {
  name: 'rarrit',
  age: 30,
};

let user2 = { ...user1 };
console.log(user1); // { name: 'rarrit', age: 30 }
console.log(user2); // { name: 'rarrit', age: 30 }
```

### :three: 나머지 매개변수
함수 매개변수에서 나머지 매개변수는 함수에 전달된 나머지 인수들을 배열로 모으는 역할을 한다.
```javascript
function func(a,b,c, ...arr){
  console.log(a,b,c); // 1, 2, 3
  console.log(arr); // [4, 5, 6, 7]
}
func(1,2,3,4,5,6,7); // 1, 2, 3  [4,5,6,7] => a=1, b=2, c=3, arr = [4,5,6,7]

```

### :four: 템플릿 리터럴
템플릿 리터럴은 문자열 내에서 변수를 쉽게 포함할 수 있으며, 멀티라인 문자열을 지원한다. (백틱 사용=> ``)
```javascript
const val = "Hello";
console.log(`
  ${val} World 
`);
// "Hello World"
```

### :fire: 마무리
자바스크립트의 최신 문법 단축 속성명, 전개 구문, 나머지 매개변수, 템플릿 리터럴에 대해 알아보았는데 전개 구문은 프로그래머스 문제 풀이를 하면서도 정말 많이 본 문법이다. 또한 템플릿 리터럴은 현재 정말 편하게 사용하고 있으며, 알지 못했던 단축 속성명, 나머지 매개변수에 대해 이해할 수 있는 시간이었다.