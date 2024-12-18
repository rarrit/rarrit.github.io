---
title: "[mjt] 옵셔널 체이닝(?.)에 대해 알아보자"
date: 2024-12-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - optional-chaining
author_profile: true
sidebar_main: true
---

## :ledger: 옵셔널 체이닝이란 무엇인지 알아보자

`옵셔널 체이닝`은 자바스크립트에서 객체의 프로퍼티에 안전하게 접근하기 위한 문법이다. 자세히 알아보도록하자.

### :one: 옵셔널 체이닝의 기본 문법

옵셔널 체이닝 연산자(`?.`)의 문법은 아래와 같다.

- `?.`을 사용하여, 대상이 `undefined`나 `null`이면 `undefined`를 반환한다.
- 값이 있다면 그 값을 반환한다.

```javascript
obj?.prop;
obj?.[prop];
obj?.method();
```

#### :pushpin: 1-1) 사용 예시

- `user.address?.city`는 정상적으로 `Seoul`을 반환
- `user.contact?.phone` 에서 `contact`가 존재하지 않기 때문에 안전하게 `undefined`를 반환한다.

```javascript
let user = {
  name: "ming",
  address: {
    city: "Seoul",
  },
};

console.log(user.address?.city); // "Seoul"
console.log(user.contact?.phone); // undefined
```

### :two: 옵셔널 체이닝과 기본값 설정

옵셔널 체이닝과 함게 `null`병합 연산자 (`??`)를 사용하면, 기본값을 설정할 수 있다.

```javascript
let user = {
  preferences: null,
};

console.log(user.preferences?.theme ?? "default");
```

### :three: 옵셔널 체이닝은 언제 사용해야해?

옵셔널 체이닝을 남용하는건 옳지 않다.

- `존재하지 않아도 괜찮은 대상에서만 사용`해야 한다.
- 예를드면 사용자 주소를 무조건 받아와야 하는 경우에서 옵셔널 체이닝을 사용해서 에러를 조기에 발견하지 못할 수 있음
  - 즉, 왼쪽 평가 대상이 없어도 괜찮은 경우에만 선택적으로 사용하는게 옳다.

### :fire: 회고

이전 프로젝트 진행할 때 무분별하게 사용했던 기억이 있었는데, 이번에 옵셔널 체이닝을 명확하게 알 수 있어서 좋았다.

- 옵셔널 체이닝은 객체, 배열, 함수 호출 등에서 안전하게 프로퍼티에 접근할 수 있는 방법이다.
- 에러를 방지하고 undefined를 반환하므로 코드가 간결하고 안전해진다.
- 하지만 읽기 전용이며, 과도하게 사용하지 않도록 주의해야 한다.
  - 왼쪽 평가 대상이 없어도 괜찮은 경우에만 선택적으로 사용하는게 옳다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 옵셔널 체이닝 '?.'](https://ko.javascript.info/optional-chaining)
