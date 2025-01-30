---
title: "[mjt] 프로퍼티 플래그와 설명자란"
date: 2025-01-22
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
  - writable
  - enumerable
  - configurable
author_profile: true
sidebar_main: true
---

## :ledger: 프로퍼티 플래그와 설명자란

### :one: 프로퍼티 플래그

객체 프로퍼티는 `값(value`과 함께 플래그(flag)라 불리는 특별한 속성 세 가지를 갖는다.

- `writable`
  - `writable` - `true`: 값을 수정할 수 있다.
  - `writable` - `false`: 읽기만 가능하다.
- `enumerable`
  - `enumerable` - `true`: 반복문을 사용해 나열할 수 있다.
  - `enumerable` - `false`: 반복문을 사용해 나열할 수 없다.
- `configurable`
  - `configurable` - `true`: 프로퍼티 삭제나 플래그 수정이 가능하다.
  - `configurable` - `false`: 프로퍼티 삭제 및 플러그 수정이 불가능하다.

#### :pushpin: 1-1) 기본 문법

프로퍼티 플래그는 특별한 경우가 아니면 다룰 일이 없으며, 지금까지 해왔던 `평범한 방식`으로 프로퍼티를 만들면 해당 프로퍼티 플래그는 모두 `true`가 된다.

- ![Object.getOwnPropertyDescriptor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)메서드를 사용하면 특정 프로퍼티에 대한 정보를 모두 얻을 수 있다.

```javascript
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

- `obj`: 정보를 얻고자 하는 객체
- `propertyName`: 정보를 얻고자 하는 객체 내 프로퍼티
- 메서드를 호출하면 "프로퍼티 설명자(descriptor)"라고 불리는 객체가 반환되며, 프로퍼티 값과 세 플래그에 대한 정보가 모두 담겨있다.

```javascript
// 예시
let user = {
  name: "John",
};

let descriptor = Object.getOwnPropertyDescriptor(user, "name");

alert(JSON.stringify(descriptor, null, 2));
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

#### :pushpin: 1-2) Object.defineProperty

메서드 ![Object.defineProperty](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)를 사용하면 플래그를 변경할 수 있다.

```javascript
Object.defineProperty(obj, propertyName, descriptor);
```

- `obj`, `propertyName`: 설명자를 적용하고 시은 객체와 객체 프로퍼티
- `descriptor`: 적용하고자 하는 프로퍼티 설명자
- `defineProperty` 메서드는 객체에 해당 프로퍼티가 있으면 플래그를 원하느대로 변경해준다. 프로퍼티가 없으면 인수로 넘겨받은 정보를 이용해 새로운 프로퍼티를 만들며 이때 플래그 정보가 없으면 값은 자동으로 `false`가 된다.

```javascript
// 프로퍼티 name이 새로 만들어지고, 모든 플래그 값이 false가 된 것을 확인할 수 있다.

let user = {};

Object.defineProperty(user, "name", {
  value: "John",
});

let descriptor = Object.getOwnPropertyDescriptor(user, "name");

alert(JSON.stringify(descriptor, null, 2));
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */
```

‘평범한 방식으로’ 객체 프로퍼티 `user.name`을 만들었을 때와 `defineProperty`를 이용해 프로퍼티를 만들었을 때의 가장 큰 **차이점**은 플래그에 있다.

- `defineProperty`를 사용해 프로퍼티를 만든 경우, `descriptor`에 플래그 값을 명시하지 않으면 플래그 값이 자동으로 `false`가 된다.
- 플래그 값을 `true`로 설정하려면 `descriptor`에 true라고 명시해 주어야 한다.

### :two: writable 플래그

- `true` → 프로퍼티 값을 변경할 수 있음
- `false` → 프로퍼티 값을 변경할 수 없음

```javascript
let user = { name: "철수" };

Object.defineProperty(user, "name", { writable: false });

user.name = "영희"; // TypeError (엄격 모드에서 오류 발생)
console.log(user.name); // 철수
```

### :three: enumerable 플래그

- `true` → for...in 루프나 Object.keys()로 열거 가능
- `false` → 반복문에서 숨겨짐

```javascript
let user = { name: "철수", age: 30 };

Object.defineProperty(user, "age", { enumerable: false });

console.log(Object.keys(user)); // ["name"]
for (let key in user) {
  console.log(key); // "name" (age는 보이지 않음)
}
```

### :four: configurable 플래그

- `true` → delete로 삭제 가능, 플래그 수정 가능
- `false` → 프로퍼티 삭제 불가, 플래그 수정 불가 (writable 변경만 가능)

```javascript
let user = { name: "철수" };

Object.defineProperty(user, "name", { configurable: false });

delete user.name; // 삭제되지 않음
console.log(user.name); // 철수

Object.defineProperty(user, "name", { writable: true }); // TypeError (configurable이 false이므로 변경 불가)
```

### :fire: 요약

| 플래그       | true일 때              | false일 때             |
| ------------ | ---------------------- | ---------------------- |
| writable     | 값을 변경할 수 있음    | 값을 변경할 수 없음    |
| enumerable   | for...in 등에서 나열됨 | 반복문에서 보이지 않음 |
| configurable | 삭제 및 속성 변경 가능 | 삭제 및 속성 변경 불가 |

#### :pushpin: 참고 문서

- [ko.javascript.info - 프로퍼티 플래그와 설명자](https://ko.javascript.info/property-descriptors)
