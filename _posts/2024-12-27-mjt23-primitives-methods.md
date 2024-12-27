---
title: "[mjt] 원시값의 메서드에 대해 알아보자"
date: 2024-12-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - primitives-methods
author_profile: true
sidebar_main: true
---

## :ledger: 원시값의 메서드에 대해 알아보자

JavaScript에서 원시값(Primitive Value)은 객체가 아니지만, 메서드를 호출할 수 있다는 점에서 특이한 성질을 가지고 있다. 원시값의 메서드가 어떻게 동작하는지, 그리고 이를 가능하게 하는 원리를 알아보자.

### :one: 원시값이란?

원시값(Primitive Value)은 JavaScript의 기본 자료형으로, <u>객체가 아닌 값</u>을 의미한다. 대표적인 원시값에는 아래와 같은 자료형이 있다.

- **String**: 문자열
- **Number**: 숫자
- **BigInt**: 큰 정수
- **Boolean**: 참/거짓
- **Symbol**: 고유한 값
- **undefined**: 정의되지 않음
- **null**: 존재하지 않음

### :two: 원시값의 메서드는 어떻게 가능한가?

원시값은 객체가 아니지만, 메서드를 호출할 때 JavaScript는 원시값을 `래퍼 객체(Wrapper Object)`로 일시 변환하여 메서드를 제공한다.

#### :pushpin: 2-1) 예시 #1 : 문자열과 메서드

아래의 코드에서 `str`은 원시값이지만, `toUpperCase()` 메서드를 호출할 수 있다. 이는 JavaScript 엔진이 내부에적으로 다음과 같이 처리하기 때문이다.

1. 문자열 원시값 (`str`)을 String 객체로 변환
2. `String.prototype`의 메서드(`toUpperCase`)를 호출
3. 결과를 반환 후, 래퍼 객체를 삭제

```javascript
const str = "Hello, World!";
console.log(str.toUpperCase()); // "HELLO, WORLD!"
```

#### :pushpin: 2-2) 예시 #2 : 숫자 메서드

아래의 코드에서 `num`은 원시값이지만, JavaScript는 이를 **Number 객체**로 변환하여 `toFixed()`메서드를 사용할 수 있도록 한다.

```javascript
const num = 123.456;
console.log(num.toFixed(2)); // "123.46"
```

#### :pushpin: 2-3) 원시값의 메서드 동작 원리 정리

1. **래퍼 객체 생성**: 메서드 호출 시, 원시값은 해당 타입의 래퍼 객체로 변환 (예시: `String` -> `new String("value")`)
2. **메서드 호출**: 래퍼 객체의 프로토타입에 정의된 메서드가 실행된다.
3. **래퍼 객체 삭제**: 메서드 호출이 끝난 후, 래퍼 객체는 삭제되어 메모리에서 사라진다.

### :three: 주의사항

원시값이 객체로 변환되는 과정은 일시적이며, 직접 원시값에 속성을 추가하려고 하면 에러가 발생한다.

1. `str`의 프로퍼티에 접근하려 하면 `래퍼 객체`가 만들어진다.
2. 엄격모드에선 래퍼 객체를 수정하려 할 때 에러가 발생
3. 비 엄격 모드에선 에러가 발생하지 않고 `래퍼 객체`에 프로퍼티 `customProperty`가 추가된다. 하지만 <u>래퍼 객체는 바로 삭제되기 때문에</u> 마지막 줄이 실행될 땐 프로퍼티 `customProperty`를 찾을 수 없음

```javascript
const str = "Hello";
str.customProperty = "World";
console.log(str.customProperty); // 비엄격 모드 undefined, 엄격모드 An error
```

### :fire: 회고

JavaScript에서 원시값이 메서드를 사용할 수 있는 것은 `래퍼 객체`덕분이다. 해당 원리를 이해하고 원시값과 객체의 차이, 메서드 호추 방식에 대해 이해할 수 있어야 한다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 원시값의 메서드](https://ko.javascript.info/primitives-methods)
