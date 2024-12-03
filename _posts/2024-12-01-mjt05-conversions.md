---
title: "[mjt] 자바스크립트의 형 변환에 대해 알아보자"
date: 2024-12-01
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - conversions
author_profile: true
sidebar_main: true
---

## :ledger: 자바스크립트의 형 변환에 대해 알아보자

자바스크립트에서 데이터는 다양한 타입이 있다. 하지만 종종 자동으로 변환(암시적 형 변환)하거나 명시적으로 변환이 필요할 때가 있다. `형 변환(Type Conversion)`은 자바스크립트의 핵심 개념 중 하나로 무엇이 있는지 알아보자.

### :one: 암시적 형 변환

자바스크립트는 다양한 데이터 타입 간의 호환성을 보장하기 위해 자동으로 타입을 변환한다. 하지만 이 과정이 예상치 못한 결과를 초래할 수 있다.

#### :pushpin: 1-1) 문자열과 숫자의 연산

1. string + number = string
2. string - number = number
3. string \* number = number
4. string / number = number
5. string % number = number

```javascript
"5" + 2; // "52"
"5" - 2; // 3
"5" * "2"; // 10
"10" / 2; // 5
"10" % 3; // 1
```

#### :pushpin: 1-2) 불리언과 숫자

`true`는 1 `false`는 0으로 계산된다.

```javascript
true + 1; // 2
false - 1; // -1
```

#### :pushpin: 1-3) Falsy와 Truthy 값

1. if(0) -> `false`
2. if("") -> `false`
3. if("hello") -> `true`

### :two: 명시적 형 변환

명시적 형 변환은 의도적으로 값을 특정 데이터 타입으로 변환한다.

#### :pushpin: 2-1) 숫자로 변환

자주 사용했던 건 `Number`와 `parseInt` 였던 것 같다.

```javascript
Number("123"); // 123
+"123"; // 123
parseInt("123px"); // 123
```

#### :pushpin: 2-2) 문자열로 변환

`Number`와 마찬가지로 `String`을 자주 사용했는데, `toString`이 더 많이 사용하는 것 같은..

```javascript
String(123); // "123"
(123).toSTring(); // "123"
```

#### :pushpin: 2-3) 불리언으로 변환

사용하지도 않았고 본 적도 없는데 불리언을 사용할 때 주로 if문을 사용해서가 아닐까.. 싶은

```javascript
Boolean(1); // true
Boolean(0); // false
Boolean("hello"); // true
Boolean(""); // false
```

### :three: 형 변환 주요 메서드

자주 사용했던 기억이 없는데, 찾아보다가 있어서 정리해봤다.

#### :pushpin: 3-1) `JSON.stringify()`와 `JSON.parse()`

객체를 문자열로, 문자열을 다시 객체로 변환하는 방법이다.

```javascript
const obj = { key: "value" };
const str = JSON.stringify(obj); // '{"key" : "value"}'
const parsed = JSON.parse(str); // {key: "value"}
```

#### :pushpin: 3-2) `isNaN()`과 `Number.isNaN()`

- `isNaN()`은 형 변환 후 체크함
- `Number.isNaN()`은 정확히 NaN인 경우만 체크함

```javascript
isNaN("123abc"); // true
Number.isNaN("123abc"); // false
```

#### :pushpin: 3-3) `typeof`와 `instanceof`의 차이

- `typeof`는 데이터 타입 확인
- `instanceof`는 특정 객체인지 확인

```javascript
typeof []; // "object"
[] instanceof Array; // true
```

#### :pushpin: 3-4) `Object.prototype.toString()`

- 객체의 정확한 타입을 확인하는 방법이다.

```javascript
Object.prototype.toString.call([]); // [object Array]
Object.prototype.toString.call({}); // [object Object]
```

### :four: 자주 겪는 실수

형 변환에서 헷갈리는 부분을 정리해봤다.

#### :pushpin: 4-1) `null`과 `undefined`의 특성

```javascript
null == undefined; // true
null === undefined; // false
```

#### :pushpin: 4-2) 배열과 객체

실제로 사용해보거나 본 적은 없는데 엥? 해서 가져옴

```javascript
[1, 2] == "1,2"; // true 엥?
[] == ""; // true
{
}
+{}; // NaN(런타임 상황에 따라 다르다함) 이건 무ㅓ..?
```

#### :pushpin: 4-3) `NaN`의 특성

`NaN`은 웃긴게 서로도 같지 않음

```javascript
NaN === NaN; // false
```

### :fire: 회고

자바스크립트는 매우 유연한 언어다. 매우 유연한.. 너무 유연한.. 언어..

#### :pushpin: 참고 문서

- [ko.javascript.info - 자료형](https://ko.javascript.info/type-conversions)
- [MDN Web Docs - Type Conversions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
