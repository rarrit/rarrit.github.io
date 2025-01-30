---
title: "[mjt] 프로토타입과 상속에 대해 알아보자"
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

## :ledger: 프로토타입과 상속에 대해 알아보자

자바스크립트는 `프로토타입 기반(prototype-based) 언어`다. 이는 객체가 다른 객체로부터 상속을 받을 수 있다는 의미다.

### :one: 데이터 프로퍼티와 접근자 프로퍼티

- 자바스크립트에서 **모든 객체**는 자신의 프로토타입을 가진다.
- 객체는 **프로토타입을 참조(prototype chain)**하여 상속받은 속성이나 메서드를 사용할 수 있다.

#### :pushpin: 1-1) 예제

아래에서 `user` 객체에는 `greet()` 메서드가 없지만, 프로토타입 체인을 따라 `person`에서 메서드를 찾고 실행한다.

```javascript
let person = {
  greet() {
    console.log("안녕하세요!");
  },
};

let user = {
  name: "철수",
};

user.__proto__ = person; // user가 person을 상속받음

console.log(user.name); // "철수" (자신의 프로퍼티)
user.greet(); // "안녕하세요!" (프로토타입에서 상속)
```

#### :pushpin: 1-2) [[Prototype]]

자바스크립트의 객체는 명세서에서 명명한 [[Prototype]]이라는 숨김 프로퍼티를 갖는다. 이 숨김 프로퍼티 값은 `null`이거나 다른 객체에 대한 참조가 되는데, 다른 객체를 참조하는 경우 참조 대상을 '프로토타입(prototype)'이라 부른다.

#### :pushpin: 1-3) **proto**

- `__proto__`는 `[[Prototype]]`과 다르다.
  - **proto**는 [[Prototype]]의 getter(획득자)이자 setter(설정자)이다.
  - 하위 호환성 때문에 `__proto_`를 사용할 수 있지만 근래에 작성된 스크립트에서는 `Object.getPrototypeOf`나 `Object.setPrototypeOf`을 써서 프로토타입을 획득(get)하거나 설정(set)한다.
- `__proto__`는 브라우저 환경에서만 지원하도록 자바스크립트 명세서에서 규정하였는데, 실상은 서버 사이드를 포함한 모든 호스트 환경에서 `__proto__`를 지원하며 [[Prototype]]보다는 **proto**가 조금 더 직관적이어서 이해하기 쉽다.

### :two: 프로토타입은 읽기 전용이다

객체의 프로토타입을 변경하는 것은 가능하지만, 프로토타입 자체를 수정하는 것은 불가능하다.

- `rabbit.canEat = false`를 실행하면 새로운 프로퍼티가 `rabbit`에 추가될 뿐, `animal`의 값은 변하지 않는다.
- 프로토타입 체인에서는 "읽기"만 가능하고, 직접 수정하지 않는다.

```javascript
let animal = {
  canEat: true,
};

let rabbit = {
  __proto__: animal,
};

rabbit.canEat = false; // rabbit에 새로운 canEat 추가

console.log(rabbit.canEat); // false (자신의 프로퍼티를 변경)
console.log(animal.canEat); // true (프로토타입의 값은 그대로)
```

#### :pushpin: 2-1) 프로토타입 자체를 변경하려면?

```javascript
let animal = {
  canEat: true,
};

let rabbit = {
  __proto__: animal,
};

Object.setPrototypeOf(rabbit, null); // 프로토타입 제거
console.log(rabbit.canEat); // undefined (이제 animal을 참조하지 않음)
```

### :three: this가 나타내는 것

프로토타입 메서드를 호출할 때 `this`는 해당 메서드를 호출한 객체를 가리킨다.

- `dog.sleep()`을 실행하면 `sleep()`은 `animal`에 정의되어 있지만, `this.name`은 `dog.name`을 참조하여 `"바둑이"`가 출력된다.

```javascript
let animal = {
  sleep() {
    console.log(`${this.name}가 잠을 잡니다.`);
  },
};

let dog = {
  name: "바둑이",
  __proto__: animal,
};

dog.sleep(); // "바둑이가 잠을 잡니다." (this는 dog를 가리킴)
```

### :four: for…in 반복문

`for...in` 반복문을 사용하면 객체뿐만 아니라 프로토타입 체인에 있는 프로퍼티까지 포함해서 순회한다.

```javascript
let animal = {
  canRun: true,
};

let rabbit = {
  name: "토끼",
  __proto__: animal,
};

for (let key in rabbit) {
  console.log(key);
}
// name
// canRun (프로토타입의 프로퍼티도 출력됨)
```

#### :pushpin: 4-1) 객체 자신의 프로퍼티만 출력하려면?

`hasOwnProperty()`를 사용하면 프로토타입이 아닌 객체 자신에게만 존재하는 프로퍼티를 확인할 수 있다.

```javascript
let animal = {
  canRun: true,
};

let rabbit = {
  name: "토끼",
  __proto__: animal,
};

for (let key in rabbit) {
  if (rabbit.hasOwnProperty(key)) {
    console.log(key); // "name" (자신의 프로퍼티만 출력)
  }
}
```

### :fire: 요약

| `개념`               | `설명`                                                                                                      |
| -------------------- | ----------------------------------------------------------------------------------------------------------- |
| 프로토타입           | 객체가 다른 객체로부터 속성/메서드를 상속받는 메커니즘                                                      |
| 프로토타입 읽기 전용 | 프로토타입의 프로퍼티를 수정하는 것은 불가능하며, 상속받은 객체에서 새로운 값을 설정하면 원본은 변하지 않음 |
| this                 | 프로토타입 메서드를 호출한 객체를 가리킴                                                                    |
| for…in               | 프로토타입 프로퍼티까지 포함하여 순회함 (hasOwnProperty로 필터링 가능)                                      |

#### :pushpin: 참고 문서

- [ko.javascript.info - 프로토타입 상속](https://ko.javascript.info/prototype-inheritance)
