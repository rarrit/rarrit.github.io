---
title: "[mjt] new 연산자와 생성자 함수에 대해 알아보자"
date: 2024-12-16
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - constructor
  - new
author_profile: true
sidebar_main: true
---

## :ledger: 유사한 객체를 여러개 만들어야할 땐? new 연산자와 생성자 함수를 사용하자.

객체 리터럴 `{...}`을 사용하면, 객체를 쉽게 만들 수 있다. 하지만 유사한 객체를 여러개 만들 때 `new` 연산자와 `생성자 함수`를 사용하면 유사한 객체 여러개를 쉽게 만들 수 있다.

### :one: 생성자 함수

- 생성자 함수(constructor function)와 일반 함수에 기술적인 차이는 없다.
- 생성자 함수는 아래의 두 관례를 따른다.
  - 함수의 이름의 첫 글자는 대문자로 시작한다.
  - 반드시 `new` 연산자를 붙여 실행한다.

```javascript
function User(name) {
  // this = {}; (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this; (this가 암시적으로 반환됨)
}

let user = new User("민규");

alert(user.name); // 민규
alert(user.isAdmin); // false
```

위 코드의 동작은 아래와 같다.

1. 빈 객체를 만들어 `this`에 핟랑한다.
2. 함수 본문을 실행한다. `this`에 새로운 프로퍼티를 추가해 `this`를 수정한다.
3. `this`를 반환한다.

### :two: new를 사용하지 않으면?

`new`없이 생성자 함수를 호출하면, `this`는 전역 객체(`window` 또는 `global`)를 참조하게 된다. 그렇게 되면 의도하지 않은 동작이나 에러가 발생할 수 있다.

```javascript
function User(name) {
  this.name = name;
}
let user = User("ming"); // new 없이 호출
console.log(user); // undefined;
console.log(window.name); // ming (전역 객체에 프로퍼티가 추가됨)
```

해결하는 방법은 `new`를 강제로 적용하는 방법이 있다.

```javascript
function user(name) {
  if (!new.target) {
    // new 없이 호출 되었다면
    return new User(name); // new를 강제로 적용
  }

  this.name = name;
}

let user = user("ming");
console.log(user.name); // ming
```

### :three: 생성자와 return 문

생성자 함수엔 보통 `return`문이 없다. 반환해야 할 것은 모두 `this`에 저장되고, `this`는 자동적으로 반환되기 때문에 반환문을 명시적으로 써 줄 필요가 없다.

- `return`문이 있을 경우엔?
  - 객체를 `return` 한다면 `this` 대신 객체가 반환된다.
  - 원시형을 `return` 한다면 `return`문이 무시된다.

`return` 뒤에 객체가 오면 생성자 함수는 해당 객체를 반환해주고, 이외의 경우에는 `this`가 반환된다.

```javascript
// 유형 1
function BigUser() {
  this.name = "원숭이";
  return { name: "고릴라" }; // <-- this가 아닌 새로운 객체를 반환함
}

alert(new BigUser().name); // 고릴라

// 유형 2
function SmallUser() {
  this.name = "원숭이";
  return; // <-- this를 반환함
}

alert(new SmallUser().name); // 원숭이
```

### :four: 생성자 내 메서드

생성자 함수를 사용하면 매개변수를 이용해 객체 내부를 자유롭게 구성할 수 있다.

```javascript
function User(name) {
  this.name = name;
  this.sayHi = function () {
    alert(`제 이름은 ${this.name} 입니다.`);
  };
}

let ming = new User("밍");
bora.sayHi(); // 제 이름은 밍입니다.
```

### :fire: 회고

- `생성자 함수`는 유사한 객체를 쉽게 만들기 위한 도구이다.
  - 다른 일반 함수와 구분하기 위해 함수의 이름 첫 글자를 대문자로 한다.
  - 반드시 `new` 연산자와 함께 호출해야한다. `new`와 함께 호출하면 내부에서 `this`가 암시적으로 만들어지고 마지막엔 `this`를 반환
- `new` 연산자는 생성자 함수와 결합하여 객체 생성 과정을 자동으로 처리한다.

#### :pushpin: 참고 문서

- [ko.javascript.info - new 연산자와 생성자 함수](https://ko.javascript.info/constructor-new)
