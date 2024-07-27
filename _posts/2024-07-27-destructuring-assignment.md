---
title: "JavaScript의 구조 분해 할당"
date: 2024-07-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - js
tags:
  - array
author_profile: true
sidebar_main: true
---

## :ledger: 자바스크립트의 구조 분해 할당이란?
- 자바스크립트에서 구조 분해 할당은 배열이나 객체의 속성을 해체하여 개별 변수에 쉽게 할당할 수 있도록 하는 문법이다.
- 이 문법은 코드를 더 간결하고 읽기 쉽게 만들며, 함수에서 매개변수를 처리하거나 여러 값을 반환할 때 유용하다.

### :one: 배열 구조 분해 할당
배열의 요소를 개별 변수에 할당할 때 사용된다.

```javascript
let [value1, value2] = [1, 'new'];
console.log('value1 =>', value1); // 1
console.log('value2 =>', value2); // new

const numbers = [1,2,3,4,5];
const [one, two, three, four, five] = numbers;
console.log('one =>', one); // one;
console.log('two =>', two); // two;
```
 
### :two: 객체 구조 분해 할당
객체의 속성을 개별 변수에 할당할 때 사용한다.

```javascript
const user = {
  name: 'rarrit',
  age: 30,
}
const {name, age} = user;
console.log('userName =>', name); // rarrit
console.log('userAge =>', age); // 30
```   
### :three: 함수 매개변수 사용 예시
함수의 매개변수로 전달되는 객체를 구조분해하여 개별 변수로 사용할 수 있다.

```javascript
function rarrit({ name, age }) {
  console.log(`나의 이름은 ${name}이고 현재 ${age}살이야.`)
}
const rarritInfo = {
  name : 'rarrit',
  age  : 30
}
function rarrit(rarritInfo); // '나의 이름은 rarrit이고 현재 30살이야'
```

### :four: 함수 반환값 예시
함수가 여러 값을 배열이나 객체로 반환할 때 구조 분해 할당을 사용하여 개별 변수로 쉽게 받을 수 있다.

```javascript
function func(){
  return { a: 1, b: 2};
}
const {a, b} = func();
console.log(a); // 1
console.log(b); // 2
```

### :five: 기본값 설정
구조분해할당을 사용할 때 기본값을 미리 설정할 수 있다.

```javascript
const [a = 1, b = 2] = [];
console.log(a); // 1
console.log(b); // 2
```

### :six: 중첩된 객체,배열의 구조 분해 할당
중첩된 구조에서 구조 분해 할당을 사용할 수 있다.

```javascript
const rarrit = {
  name: 'minkyu',
  info: {
    age: 30,
  }
};
const { name, info : {age} } = rarrit;
console.log(name); // minkyu
console.log(age); // 30
```

### :seven: 구조 분해 할당 유의할 점
#### :pushpin: 7-1) undefined
배열이나 객체의 속성이 존재하지 않는 경우 `undefined`가 할당되어 기본값을 설정하는 것이 좋다.
```javascript
const [a = 1, b = 2] = [7];
console.log(a); // 7
console.log(b); // 2
```

#### :pushpin: 7-2) 중첩된 구조
중첩된 구조의 경우 가독성이 떨어질 수 있어 적절하게 사용하는 것이 중요하다.<br/>
반드시 사용해야한다면 중첩된 구조를 단계별로 구조 분해 할당해서 가독성을 높힌다.
```javascript
const rarrit = {
  id: 1
  name: 'minkyu',
  info: {
    age: 30,
  },
  contact: {
    email: {
      naver : 'rarrit94@naver.com',
      google : 'tjfvndfud@gmail',
    },
    phone: '010-1234-5678'
  }
};
// 가독성 나쁜 예시
const {id, name, info: {age}, contact: {email:{naver,google}, phone}} = rarrit

// 위를 개선하는 방법
const { id, name } = rarrit;
const { info: { age } } = rarrit;
const { contact: { email, phone } } = rarrit;
const { naver, google } = email;
```

### :eight: 구조 분해 할당을 알아야 하는 이유
#### :pushpin: 8-1) 코드 가독성 향상
구조 분해 할당을 사용하면 코드가 더 간결하고 읽기 쉬워지며 유지보수에도 도움이 된다.

```javascript
// 구조 분해 할당 전
const user = response.data.user;
const id = user.id;
const name = user.name;

// 구조 분해 할당 후
const {id, name} = response.data.user;
```

#### :pushpin: 8-1) 코드 중복 감소
객체나 배열에서 필요한 값을 추출하는 과정에서 코드의 중복을 줄일 수 있음

```javascript
// 구조 분해 할당 전
const x = user.x;
const y = user.y;

// 구조 분해 할당 후
const {x, y} = user;
```

### :fire: 마무리
이전 강의를 시청하며 구조 분해 할당에 대해 헷갈리는 부분이 있어 공부를 해보았다.<br/>
구조 분해 할당을 실무에서 사용하는 예시는 아직 크게 모르겠지만 코드의 가독성을 높이고 중복을 줄이며, 데이터 접근을 간편하게 할 수 있다고 생각되며, 실무에서 사용하게 된다면 추가해보도록 하겠다.