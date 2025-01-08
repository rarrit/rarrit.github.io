---
title: "[mjt] 전역 객체에 대해 알아보자"
date: 2025-01-08
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - global object
author_profile: true
sidebar_main: true
---

## :ledger: 전역 객체에 대해 알아보자

JavaScript에서 **전역 객체(Global Object)**는 모든 환경에서 사용할 수 있는 특수 객체입니다. 브라우저, Node.js, 또는 기타 JavaScript 실행 환경에 따라 전역 객체의 형태와 기능은 다소 달라지지만, 핵심 개념은 동일하다. 전역 객체의 정의와 특징, 주요 프로퍼티, 실행환경에서의 차이를 알아보자.

### :one: 전역 객체란?

#### :pushpin: 1-1) 어디서든 접근 가능

별도의 import 없이 바로 사용할 수 있습니다.

#### :pushpin: 1-2) 환경에 따라 이름이 다름

- `브라우저`: window, self, frames, globalThis
- `Node.js`: global, globalThis

#### :pushpin: 1-3) 전역 변수와의 관계

전역 변수로 선언된 값은 전역 객체의 프로퍼티로 등록됩니다.

### :two: 전역 객체의 실행 환경별 이름

#### :pushpin: 2-1) 브라우저 환경

브라우저에서 전역 객체는 주로 `window`로 접근한다.

```javascript
console.log(window); // 브라우저의 전역 객체
console.log(window.document); // DOM 접근
```

#### :pushpin: 2-2) Node.js 환경

Node.js에서는 전역 객체를 `global`로 접근한다.

```javascript
console.log(global); // Node.js의 전역 객체
```

#### :pushpin: 2-3) 공통으로 사용 가능한 globalThis

ECMAScript 2020부터 추가된 `globalThis`는 실행 환경에 상관없이 전역 객체에 접근할 수 있다

```javascript
console.log(globalThis); // 환경에 따라 적합한 전역 객체 반환
```

### :three: 전역 객체의 주요 프로퍼티와 메서드

#### :pushpin: 3-1) 브라우저 환경의 주요 프로퍼티

- `window.document`: DOM(Document Object Model)에 접근
- `window.localStorage`: 데이터를 브라우저에 저장
- `window.location`: 현재 URL 정보를 제공
- `window.setTimeout`: 일정 시간 후에 콜백 함수를 실행

```javascript
console.log(window.location.href); // 현재 페이지의 URL 출력
```

#### :pushpin: 3-2) Node.js 환경의 주요 프로퍼티

- `global.console`: 콘솔 출력 메서드
- `global.process`: 현재 프로세스 정보
- `global.setTimeout`: 일정 시간 후에 콜백 함수를 실행
- `global.Buffer`: 버퍼 데이터 처리

### :four: 전역 객체 활용 사례

#### :pushpin: 4-1) 브라우저에서 DOM 조작

```javascript
document.body.style.backgroundColor = "lightblue";
```

#### :pushpin: 4-2) 전역 타이머 메서드

```javascript
setTimeout(() => {
  console.log("1초 뒤에 실행됩니다.");
}, 1000);
```

### :fire: 회고

- **전역 객체(Global Object)**는 JavaScript의 모든 실행 환경에서 기본적으로 제공된다.
- 환경에 따라 이름과 프로퍼티가 달라지지만, 핵심 개념은 동일하다.
- 보안성과 유지보수성을 고려해 전역 객체의 사용을 적절히 제한하고, 모듈 시스템을 활용하는 것이 좋다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 전역 객체](https://ko.javascript.info/global-object)
