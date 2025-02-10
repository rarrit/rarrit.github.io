---
title: "[mjt] 정적 메서드와 정적 프로퍼티"
date: 2025-02-03
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
  - class inheritance
author_profile: true
sidebar_main: true
---

## :ledger: 정적 메서드와 정적 프로퍼티

### :one: 정적 메서드

- 정적 메서드는 `static` 키워드를 사용하여 클래스에 직접 정의된다.
- 이러한 메서드는 인스턴스를 생성하지 않고도 호출할 수 있으며, `this`는 해당 클래스 자체를 가리킨다.

#### :pushpin: 1-1) 예제

- `User.greet()`는 인스턴스를 생성하지 않고 바로 호출할 수 있습니다.
- `static`이 없으면 `new User().greet()`처럼 인스턴스를 생성해야 합니다.

```javascript
class User {
  static greet() {
    return "Hello!";
  }
}

console.log(User.greet()); // "Hello!"
```

#### :pushpin: 1-2) 특징

- 인스턴스에서는 호출할 수 없음
- 클래스 내부에서만 사용 가능
  - 정적 메서드는 인스턴스 속성을 참조할 수 없다.
  - `this`는 클래스 자체를 가리킨다.

```javascript
const user = new User();
console.log(user.greet()); // TypeError: user.greet is not a function
```

### :two: 정적 프로퍼티

- 정적 프로퍼티도 `static` 키워드를 사용하여 선언된다.
- 이는 클래스의 특정 정보(상수, 설정값 등)를 저장할 때 유용합니다.

#### :pushpin: 2-1) 예제

정적 프로퍼티는 인스턴스가 아닌 **클래스 자체**에 저장된다.

```javascript
class User {
  static role = "admin";
}

console.log(User.role); // "admin"
```

#### :pushpin: 2-2) 특징

1. 인스턴스에서는 접근할 수 없음
2. 클래스명으로 직접 접근해야 함

```javascript
// 인스턴스에서는 접근할 수 없음
const user = new User();
console.log(user.role); // undefined

// 클래스명으로 직접 접근해야 함
console.log(User.role); // "admin"
```

### :three: 정적 프로퍼티와 메서드 상속

정적 멤버는 상속될 수 있다. 즉, 부모 클래스의 정적 메서드와 정적 프로퍼티를 자식 클래스에서 사용할 수 있다.

#### :pushpin: 3-1) 정적 메서드 상속 예시

`Child` 클래스는 `Parent` 클래스의 정적 메서드를 상속받아 사용할 수 있다.

```javascript
class Parent {
  static hello() {
    return "Hello from Parent";
  }
}

class Child extends Parent {}

console.log(Child.hello()); // "Hello from Parent"
```

#### :pushpin: 3-2) 정적 프로퍼티 상속 에시

정적 프로퍼티도 상속된다.

```javascript
class Parent {
  static type = "Base";
}

class Child extends Parent {}

console.log(Child.type); // "Base"
```

### :four: 정적 메서드 오버라이딩

자식 클래스에서 부모 클래스의 정적 메서드를 **오버라이딩(재정의)** 할 수도 있다

```javascript
class Parent {
  static greet() {
    return "Hello from Parent";
  }
}

class Child extends Parent {
  static greet() {
    return "Hello from Child";
  }
}

console.log(Parent.greet()); // "Hello from Parent"
console.log(Child.greet()); // "Hello from Child"
```

### :fire: 정리

- `정적 메서드`
  - static 키워드로 선언
  - 클래스에서 직접 호출 가능
  - 인스턴스에서 호출 불가능
- `정적 프로퍼티`
  - static 키워드로 선언
  - 클래스에 저장됨
  - 인스턴스에서는 접근 불가능
- `정적 멤버 상속`
  - 자식 클래스에서 부모 클래스의 정적 멤버를 사용할 수 있음
  - 정적 메서드 및 프로퍼티는 오버라이딩 가능

#### :pushpin: 참고 문서

- [ko.javascript.info - 정적 메서드와 정적 프로퍼티](https://ko.javascript.info/static-properties-methods#ref-1302)
