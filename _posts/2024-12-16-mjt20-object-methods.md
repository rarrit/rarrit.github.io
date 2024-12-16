---
title: "[mjt] 메서드와 this에 대해 알아보자"
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
  - object-methods
author_profile: true
sidebar_main: true
---

## :ledger: 메서드와 this에 대해 알아보자

자바스크립트에서 객체는 데이터와 `행동(메서드)`을 함께 담는 그릇이다. 메서드는 객체의 프로퍼티로 할당된 함수로, `this`를 통해 객체의 데이터를 어떻게 다루는지 알아보도록 하자.

### :one: 메서드란?

- 메서드는 객체의 프로퍼티로 정의된 함수를 의미한다.
- 객체 내부에서 데이터를 처리하거나 동작을 수행할 때 사용된다.

#### :pushpin: 1-1) 메서드 만들기

아래의 예시로 `user`에게 인사할 수 있는 행동을 부여해준다.

- 함수 표현식으로 함수를 만들고, 객체 프로퍼티 `user.sayHi`에 함수를 할당해준다.
- `user`에 할당된 `sayHi`가 메서드다.

```javascript
let user = {
  name: "Ming",
  age: 30,
};

user.sayHi = function () {
  alert("hi ~");
};

user.sayHi(); // "hi ~"
```

#### :pushpin: 1-2) 메서드 단축 구문

객체 리터럴 안에 메서드를 선언할 때 사용할 수 있는 단축 문법이다.

```javascript
// 아래 두 객체는 동일하게 동작한다.
user = {
  sayHi: function () {
    alert("Hello");
  },
};

// 단축 구문을 사용
user = {
  sayHi() {
    // "sayHi: function()"과 동일합니다.
    alert("Hello");
  },
};
```

### :two: 메서드와 this

- 메서드는 객체에 저장된 정보에 접근할 수 있어야 제 역할을 할 수 있다.
- 대부분의 메서드가 객체 프로퍼티의 값을 활용한다. (모든 메서드가 그렇진 않음)

#### :pushpin: 2-1) 메서드 내부에서의 This

메서드 내부에서 `this` 키워드를 사용하면 객체에 접근할 수 있다.

```javascript
let user = {
  name: "ming",
  age: 30,
  sayHi() {
    // 'this'는 '현재 객체'를 나타냄
    alert(this.name);
  },
};
user.sayHi(); // ming
```

#### :pushpin: 2-2) 외부 변수를 사용해 객체를 참조할 경우

`user`를 복사해 다른 변수에 할당(`admin = user`)하고, `user`는 전혀 다른 값으로 덮어씌여진 경우 `sayHi()`는 원치 않는 값(null)을 참조한다.

- `alert` 함수가 `user.name`이 아닌 `this.name`을 인수로 받았다면 에러가 발생하지 않는다.

```javascript
let user = {
  name: "ming",
  age: 30,
  sayHi() {
    alert(user.name);
  },
};

let admin = user;
user = null;

admin.sayHi();
```

### :three: 자유로운 this

- 자바스크립트의 `this`는 다른 프로그래밍 언어의 `this`와 동작 방식이 다르다.
- 자바스크립트에선 모든 함수에 `this`를 사용할 수 있다.
- `this`의 값은 런타임에 결정된다. (컨텍스트에 따라 달라짐)
- 동일한 함수라도 다른 객체에서 호출했다면, `this`가 참조하는 값이 달라진다.

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert(this.name);
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin["f"](); // Admin (점과 대괄호는 동일하게 동작함)
```

#### :pushpin: 3-1) 일반 함수에서의 this

브라우저 환경에서는 window, 엄격 모드(strict mode)에서는 undefined를 반환한다.

```javascript
function sayHi() {
  console.log(this);
}
sayHi(); // window or undefined
```

#### :pushpin: 3-2) 메서드에서의 this

메서드를 호출한 객체를 참조한다.

```javascript
let user = {
  name: "ming",
  greet() {
    console.log(this.name);
  },
};

user.greet(); // ming
```

#### :pushpin: 3-3) 콜백에서의 this

콜백 함수에서 `this`가 원래 객체를 잃어버리는 경우가 많다.

```javascript
let user = {
  name: "ming",
  greet() {
    setTimeout(function () {
      console.log(this.name);
    }, 1000);
  },
};

user.greet(); // ming
```

#### :pushpin: 3-4) 화살표 함수의 this

화살표 함수는 `this`를 자신의 상위 컨텍스트에 바인딩한다.

```javascript
let user = {
  name: "ming",
  greet() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  },
};

user.greet(); // ming
```

#### :pushpin: 3-5) 클래스에서의 this

클래스에서 `this`는 인스턴스를 참조한다.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

let user = new User("ming");
user.greet(); // Hello, my name is ming
```

### :four: 자주 발생하는 문제

#### :pushpin: 4-1) 메서드를 변수에 할당했을 때

메서드를 변수에 할당하면 `this`가 undefined 인 경우가 있다.

```javascript
let user = {
  name: "ming",
  greet() {
    console.log(this.name);
  },
};

let greet = user.greet;
greet(); // undefined (this가 전역 객체를 참조)
```

이럴 경우 `bind`를 사용하면 문제가 해결된다.

```javascript
let greetBound = user.greet.bind(user);
greetBound();
("ming");
```

#### :pushpin: 4-2) 클래스에서 메서드를 콜백으로 전달할 때

클래스에서 `this`를 사용하고 메서드를 콜백으로 전달할 때 `this`가 undefined 경우가 있다.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(this.name);
  }
}

let user = new User("ming");
let greet = user.greet;
greet(); // undefined
```

이럴 경우에도 `bind`를 사용하면 된다.

```javascript
let greet = user.greet.bind(user);
greet(); // ming
```

### :fire: 회고

자바스크립트에서 `this`는 정말 헷갈리는 부분 중 하나이다. 사용 방식에 따라 결과가 달라지며, 메서드를 타른 컨텍스트에서 호출하거나 콜백으로 전달할 때 `this`가 달라지는 것을 주의해야될 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 메서드와 this](https://ko.javascript.info/object-methods)
