---
title: "[mjt] 참조에 의한 객체 복사에 대해 알아보자"
date: 2024-12-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - object-copy
author_profile: true
sidebar_main: true
---

## :ledger: 참조에 의한 객체 복사에 대해 알아보자

자바스크립트 자료형 중 `객체`와 `원시 타입`의 근본적인 차이 중 하나는 객체는 `참조에 의해` 저장되고 복사된다는 것이다. 반면에 원시값은 값 그대로 저장·할당되고 복사된다.

### :one: 기본 자료형과 객체의 차이

먼저 자바스크립트의 기본 자료형(문자열, 숫자 등)은 값이 `그자체로 복사`되지만 객체는 `참조에 의해 복사`된다.

#### :pushpin: 1-1) 기본 자료형 복사

여기서 `a`와 `b`는 각각 독립적인 값을 가지므로 `b`의 값을 변경해도 `a`에 영향을 미치지 않는다.

```javascript
let a = 10;
let b = a;
b = 20;

console.log(a); // 10
console.log(b); // 20
```

#### :pushpin: 1-2) 객체 복사 및 비교

여기서 `obj1`과 `obj2`는 같은 객체를 참조하고 있으므로, 하나의 변경사항이 다른쪽에도 영향을 준다.

- 객체 비교 시 `동등 연산자 (==)`와 `일치 연산자(==)`는 동일하게 동작한다.
- 비교 시 피연산자인 두 객체가 동일한 객체인 경우에 참을 반환함

```javascript
let obj1 = { name: "김민규" };
let obj2 = obj1; // obj1의 참조값이 복사된다.
let obj3 = {};
let obj4 = {};

obj2.name = "규민김";

console.log(obj1); // "규민김"
console.log(obj2); // "규민김"
console.log(obj1 == obj2); // true
console.log(obj1 === obj2); // true
console.log(obj3 == obj4); // false
```

#### :pushpin: 1-3) 차이점

- `메모리 관점`
  - 객체는 메모리에 한 번만 저장되고, 여러 변수가 이 메모리를 참조하게됨
  - 객체를 복사하거나 함수에 전달할 때, 메모리 주소(참조)만 복사됨
- `원시값과의 차이`
  - 원시값(문자열,숫자 등)은 복사 시 값 자체가 복사됨 (복사 후 변경하면 원본의 데이터 값은 변경되지 않음)
  - 객체는 복사 시 참조가 복사됨 (복사 후 변경될 때 메모리를 참조하기에 원본의 값 또한 변경됨)

### :two: 객체 복사 (얕은 복사, 깊은 복사)

객체를 복제하는 일은 거의 없다. 참조에 의한 복사로 해결 가능한 일이 대다수다. 하지만 복제가 정말 필요한 상황이라면, 새로운 객체를 만든 다음 기존 객체의 프로퍼티들을 순회해 `원시 수준까지 프로퍼티를 복사`하면 된다.

#### :pushpin: 2-1) for..in 문 사용 (얕은 복사)

```javascript
let user = {
  name: "John",
  age: 30,
};

let clone = {}; // 새로운 빈 객체

// 빈 객체에 user 프로퍼티 전부를 복사해 넣어준다.
for (let key in user) {
  clone[key] = user[key];
}

// 이제 clone은 완전히 독립적인 복제본이 되었음.
console.log(user == clone); // false;
console.log(user === clone); // false;

clone.name = "Pete"; // clone의 데이터를 변경한다.

alert(user.name); // 기존 객체에는 여전히 John이 있음.
```

#### :pushpin: 2-2) Object.assign 사용 (얕은 복사)

Object.assign 을 사용하는 방법도 있다.

```javascript
let user = { name: "john", age: 30 };
let clone = Object.assign({}, user);
clone.name = "Alice";
console.log(user.name); // "john" (원본은 영향을 받지 않음)
console.log(clone, clone.name); // {name: "Alice", age: 30} "Alice"
```

#### :pushpin: 2-3) 스프레드 연산자 사용 (얕은 복사)

가장 많이 사용되는 것이 스프레드 연산자인 것 같다.

```javascript
let user = { name: "John", age: 30 };
let clone = { ...user };
clone.age = 25;
console.log(user.age); // 30
console.log(clone, clone.age); // {name: "John", age: 25} 25
```

#### :pushpin: 2-4) JSON (깊은 복사)

객체 내부의 모든 중첩 구조까지 복사한다.

```javascript
let user = { name: "John", preferences: { theme: "dark" } };
let clone = JSON.parse(JSON.stringify(user));
clone.preferences.theme = "light";
console.log(user.preferences.theme); // "dark" (원본은 영향을 받지 않음)
```

#### :pushpin: 2-5) lodash 라이브러리 사용

[Lodash](https://lodash.com/) 라이브러리의 cloneDeep 메서드가 대표적입니다.

```javascript
import _ from "lodash";

let user = { name: "John", preferences: { theme: "dark" } };
let clone = _.cloneDeep(user);
clone.preferences.theme = "light";
console.log(user.preferences.theme); // "dark"
```

### :three: 깊은 복사는 언제 사용할까?

- 중첩된 객체 구조를 가진 데이터를 복사할 때.
- 원본 데이터가 유지되어야 하는 경우(예: 상태 관리에서 불변성 유지).
- 데이터가 의도치 않게 수정되는 것을 방지하고 싶을 때.

### :fire: 회고

참조에 의한 객체 복사는 자바스크립트 객체의 중요한 특성이다. 얕은 복사와 깊은 복사를 적절히 활용하면, 예기치 않은 동작을 방지하고 더욱 안정적인 코드를 작성할 수 있으며. 상황에 맞게 판단하고, 적합한 방법을 선택하는 것이 중요할 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 참조에 의한 객체 복사](https://ko.javascript.info/object-copy)
