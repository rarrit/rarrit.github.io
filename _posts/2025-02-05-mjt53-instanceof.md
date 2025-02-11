---
title: "[mjt] instanceof로 클래스 확인하기"
date: 2025-02-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - class
  - instanceof
author_profile: true
sidebar_main: true
---

## :ledger: instanceof로 클래스 확인하기

### :one: instanceof 연산자

`instanceof` 연산자를 사용하면 객체가 특정 클래스에 속하는지 아닌지를 확인할 수 잇다. `instanceof`는 상속 관계도 확인해준다.

#### :pushpin: 1-1) 기본 문법

1. `dog instanceof Animal` → dog 객체가 Animal을 기반으로 만들어졌는지 확인
2. `dog instanceof Object` → 모든 객체는 Object를 상속받으므로 true

```javascript
class Animal {}

const dog = new Animal();

console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true (모든 객체는 Object의 인스턴스)
```

### :two: 상속과 instanceof

`instanceof`는 상속 관계도 확인할 수 있다.

- `Dog` 클래스는 `Animal`을 상속받았으므로, `myDog instanceof Animal`은 `true`를 반환한다.
- 모든 객체는 `Object를` 기반으로 하므로 instanceof Object는 `true`다.

```javascript
class Animal {}
class Dog extends Animal {}

const myDog = new Dog();

console.log(myDog instanceof Dog); // true
console.log(myDog instanceof Animal); // true (상속받았기 때문)
console.log(myDog instanceof Object); // true
```

### :three: instanceof와 커스텀 클래스

클래스가 아닌 함수를 이용한 생성자 함수에서도 `instanceof`를 사용할 수 있다.

```javascript
function Car() {}
const myCar = new Car();

console.log(myCar instanceof Car); // true
console.log(myCar instanceof Object); // true
```

### :four: instanceof가 오작동하는 경우

#### :pushpin: 4-1) 다른 실행 환경 (iframe, Worker)

객체가 다른 실행 환경에서 생성된 경우 `instanceof`가 예상과 다르게 동작할 수 있습니다.

```javascript
const iframe = document.createElement("iframe");
document.body.appendChild(iframe);

const obj = new iframe.contentWindow.Object();
console.log(obj instanceof Object); // false (다른 환경에서 생성됨)

// 해결 방법: Object.prototype.toString.call() 사용
console.log(Object.prototype.toString.call(obj)); // "[object Object]"
```

### :five: instanceof 대신 Symbol.hasInstance 활용

클래스의 `Symbol.hasInstance` 메서드를 오버라이드하면 `instanceof`의 동작을 커스텀할 수 있다.

- `Symbol.hasInstance`를 정의하면 `instanceof` 동작을 변경할 수 있음
- 특정 조건을 만족하는 객체만 `instanceof` 검사에서 `true`를 반환하게 설정 가능

```javascript
class CustomChecker {
  static [Symbol.hasInstance](instance) {
    return instance.canUseInstanceof === true;
  }
}

const obj1 = { canUseInstanceof: true };
const obj2 = { canUseInstanceof: false };

console.log(obj1 instanceof CustomChecker); // true
console.log(obj2 instanceof CustomChecker); // false
```

### :fire: 정리

- `instanceof란?`
  - 객체가 특정 클래스(또는 생성자 함수)의 인스턴스인지 확인하는 연산자
- `상속 관계 확인 가능`
  - 부모 클래스까지 확인하므로 instanceof는 상속된 클래스에도 적용됨
- `iframe, Worker 등에서 주의 필요`
  - 다른 실행 환경에서 생성된 객체는 instanceof가 예상과 다르게 동작할 수 있음
  - Object.prototype.toString.call(obj)로 확인하는 것이 더 안전할 수 있음
- `Symbol.hasInstance로 커스텀 가능`
  - 특정 조건을 만족하는 객체만 instanceof 검사에서 true를 반환하도록 설정 가능

#### :pushpin: 참고 문서

- [ko.javascript.info - 'instanceof'로 클래스 확인하기](https://ko.javascript.info/instanceof)
