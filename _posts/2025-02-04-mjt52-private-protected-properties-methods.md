---
title: "[mjt] private, protected 프로퍼티와 메서드"
date: 2025-02-04
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
author_profile: true
sidebar_main: true
---

## :ledger: private, protected 프로퍼티와 메서드

### :one: 내부 인터페이스와 외부 인터페이스

객체 지향 프로그래밍에서 프로퍼티와 메서드는 두 그룹으로 나뉜다.

- `내부 인터페이스`
  - 동일한 클래스 내의 다른 메서드에선 접근할 수 있지만, 클래스 밖에서 접근할 수 없는 프로퍼티와 메서드
  - 커피 머신으로 비유하면 기계 안쪽에 숨어있는 뜨거운 물이 지나가는 관이나 발열 장치 등이 내부 인터페이스가 될 수 있다.
  - `private(#)` 또는 `protected(_)`를 사용하여 선언
  - 보통 데이터를 안전하게 보호하거나 내부 로직을 관리할 때 사용
- `외부 인터페이스`
  - 클래스 밖에서도 접근 가능한 프로퍼티와 메서드
  - 외부 인터페이스는 예시로 커피 머신 보호 커버에를 뜯지 않고 접근할 수 없기에 외부 인터페이스를 통해야만 내부 인터페이스 기능을 사용할 수 있다.
  - 보통 메서드를 통해 내부 데이터를 안전하게 조작할 수 있도록 함

### :two: 두 가지 타입의 객체 필드(프로퍼티와 메서드)

자바스크립트 이외의 다수 언어에서 클래스 자신과 자손 클래스에서만 접근을 허용하는 ‘protected’ 필드를 지원한다. `protected` 필드는 `private`과 비슷하지만, <u>자손 클래스에서도 접근이 가능하다는것이 다른점</u>이다. `protected` 필드도 내부 인터페이스를 만들 때 유용하며 자손 클래스의 필드에 접근해야 하는 경우가 많기 때문에, `protected` 필드가 좀 더 광범위하게 사용된다.

- `public`
  - 어디서든지 접근할 수 있으며 외부 인터페이스를 구성한다. 지금까지 다룬 프로퍼티와 메서드는 모두 `public`이다.
- `private`
  - 클래스 내부에서만 접근할 수 있으며 내부 인터페이스를 구성할 때 쓰인다.

### :three: 프로퍼티 보호하기

`private`과 `protected`를 사용하면 중요한 데이터를 보호할 수 있다. 하지만, 완전히 차단하는 것보다 안전한 방식으로 데이터를 노출하는 것이 중요하다.

- `protected (_salary)`: 외부에서는 직접 접근하지 않도록 암묵적인 규칙을 제공
- 하위 클래스에서만 값을 조작할 수 있도록 설계

```javascript
class Employee {
  constructor(name, salary) {
    this.name = name;
    this._salary = salary; // protected
  }

  getSalary() {
    return this._salary;
  }
}

class Manager extends Employee {
  increaseSalary(amount) {
    this._salary += amount;
    console.log(`급여가 ${amount}원 인상되었습니다.`);
  }
}

const manager = new Manager("Bob", 5000);
console.log(manager.getSalary()); // ✅ 5000
manager.increaseSalary(1000); // ✅ 급여가 1000원 인상되었습니다.
console.log(manager.getSalary()); // ✅ 6000

console.log(manager._salary); // ✅ 접근 가능하지만 권장되지 않음
```

### :four: 읽기 전용 프로퍼티

읽기 전용 프로퍼티는 초기화 후 값을 변경할 수 없도록 보호할 때 사용된다. 이를 위해 `getter`를 활용할 수 있다.

#### :pushpin: 4-1) getter를 이용한 읽기 전용 속성

- `getter`: 클래스 내부 데이터의 읽기 전용 속성 제공

```javascript
class Product {
  #price;

  constructor(name, price) {
    this.name = name;
    this.#price = price;
  }

  get price() {
    return this.#price;
  }
}

const item = new Product("노트북", 1500000);
console.log(item.price); // ✅ 1500000

item.price = 2000000; // ❌ Error: Cannot set property
```

#### :pushpin: 4-2) Object.freeze()를 활용한 읽기 전용 객체

- `Object.freeze()`: 객체의 속성을 변경 불가능하게 설정

```javascript
const settings = Object.freeze({
  apiUrl: "https://api.example.com",
  maxConnections: 5,
});

console.log(settings.apiUrl); // ✅ "https://api.example.com"

settings.apiUrl = "https://new-api.com"; // ❌ 변경 불가능
```

### :fire: 정리

- `private (#) vs protected (_)`
  - **#private**: 클래스 내부에서만 접근 가능 (외부 접근 불가능)
  - **\_protected**: 직접 접근 가능하지만, 하위 클래스에서만 사용하도록 설계
- `내부 인터페이스와 외부 인터페이스`
  - **내부 인터페이스**: `private`, `protected` 멤버로 정보 은닉
  - **외부 인터페이스**: `public` 메서드로만 접근 허용
- `데이터 보호 방법`
  - `private` 프로퍼티로 직접 접근 차단
  - `protected` 프로퍼티로 하위 클래스에서만 조작 가능
  - `getter`를 활용하여 읽기 전용 속성 제공

#### :pushpin: 참고 문서

- [ko.javascript.info - private, protected 프로퍼티와 메서드](https://ko.javascript.info/private-protected-properties-methods)
