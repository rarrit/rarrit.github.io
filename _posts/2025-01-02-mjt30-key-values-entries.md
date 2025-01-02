---
title: "[mjt] Object.key,values,entries에 대해 알아보자"
date: 2025-01-02
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - object
  - obejct key
  - object values
  - object entries
author_profile: true
sidebar_main: true
---

## :ledger: Object key,values,entries에 대해 알아보자

JavaScript에서 객체(Object)는 데이터 저장과 관리에 매우 유용한 자료구조이다. 객체를 다룰 때 자주 사용하는 메서드로 **`Object.keys`**, **`Object.values`**, **`Object.entries`**가 있으며, 각 메서드의 특징과 활용 방법에 대해 알아보자

### :one: Object.keys

`Object.keys` 메서드는 객체의 **키(key)**를 배열 형태로 반환한다.

- 반환되는 배열의 각 요소는 객체의 키다.
- 키 순서는 객체에 정의된 순서를 따른다.

#### :pushpin: 1-1) 사용법

```javascript
const user = {
  name: "Alice",
  age: 25,
  city: "Seoul",
};

console.log(Object.keys(user));
// ["name", "age", "city"]
```

#### :pushpin: 1-2) 사용예시

객체의 키로 반복문 실행하기

```javascript
const user = {
  name: "Alice",
  age: 25,
  city: "Seoul",
};

Object.keys(user).forEach((key) => {
  console.log(`${key}: ${user[key]}`);
});

// name: Alice
// age: 25
// city: Seoul
```

### :two: Object.values

`Object.values` 메서드는 객체의 **값(value)**을 배열 형태로 반환한다.

- 반환되는 배열의 각 요소는 객체의 값이다.
- 값 순서 역시 객체에 정의된 순서를 따른다.

#### :pushpin: 2-1) 사용법

```javascript
const user = {
  name: "Alice",
  age: 25,
  city: "Seoul",
};

console.log(Object.values(user));
// ["Alice", 25, "Seoul"]
```

#### :pushpin: 2-2) 사용예시

값 필터링하기

```javascript
const scores = {
  math: 80,
  english: 90,
  science: 85,
};

const highScores = Object.values(scores).filter((score) => score > 85);
console.log(highScores);
// [90]
```

### :three: Object.entries

`Object.entries` 메서드는 객체의 키-값 쌍을 배열 형태로 반환한다. 이 배열의 각 요소는 `[key, value]` 형태의 배열이다.

- 반환된 배열의 각 요소는 [키, 값] 형태이다.
- 이중 배열 형태이므로, 순회하면서 키와 값을 동시에 다룰 수 있다.

#### :pushpin: 3-1) 사용법

```javascript
const user = {
  name: "Alice",
  age: 25,
  city: "Seoul",
};

console.log(Object.entries(user));
// [["name", "Alice"], ["age", 25], ["city", "Seoul"]]
```

#### :pushpin: 3-2) 사용예시

키-값 변환하기

```javascript
const user = {
  name: "Alice",
  age: 25,
  city: "Seoul",
};

const keyValueArray = Object.entries(user).map(
  ([key, value]) => `${key}: ${value}`
);
console.log(keyValueArray);
// ["name: Alice", "age: 25", "city: Seoul"]
```

### :four: 메서드 비교

| 메서드           | 반환값        | 특징                |
| ---------------- | ------------- | ------------------- |
| `Object.keys`    | 키의 배열     | 객체의 모든 키 반환 |
| `Object.values`  | 값의 배열     | 객체의 모든 값 반환 |
| `Object.entries` | [키, 값] 배열 | 키와 값을 함께 반환 |

### :fire: 회고

`Object.keys`, `Object.values`, `Object.entries`는 객체를 다룰 때 매우 유용한 메서드다. 각각의 메서드는 상황에 따라 효율적으로 데이터를 처리할 수 있도록 도와준다. 실무에서 데이터를 가공하거나 객체를 순회해야 할 때 적극적으로 사용하면 좋을 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - Object.keys, values, entries](https://ko.javascript.info/keys-values-entries)
