---
title: "JavaScript의 undefined와 null"
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
  - undefined
  - null
author_profile: true
sidebar_main: true
---

## :ledger: 자바스크립트의 undefined와 null에 대해 알아보자
JavaScript에서 `undefined`와 `null`은 둘 다 없음을 의미하여 자주 혼동되기 쉬운 개념이지만 중요한 차이가있다. 알아보도록 하자!

### :one: undefined
`undefined`는 자바스크립트에서 변수가 선언되었지만, 값이 할당되지 않은 상태를 나타낸다.<br/>
특히 개발자가 직접 지정하는 경우는 거의 없다. 그렇다고 `undefined`를 없다를 표현할 때 명시적으로 표현하지 말것! (개발자들이 헷갈림)

- 변수가 선언되었지만 값이 할당되지 않은 경우
- 함수가 명시적으로 값을 반환하지 않는 경우
- 객체의 속성을 찾을 수 없는 경우

```javascript
// 예시 1) 변수가 선언되었지만 값이 할당되지 않은 경우
let x;
console.log(x); // undefined

// 예시2) 함수가 명시적으로 값을 반환하지 않는 경우
function func(){}
console.log(func()); // undefined

// 예시3) 객체의 속성을 찾을 수 없는 경우
let obj = {a : 'hi'};
console.log(obj.b); // undefined
```
 
### :two: null
`null`은 의도적으로 "값이 없음"을 나타내기 위해 사용되는 값이며, 개발자가 변수에 아무런 값이 없음을 명시적으로 설정한다.<br/>
`typeof null` 하면 `object` 인것은 자바스크립트 자체 버그이니 조심하자!

```javascript
let a = null;
console.log(a); // null
console.log(typeof a); // object : 버그임 버그

let person = { name: 'rarrit', age: null }; // age의 값을 설정하지 않음
console.log(person.age); // 출력: null
```

### :three: 차이점 요약
- `undefined`: 변수가 선언되었지만 값이 할당되지 않았거나, 값이 없음을 나타내기 위해 자바스크립트 자체에서 사용하는 값이다.
- `null`: 개발자가 명시적으로 "값이 없음"을 나타내기 위해 사용하는 값이다.

```javascript
// 동등연산자 : 타입까지는 일치하지 않아도 됌!
console.log(n == undefined); // 둘 다 없음을 의미해서 true
console.log(n == null); // null = null 이므로 true

// 일치연산자 : 타입까지 일치해야 됌!
console.log(n === undefined); // false
console.log(n === null); // null = null 이므로 true
```


### :fire: 마무리
`undefined`는 정말 많이 보았지만, `null`은 많이 본적이 없었는데, 이번 공부를 통해 각각 사용성과 둘의 차이점에 대해 알게되었다.