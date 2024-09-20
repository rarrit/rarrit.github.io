---
title: "옵셔널 체이닝(Optional Chaining)이란 무엇인지 알아보자"
date: 2024-09-16
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - development
  - til
tags:
  - Optional Chaining
author_profile: true
sidebar_main: true
---

## :ledger: 옵셔널 체이닝에 대해 알아보자.
자바스크립트로 코딩을 하다 보면, 객체의 깊은 속성에 접근할 때 예상치 못한 `undefined`에러를 만나는 경우가 많다. 이러한 문제를 효율적으로 해결해주는 문법이 바로 옵셔널 체이닝이다.

### :one: 옵셔널 체이닝이란?

- 옵셔널 체이닝은 객체 속성에 안전하게 접근하기 위한 자바스크립트 문법이다. 
- 객체의 속성이나 함수가 `undefined` 또는 `null`인 경우에도 에러를 발생시키지 않고, 안전하게 값을 반환할 수 있다.

```javascript
const user = {
  name: 'John',
  address: {
    city: 'Seoul'
  }
};

// 옵셔널 체이닝이 없는 경우
console.log(user.address.city); // 'Seoul'
console.log(user.address.zipCode); //  TypeError: Cannot read properties of undefined

// 옵셔널 체이닝 사용
console.log(user?.profile?.age); // undefined (에러 발생하지 않음)
```


### :two: 옵셔널 체이닝의 필요성
- 옵셔널 체이닝을 사용하지 않으면, 객체의 속성에 접근할 때마다 아래와 같은 조건문을 사용해야 한다.
- 아래와 같이 반복되는 조건문을 피하고 더 안전하고 직관적인 코드를 작성하기 위해 옵셔널 체이닝을 사용할 필요가 있음

```javascript
if (user && user.profile && user.profile.age) {
  console.log(user.profile.age);
}
```

### :three: 옵셔널 체이닝의 사용법

#### :pushpin: 3-1) 객체 속성 접근
- 객체의 존재 여부를 확인한 후 속성에 접근

```javascript
const city = user?.address?.city;
```

#### :pushpin: 3-2) 함수 호출
- 함수가 존재할 때만 호출

```javascript
user?.getAddress?.();
```

#### :pushpin: 3-3) 배열 접근
- 배열이 존재할 때만 인덱스에 접근

```javascript
const firstItem = items?.[0];
```

### :four: 옵셔널 체이닝의 장단점

#### :pushpin: 4-1) 장점
1. `안전성`
  - 객체의 중첩된 속성에 접근할 때 발생할 수 있는 `undefined` 에러를 방지함
2. `가독성`
  - 코드가 간결해지고 에러 처리를 위한 조건문을 줄일 수 있음.

#### :pushpin: 4-2) 단점
1. `실수 방지`
  - 옵셔널 체이닝을 남용하면, 데이터가 없는 상황에서도 실수로 잘못된 데이터에 의존할 수 있음
2. `구버전 브라우저 지원`
  - 구버전 브라우저는 지원되지 않음

### :fire: 마무리
프로젝트를 진행하며 자주 보던 옵셔널 체이닝에 대해 알아보았다. 까먹었었는데 이번에 다시 알 수 있게되었으며, API 응답이나 복잡한 객체를 다룰 때 큰 도움이 될 것 같다.