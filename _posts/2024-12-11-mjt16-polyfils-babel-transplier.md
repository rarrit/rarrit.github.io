---
title: "[mjt] 폴리필과 바벨, 트랜스파일러란 무엇인지 알아보자"
date: 2024-12-11
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
  - development
tags:
  - testing-mocha
author_profile: true
sidebar_main: true
---

## :ledger: 폴리필과 바벨, 트랜스파일러란 무엇인지 알아보자

프론트엔드 개발을 하다보면 `폴리필`, `바벨`, `트랜드파일러` 같은 용어를 접하게된다. 이 개념들이 무엇인지 어떤 역할을 하는지 알아보자

### :one: 폴리필(Polyfill)란 무엇인가?

`폴리필`은 브라우저가 <u>특정 기능(메서드, API 등)을 지원하지 않을 때</u>, 이를 **흉내 내어 사용할 수 있도록 제공**하는 코드이다.

- 예를 들어, 오래된 브라우저는 최신 자바스크립트 표준에 정의된 `Array.prototype.includes()`를 지원하지 않을 수 있는데 폴리필을 사용하면 이 기능을 해당 브라우저에서도 사용할 수 있도록 만들어 준다.

#### :pushpin: 1-1) 폴리필 예시

다음은 브라우저가 `includes` 메서드를 지원하지 않을 경우 사용할 수 있는 폴리필이다.

```javascript
if (!Array.prototype.includes) {
  Array.prototype.includes = function (element) {
    return this.indexOf(element) !== -1;
  };
}
```

#### :pushpin: 1-2) 사용법

**Core-JS 설치**

```javascript
npm install core-js
```

**폴리필 추가**

```javascript
import "core-js/stable";
import "regenerator-runtime/runtime";
```

**_폴리필의 특징_**

- 브라우저의 호환성을 높이는 역할을 한다.
- 주로 구형 브라우저에서 최신 기능을 사용할 때 유용하다.
- [Core-JS](https://github.com/zloirock/core-js) 라이브러리가 대표적인 폴리필 모음집이다.

### :two: 바벨(Babel)이란 무엇인가?

`바벨`은 최신 자바스크립트 코드를 구형 브라우저에서도 실행할 수 있도록 **변환(트랜스파일)하는 도구**다. 최신 문법을 이해하지 못하는 브라우저를 위해 코드를 변환해 준다.

#### :pushpin: 2-1) 바벨의 필요성

- 최신 자바스크립트 문법은 오래된 브라우저에서 지원되지 않을 수 있다.
- 예를 들어, ES6의 let, const, arrow function 등의 문법은 IE11 같은 브라우저에서 실행되지 않음.
- 바벨은 이런 문법을 구형 브라우저에서도 동작하도록 ES5로 변환한다.

```javascript
// ES6 문법
const add = (a, b) => a + b;

// 바벨이 변환한 코드
var add = function (a, b) {
  return a + b;
};
```

### :three: 트랜스파일러(Transpiler)란 무엇인가?

- 트랜스파일러는 코드의 한 프로그래밍 언어를 같은 수준의 다른 언어로 변환하는 도구다.
- 바벨도 트랜스파일러의 일종으로, 최신 자바스크립트를 과거 버전의 자바스크립트로 변환한다.

#### :pushpin: 3-1) 트랜스파일러와 컴파일러의 차이

- `컴파일러`는 고급 언어(예: 자바)를 저급 언어(예: 머신 코드)로 변환한다.
- `트랜스파일러`는 동일한 수준의 언어(예: 최신 JS → 구 JS) 간의 변환을 수행한다.

### :four: 폴리필과 바벨의 차이점

| 특징       | 폴리필                                      | 바벨                                    |
| ---------- | ------------------------------------------- | --------------------------------------- |
| 역할       | 특정 기능(API, 메서드)의 구현을 보충        | 최신 문법을 구 버전 자바스크립트로 변환 |
| 적용 대상  | 브라우저에서 동작하지 않는 기능             | 브라우저가 이해하지 못하는 새로운 문법  |
| 사용 사례  | Array.prototype.includes, Promise, fetch 등 | const, let, arrow functions, classes 등 |
| 라이브러리 | Core-JS 등                                  | Babel                                   |

### :fire: 회고

- 폴리필은 브라우저에서 부족한 기능을 보충하는 코드이다.
- 바벨은 최신 자바스크립트를 구형 브라우저에서도 실행 가능하도록 변환한다.
- 트랜스파일러는 바벨과 같은 도구를 포함하며, 언어 간의 호환성을 높이는 데 사용된다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 폴리필](https://ko.javascript.info/polyfills)
