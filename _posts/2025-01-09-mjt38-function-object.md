---
title: "[mjt] 객체로서의 함수와 기명 함수 표현식을 알아보자"
date: 2025-01-09
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - function
  - object
author_profile: true
sidebar_main: true
---

## :ledger: 객체로서의 함수와 기명 함수 표현식을 알아보자

`JavaScript`에서 함수는 단순히 호출 가능한 코드 블록이다. 객체로서의 특성과 **기명 함수 표현식**이라는 방식으로 유연하게 활용된다. 무엇인지 알아보자.

### :one: 함수는 객체다.

`JavaScript`에서 함수는 객체다. 즉, 함수는 값으로 취급될 수 있으며 프로퍼티를 추가하거나 메서드를 호출할 수 있다.

#### :pushpin: 1-1) 함수의 객체적 특성

- `프로퍼티 추가 가능`
  - 함수에 프로퍼티를 추가하여 데이터를 저장할 수 있다.
- `함수의 메서드 활용 가능`
  - `call`, `apply`, `bind` 등의 메서드를 통해 동적으로 함수의 동작을 변경할 수 있다.

#### :pushpin: 1-2) 예시 1: 함수에 프로퍼티 추가

```javascript
// #01 프로퍼티 추가
function greet() {
  console.log("Hello!");
}

// 프로퍼티 추가
greet.language = "English";
console.log(greet.language);
```

#### :pushpin: 1-3) 예시 2: 함수도 객체임을 확인하기

```javascript
function example() {}

console.log(typeof example); // 출력: function
console.log(example instanceof Object); // 출력: true
```

#### :pushpin: 1-4) 예시 3: 매개변수 개수 확인

```javascript
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {}

alert(f1.length); // 1
alert(f2.length); // 2
alert(many.length); // 2
```

#### :pushpin: 1-5) 예시 4: 커스텀 프로퍼티

함수에 자체적으로 만든 프로퍼티를 추가가 가능하다.

- 프로퍼티는 변수가 아니다.
  - `sayHi.counter = 0`와 같이 프로퍼티를 할당해도 함수 내에 지역변수 `counter`가 만들어지지 않는다.
  - `counter` 프로퍼티와 변수 `let counter`는 전혀 관계가 없다.
  - 프로퍼티를 저장하는 것처럼 함수를 객체처럼 다룰 수 있지만 실행에 영향을 끼치지 않는다.
    - 변수는 함수 프로퍼티가 아니고 함수 프로퍼티는 변수가 아니기 때문이다. 둘 사이에 공통점은 없다.

```javascript
function sayHi() {
  alert("Hi");

  // 함수를 몇 번 호출했는지 세봅시다.
  sayHi.counter++;
}
sayHi.counter = 0; // 초깃값

sayHi(); // Hi
sayHi(); // Hi

alert(`호출 횟수: ${sayHi.counter}회`); // 호출 횟수: 2회
```

### :two: 기명 함수 표현식

**기명 함수 표현식(Named Function Expression, NFE)**은 이름이 있는 함수 표현식을 나타내는 용어다.

```javascript
// 일반 함수 표현식
let sayHi = function (who) {
  alert(`Hello, ${who}`);
};

// 이름 'func'를 붙여줌
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```

#### :pushpin: 2-1) 왜 사용함?

위와 같이 일반 함수 표현식에 이름을 붙여줘도 여전히 <u>함수 표현식</u>이라는 점을 알아야한다. `function` 뒤에 `func`라는 이름을 붙이더라도 여전히 표현식을 할당한 형태를 유지하기에 함수 선언문으로 바뀌지는 않는다. 변경된 점은 아래와 같다.

- 이름을 사용해서 함수 표현식 내부에서 자기 자신을 참조할 수 있음
- 기명 함수 표현식 외부에선 그 이름을 사용할 수 없음

```javascript
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // func를 사용해서 자신을 호출함.
  }
};

sayHi(); // Hello, Guest

// 하지만 아래와 같이 func를 호출하는 건 불가능.
func(); // Error, func is not defined (기명 함수 표현식 밖에서는 그 이름에 접근할 수 없다.)
```

위의 함수에서 `sayHi`는 `who`에 값이 없는 경우, 인수 `"Guest"`를 받고 자기 자신을 호출한다. 하지만 이러한 방법은 대부분 아래와 같이 `if`문을 통해 사용하게 되는데 그렇게 되면 외부 코드에 의해 `sayHi`가 변경될 수 있다는 문제가 생긴다.

```javascript
// if문을 통해 사용할 경우 외부 코드에 의해 sayHi가 변경될 수 있음
let sayHi = function (who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    sayHi("Guest");
  }
};

// 기명 함수 표현식을 사용하면 이러한 문제를 해결할 수 있음
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // 원하는 값이 제대로 출력됩니다.
  }
};

let welcome = sayHi;
sayHi = null;

welcome(); // Hello, Guest (중첩 호출이 제대로 동작함)
```

#### :pushpin: 2-2) 함수 선언문엔 내부 이름을 지정할 수 없다.

즉, 개발을 하다 믿을만한 내부 이름이 필요할 경우 `기명 함수 표현식`으로 사용하면 좋다.

- `내부 이름`은 함수 표현식에서만 사용이 가능함
- 함수 선언문에는 사용이 불가능
  - 함수 선언문엔 `내부` 이름을 지정하는 문법이 없음

### :fire: 회고

- `name`: 함수의 이름이 저장되어 디버깅과 코드 가독성을 높이는 데 유용하다.
- `length`: 함수가 선언부에서 기대하는 인수의 수를 나타내며, 나머지 매개변수는 제외된다.
- `기명 함수 표현식(NFE)`: 재귀 호출이나 함수 내부에서 자기 자신을 참조할 때 활용된다.
- 개발을 하다 믿을만한 내부 이름이 필요할 경우 `기명 함수 표현식`으로 사용하면 좋다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 객체로서의 함수와 기명 함수 표현식](https://ko.javascript.info/global-object)
