---
title: "[mjt] 자바스크립트의 new Function 문법을 알아보자"
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
  - new function
author_profile: true
sidebar_main: true
---

## :ledger: 자바스크립트의 new Function 문법을 알아보자

`JavaScript`의 `new Function` 생성자 문법에 대해 알아보자

### :one: new Function이란?

`new Function`은 `JavaScript`에서 동적으로 함수를 생성할 수 있는 생성자다.

- `arg1, arg2, ...argN`: 함수의 매개변수 일므을 문자열로 작성한다.
- `functionBody`: 함수 본문을 문자열로 작성한다.

```javascript
let func = new Function([arg1, arg2, ...argN], functionBody);

// 예시
let sum = new Function("a", "b", "return a + b;");
console.log(sum(1, 2)); //3
```

### :two: new Function의 특징

- `런타임에 동적으로 함수 생성한다.`
  - 코드 실행 중 문자열을 바탕으로 새로운 함수를 정의할 수 있다.
- `글로벌 스코프에서 생성`
  - `new Function`으로 만든 함수는 항상 전역 스코프를 기준으로 작동한다. 이는 함수가 생성된 위치에 상관없이, 해당 함수 내부에서 전역 변수만 접근 가능하다는 것이다.
- `동적 코드 실행`
  - 함수 본문을 문자열로 정의하므로 동적으로 코드를 실행할 수 있다.

```javascript
let value = 10;

function createFunc() {
  let value = 20;
  return new Function("return value;");
}

let func = createFunc();
console.log(func()); // 10 (전역 변수 value)
```

### :three: 사용 시 주의 사항

- `보안 취약점`
  - `new Function`은 문자열을 기반으로 동작하므로, 악의적인 코드를 삽입당할 위험이 있다. 외부 입력을 절대 함수 본문에 포함하지 않도록 주의해야함
- `디버깅이 어려움`
  - 함수 본문이 문자열로 정의되기 때문에 에러 추적이 어렵다.
- `성능 문제`
  - 동적 코드 실행은 일반적인 함수 정의 방식보다 성능이 떨어질 수 있음

```javascript
let userInput = "alert('악성 코드 실행!')";
let func = new Function(userInput);
func(); // 악성 코드가 실행될 수 있음
```

### :four: 그럼 언제 사용하면 좋을까?

- 런타임에 사용자 입력을 기반으로 함수를 정의해야 할 때
- JSON 또는 다른 형식으로 저장된 코드를 실행해야 할 때
- 스크립트 생성 툴이나 코드 빌더를 개발할 때

### :fire: 회고

`new Function`은 JavaScript의 강력한 기능 중 하나로, 동적으로 함수를 생성할 수 있는 유연성함이 특징이다. 하지만 보안, 성능, 디버깅 측면에서 단점도 존재하므로 신중하게 사용해야 된다.

#### :pushpin: 참고 문서

- [ko.javascript.info - new Function 문법](https://ko.javascript.info/new-function)
