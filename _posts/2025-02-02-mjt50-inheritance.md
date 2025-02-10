---
title: "[mjt] 클래스 상속에 대해 알아보자"
date: 2025-02-02
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

## :ledger: 클래스 상속에 대해 알아보자

`클래스 상속(inheritance)`은 기존 클래스를 확장하여 새로운 클래스를 만드는 기능이다. 이를 통해 코드의 재사용성을 높이고 유지보수를 쉽게 할 수 있다.

### :one: 문법

#### :pushpin: 1-1) extends

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`안녕하세요, 저는 ${this.name}입니다.`);
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name); // 부모 클래스의 생성자 호출
    this.age = age;
  }
  introduce() {
    console.log(`저는 ${this.name}, ${this.age}살입니다.`);
  }
}

const child = new Child("홍길동", 20);
child.greet(); // 안녕하세요, 저는 홍길동입니다.
child.introduce(); // 저는 홍길동, 20살입니다.
```

#### :pushpin: 1-2) super

- `super()`는 부모 클래스의 생성자를 호출할 때 사용한다.
- `super.method()`를 사용하면 부모 클래스의 메서드를 호출할 수 있다.

```javascript
class Parent {
  hello() {
    console.log("부모 클래스의 hello 메서드");
  }
}

class Child extends Parent {
  hello() {
    super.hello(); // 부모 클래스의 메서드 호출
    console.log("자식 클래스의 hello 메서드");
  }
}

const child = new Child();
child.hello();
// 부모 클래스의 hello 메서드
// 자식 클래스의 hello 메서드
```

### :two: 메서드 오버라이딩

자식 클래스는 부모 클래스의 메서드를 동일한 이름으로 재정의(오버라이딩)할 수 있다.

```javascript
class Parent {
  sayHi() {
    console.log("부모 클래스의 인사");
  }
}

class Child extends Parent {
  sayHi() {
    console.log("자식 클래스의 인사");
  }
}

const child = new Child();
child.sayHi(); // 자식 클래스의 인사
```

### :three: 생성자 오버라이딩

자식 클래스에서 생성자를 재정의(오버라이딩)할 수도 있다. 이때, `super()`를 사용하여 부모 클래스의 생성자를 호출해야 한다.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // 부모 클래스의 생성자 호출
    this.breed = breed;
  }
}

const dog = new Dog("바둑이", "진돗개");
console.log(dog.name); // "바둑이"
console.log(dog.breed); // "진돗개"
```

### :four: 클래스 필드 오버라이딩

부모 클래스에서 정의된 필드를 자식 클래스에서 동일한 이름으로 다시 정의하여 값을 변경할 수 있다.

```javascript
class Parent {
  name = "부모";
}

class Child extends Parent {
  name = "자식";
}

const child = new Child();
console.log(child.name); // "자식"
```

### :fire: 정리

1. `extends` 키워드를 사용하여 클래스를 상속할 수 있다.
2. `super()`를 호출하여 부모 클래스의 생성자를 실행해야 한다.
3. 부모 클래스의 메서드를 자식 클래스에서 재정의(오버라이딩)할 수 있다.
4. 부모 클래스의 필드도 자식 클래스에서 오버라이딩할 수 있다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 클래스 상속](https://ko.javascript.info/class-inheritance)
