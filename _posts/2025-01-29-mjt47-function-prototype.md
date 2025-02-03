---
title: "[mjt] 함수의 prototype 프로퍼티에 대해 알아보자"
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

자바스크립트에서 모든 함수는 자동으로 생성되는 `prototype` 프로퍼티를 가진다.
이 `prototype` 프로퍼티는 생성자 함수와 함께 동작하며, 객체 지향 프로그래밍에서 상속을 구현하는 핵심 요소다.

### :one: 함수의 디폴트 프로퍼티: prototype

- 자바스크립트에서 함수(**특히 생성자 함수**)는 `prototype`이라는 기본 프로퍼티를 가진다.
- 이를 통해 `프로토타입 체인(prototype chain)` 이 형성되어 객체가 속성과 메서드를 상속받을 수 있다.

```javascript
function Person() {}

console.log(Person.prototype);
// { constructor: ƒ Person() }
```

#### :pushpin: 1-1) 생성자 함수와 prototype 관계

- `Rabbit.prototype = animal`은 "new Rabbit을 호출해 만든 새로운 객체의 [[Prototype]]을 animal로 설정하라."는 것을 의미한다.
- 즉 `rabbit`이 `animal`을 상속받았다는 것을 의미한다.

```javascript
let animal = {
  eats: true,
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("흰 토끼"); //  rabbit.__proto__ == animal

alert(rabbit.eats); // true
```

#### :pushpin: 1-2) F.prototype은 new F를 호출할 때만 사용된다.

`F.prototype` 프로퍼티는 `new F`를 호출할 때만 사용되며 `new F`를 호출할 때 만들어지는 새로운 객체의 `[[Prototype]]`을 할당해준다.<br/>
새로운 객체가 만들어진 후에 `F.prototype` 프로퍼티가 바뀌면(`F.prototype = <another object>`) `new F`를 호출해 만드는 또 다른 새로운 객체는 another object를 `[[Prototype]]`으로 갖게 되며 기존에 있던 객체의 `[[Prototype]]`은 유지된다.

### :two: 함수의 디폴트 프로퍼티 prototype과 constructor 프로퍼티

개발자가 특별히 할당하지 않더라도 모든 함수는 기본적으로 `prototype` 프로퍼티를 갖는다.

- 디폴트 프로퍼티 `prototype`은 `constructor` 프로퍼티 하나만 있는 객체를 가르키는데, 여기서 `constructor` 프로퍼티는 함수 자신을 가르킨다.

```javascript
function Rabbit() {}

/* 
  디폴트 prototype
  Rabbit.prototype = { constructor: Rabbit };
*/
```

#### :pushpin: 2-1) 예시를 통해 확인

```javascript
function Rabbit() {}
// 함수를 만들기만 해도 디폴트 프로퍼티인 prototype이 설정된다.
// Rabbit.prototype = { constructor: Rabbit }

alert(Rabbit.prototype.constructor == Rabbit); // true
```

특별한 조작을 하지 않았다면 `new Rabbit`을 실행해 만든 토끼 객체 모두에서 `constructor` 프로퍼티를 사용할 수 있는데, 이때 `[[Prototype]]`을 거친다.

```javascript
function Rabbit() {}
// 디폴트 prototype:
// Rabbit.prototype = { constructor: Rabbit }

let rabbit = new Rabbit(); // {constructor: Rabbit}을 상속받음

alert(rabbit.constructor == Rabbit); // true ([[Prototype]]을 거쳐 접근함)
```

#### :pushpin: 2-2) 새로운 객체를 생성

`constructor` 프로퍼티는 기존에 있던 객체의 `constructor`를 사용해 새로운 객체를 만들떄 사용할 수 있다.

```javascript
function Rabbit(name) {
  this.name = name;
  alert(name);
}

let rabbit = new Rabbit("흰 토끼");

let rabbit2 = new rabbit.constructor("검정 토끼");
```

이 방법은 객체가 있는데 이 객체를 만들 때 어떤 생성자가 사용되었는지 알 수 없는 경우 유용하게 쓸 수 있다.

#### :pushpin: 2-3) 자바스크립트는 알맞는 constructor 값을 보장하지 않는다.

함수엔 기본적으로 `prototype`이 설정된다라는 사실이 전부이며 `constructor`와 관련해서 벌어지는 모든 일은 전적으로 개발자에게 달려있다.<br/>
함수에 기본으로 설정되는 `prototype` 프로퍼티 값을 다른 객체로 바꿔 무슨 일이 일어났는지 아래의 예씨를 통해 알아보자

```javascript
// 예시
function Rabbit() {}
Rabbit.prototype = {
  jumps: true,
};

let rabbit = new Rabbit();
alert(rabbit.constructor === Rabbit); // false

// 이런 상황을 방지하고 constructor의 기본 성질을 제대로 활용하려면 prototype에 뭔가를 하고 싶을 때
// prototype 전체를 덮어쓰지 말고 디폴트 prototype에 원하는 프로퍼티를 추가, 제거 해야한다.
function Rabbit() {}

// Rabbit.prototype 전체를 덮어쓰지 말고
// 원하는 프로퍼티가 있으면 그냥 추가한다.
Rabbit.prototype.jumps = true;
// 이렇게 하면 디폴트 프로퍼티 Rabbit.prototype.constructor가 유지되.

// 실수로 prototype을 덮어썻다 해도 constructor 프로퍼티를 수동으로 다시 만들어주면 constructor를 다시 사용할 수 있다.
Rabbit.prototype = {
  jumps: true,
  constructor: Rabbit,
};

// 수동으로 constructor를 추가해 주었기 때문에 우리가 알고 있던 constructor의 특징을 그대로 사용할 수 있다.
```

### :fire: 요약

- 생성자 함수에 기본으로 세팅되는 프로퍼티(F.prototype)는 [[Prototype]]과 다르다. `F.prototype`은 `new F()`를 호출할 때 만들어지는 새로운 객체의 [[Prototype]]을 설정한다.
- `F.prototype`의 값은 객체나 `null`만 가능합니다. 다른 값은 무시된다.
- 지금까지 배운 내용은 생성자 함수를 `new`를 사용해 호출할 때만 적용된다.
- 모든 함수는 기본적으로 `F.prototype = { constructor : F }`를 가지고 있으므로 `"constructor"` 프로퍼티를 사용하면 객체의 생성자를 얻을 수 있다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 함수의 prototype 프로퍼티](https://ko.javascript.info/function-prototype)
