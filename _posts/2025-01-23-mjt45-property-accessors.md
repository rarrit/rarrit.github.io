---
title: "[mjt] 프로퍼티 getter와 setter를 알아보자"
date: 2025-01-23
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - property
  - getter
  - setter
author_profile: true
sidebar_main: true
---

## :ledger: 프로퍼티 getter와 setter를 알아보자

### :one: 데이터 프로퍼티와 접근자 프로퍼티

객체의 프로퍼티는 두 종류로 나뉜다.

#### :pushpin: 1-1) 데이터 프로퍼티

- 일반적인 키-값 쌍 형태의 프로퍼티
- `value`, `writable`, `enumerable`, `configurable` 속성을 가짐

```javascript
let user = {
  name: "철수", // 데이터 프로퍼티
  age: 25,
};

console.log(user.name); // "철수"
user.age = 30; // 값 변경 가능
```

#### :pushpin: 1-2) 접근자 프로퍼티

- `getter(get)`과 `setter(set)`를 이용해 값을 가져오거나 설정하는 프로퍼티
- `value` 속성 없이 `get`과 `set` 함수로 동작

```javascript
let user = {
  firstName: "철수",
  lastName: "김",

  get fullName() {
    return `${this.lastName} ${this.firstName}`;
  },

  set fullName(value) {
    [this.lastName, this.firstName] = value.split(" ");
  },
};

console.log(user.fullName); // "김 철수"
user.fullName = "박 영희";
console.log(user.firstName); // "영희"
console.log(user.lastName); // "박"
```

#### :pushpin: 1-3) 차이점

| 구분         | 데이터 프로퍼티                    | 접근자 프로퍼티              |
| ------------ | ---------------------------------- | ---------------------------- |
| 값 저장 방식 | `value` 속성을 직접 저장           | `get/set` 메서드를 사용      |
| 읽기 방식    | `obj.property`로 직접 접근         | `get` 메서드를 실행하여 반환 |
| 쓰기 방식    | `obj.property` = value로 직접 변경 | `set` 메서드를 실행하여 처리 |
| 기능         | 단순 값 저장 및 읽기               | 동적 계산, 데이터 검증 가능  |
| 사용 예시    | 일반적인 객체 프로퍼티             | 특정 값 계산, 유효성 검사    |

### :two: Getter(get)

- 프로퍼티 값을 읽을 때 실행되는 함수
- `obj.property` 형식으로 접근 시 자동 실행됨

```javascript
let user = {
  firstName: "철수",
  lastName: "김",

  get fullName() {
    return `${this.lastName} ${this.firstName}`;
  },
};

console.log(user.fullName); // "김 철수"
```

#### :pushpin: 2-1) 장점

- 내부적으로 계산된 값을 반환할 수 있음
- 읽기 전용 프로퍼티를 만들 수 있음

```javascript
let circle = {
  radius: 5,

  get diameter() {
    return this.radius * 2;
  },
};

console.log(circle.diameter); // 10
circle.diameter = 20; // setter가 없으므로 변경되지 않음
```

### :three: Setter (set)

- 프로퍼티 값을 설정할 때 실행되는 함수
- `obj.property = value` 형식으로 접근 시 자동 실행됨

```javascript
let user = {
  firstName: "철수",
  lastName: "김",

  set fullName(value) {
    [this.lastName, this.firstName] = value.split(" ");
  },
};

user.fullName = "박 영희";

console.log(user.firstName); // "영희"
console.log(user.lastName); // "박"
```

#### :pushpin: 3-1) 장점

- 값을 검증하거나 변환할 수 있음
- 객체 내부 상태를 안전하게 조작 가능

```javascript
let user = {
  age: 0,

  set setAge(value) {
    if (value < 0) {
      console.error("나이는 0 이상이어야 합니다.");
    } else {
      this.age = value;
    }
  },
};

user.setAge = -5; // "나이는 0 이상이어야 합니다."
user.setAge = 30;
console.log(user.age); // 30
```

### :four: Getter, Setter와 this 주의점

- `Getter`와 `Setter` 내부에서 `this`는 객체 자신을 가리킨다.
- 독립된 함수로 사용될 경우 `this`가 `undefined`가 될 수 있다.

```javascript
function User(name) {
  this._name = name;

  Object.defineProperty(this, "name", {
    get() {
      return this._name;
    },
    set(value) {
      if (value.length < 3) {
        console.log("이름은 최소 3글자 이상이어야 합니다.");
      } else {
        this._name = value;
      }
    },
  });
}

let user = new User("철수");

console.log(user.name); // "철수"
user.name = "영"; // "이름은 최소 3글자 이상이어야 합니다."
console.log(user.name); // "철수"
```

### :fire: 요약

| 개념            | 설명                                                    |
| --------------- | ------------------------------------------------------- |
| 데이터 프로퍼티 | 일반적인 키-값 형태의 프로퍼티 (`value`, `writable` 등) |
| 접근자 프로퍼티 | `get`, `set`을 사용하여 값을 제어하는 프로퍼티          |
| Getter (`get`)  | 프로퍼티를 읽을 때 실행되는 함수                        |
| Setter (`set`)  | 프로퍼티 값을 설정할 때 실행되는 함수                   |
| 주의점          | `this` 사용 시 주의, setter 없이 값 변경 불가능         |

#### :pushpin: 참고 문서

- [ko.javascript.info - 프로퍼티 getter와 setter](https://ko.javascript.info/property-accessors)
