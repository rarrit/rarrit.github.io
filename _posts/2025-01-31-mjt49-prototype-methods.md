---
title: "[mjt] 프로토타입 메서드와 __proto__가 없는 객체에 대해 알아보자"
date: 2025-01-30
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - prototype
  - prototype method
author_profile: true
sidebar_main: true
---

## :ledger: 프로토타입 메서드와 **proto**가 없는 객체에 대해 알아보자

### :one: 프로토타입 메서드

`__proto__`는 브라우저를 대상으로 개발하고 있다면 다소 구식이기 때문에 더는 사용하지 않는 것이 좋다. 표준에도 관련 내용이 명시되어있다. 대신 아래와 같은 모던한 메서드들을 사용하는 것이 좋다.

- ![Object.create(proto, [descriptors]) ](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
  – `[[Prototype]]`이 proto를 참조하는 빈 객체를 만든다. 이때 프로퍼티 설명자를 추가로 넘길 수 있다.
- ![Object.getPrototypeOf(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)
  – `obj`의 `[[Prototype]]`을 반환한다.
- ![Object.setPrototypeOf(obj, proto)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)
  – `obj`의 `[[Prototype]]`이 proto가 되도록 설정한다.

#### :pushpin: 1-1) **proto** 대신 메서드 사용 예시

```javascript
let animal = {
  eats: true,
};

// 프로토타입이 animal인 새로운 객체를 생성합니다.
let rabbit = Object.create(animal);

alert(rabbit.eats); // true

alert(Object.getPrototypeOf(rabbit) === animal); // true

Object.setPrototypeOf(rabbit, {}); // rabbit의 프로토타입을 {}으로 바꿉니다.
```

#### :pushpin: 1-2) Object.create 사용 예시

`Object.create`에는 프로퍼티 설명자를 선택적으로 전달할 수 있다. 설명자를 이용해 새 객체에 프로퍼티를 추가해본다.

```javascript
let animal = {
  eats: true,
};

let rabbit = Object.create(animal, {
  jumps: {
    value: true,
  },
});

alert(rabbit.jumps); // true
```

`Object.create`를 사용하면 `for..in`을 사용해 프로퍼티를 복사하는 것보다 더 효과적으로 객체를 복제할 수 있다.

```javascript
let clone = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```

### :two: **proto**가 없는 객체

- 객체를 생성할 때 `Object.create(null)`을 사용하면 `__proto__`가 없는 객체를 만들 수 있다.
- 이런 객체는 `Object.prototype`의 영향을 받지 않기 때문에, 순수한 `키-값` 저장 용도로 사용할 때 유용하다.

```javascript
let obj = Object.create(null);
obj.someKey = "someValue";

console.log(obj.someKey); // "someValue"
console.log(obj.toString); // undefined (Object.prototype을 상속받지 않음)
```

보통 `Object.prototype`에는 `toString`, `hasOwnProperty` 같은 메서드가 포함되어 있다.<br/>
하지만 `Object.create(null)`을 사용하면 이런 프로퍼티들이 포함되지 않아 예상치 못한 충돌을 방지할 수 있다.<br/><br/>
이런 특성 때문에 `Object.create(null)`은 맵(Map)과 유사한 객체를 만들 때 유용하다.<br/>
예를 들어, 키-값 저장용 객체를 만들 경우 다음과 같이 사용할 수 있다.<br/>

```javascript
let dictionary = Object.create(null);
dictionary.apple = "사과";
dictionary.orange = "오렌지";

console.log(dictionary.apple); // "사과"
console.log("toString" in dictionary); // false (Object.prototype을 상속받지 않음)
```

하지만 `Object.create(null)`로 만든 객체는 `Object.keys`, `for...in` 반복문에서 의도한 대로 **동작하지 않을 수 있으므로** 주의해야 한다.

### :fire: 정리

1. `__proto__`는 구식 방식이므로 더는 사용하지 않는 것이 좋으며, 대신 `Object.create`, `Object.getPrototypeOf`, `Object.setPrototypeOf` 같은 모던한 메서드를 사용해야 한다.
2. `Object.create(proto, descriptors)`를 사용하면 특정 프로토타입을 가지는 객체를 생성하면서 프로퍼티 설명자를 설정할 수 있다.
3. `Object.create(null)`을 사용하면 `__proto__`가 없는 객체를 생성할 수 있으며, `Object.prototype`의 영향을 받지 않아 키-값 저장 용도로 유용하다.
4. 하지만 `Object.create(null)`로 생성한 객체는 `Object.keys`, `for...in` 같은 반복문에서 예상과 다르게 동작할 수 있으므로 주의해야 한다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 프로토타입 메서드와 **proto**가 없는 객체](https://ko.javascript.info/prototype-methods)
