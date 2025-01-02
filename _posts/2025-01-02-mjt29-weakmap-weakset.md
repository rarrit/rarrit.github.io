---
title: "[mjt] 위크맵과 위크셋에 대해 알아보자 (weakmap, weakset)"
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
  - weakmap
  - weakset
author_profile: true
sidebar_main: true
---

## :ledger: WeakMap과 WeakSet에 대해 알아보자

JavaScript에서 **WeakMap**과 **WeakSet**은 메모리 관리를 위한 특별한 자료구조이다. 무엇인지 알아보자.

### :one: WeakMap이란?

`WeakMap`은 키로 **객체만** 사용할 수 있는 `key-value` 자료구조이다. 주요 특징은 **참조가 약하다는 점**이 있으며, 이는 키에 대한 참조가 없어지면 그 데이터가 자동으로 <u>가비지 컬렉션</u>이 된다.

#### :pushpin: 1-1) 사용법

```javascript
const weakMap = new WeakMap();
const obj = { key: "value" };

// 데이터 추가
weakMap.set(obj, "Hello, WeakMap!");

// 데이터 조회
console.log(weakMap.get(obj)); // "Hello, WeakMap!"

// 객체 참조 제거
obj = null;

// WeakMap에 저장된 데이터는 가비지 컬렉션됨
```

#### :pushpin: 1-2) 주요 메서드

| 메서드            | 설명                   |
| ----------------- | ---------------------- |
| `set(key, value)` | 키-값 쌍 추가          |
| `get(key)`        | 키에 해당하는 값 반환  |
| `has(key)`        | 특정 키 존재 여부 확인 |
| `delete(key)`     | 특정 키 삭제           |

#### :pushpin: 1-3) 제약사항

- `WeakMap`은 반복(iterable) 불가능하므로 `forEach`나 `for...of` 같은 방식으로 순회할 수 없다.
- 키로 객체만 사용할 수 있으며, 원시 값은 사용할 수 없다.

### :two: WeakSet이란?

`WeakSet`은 객체의 집합을 저장하는 자료구조다. `WeakMap`과 마찬가지로, 객체에 대한 참조가 사라지면 가비지 컬렉션이 된다.

#### :pushpin: 2-1) 사용법

```javascript
const weakSet = new WeakSet();

let obj1 = { id: 1 };
let obj2 = { id: 2 };

// 데이터 추가
weakSet.add(obj1);
weakSet.add(obj2);

// 데이터 확인
console.log(weakSet.has(obj1)); // true

// 객체 참조 제거
obj1 = null;

// WeakSet에 저장된 데이터는 가비지 컬렉션됨
```

#### pushpin: 2-2) 주요 메서드

| 메서드          | 설명                   |
| --------------- | ---------------------- |
| `add(value)`    | 값을 추가              |
| `has(value)`    | 특정 값 존재 여부 확인 |
| `delete(value)` | 특정 값 삭제           |

### :three: WeakMap과 WeakSet의 활용 사례

#### :pushpin: 3-1) 비공개 데이터 저장 (WeakMap)

`WeakMap`은 객체와 관련된 비공개 데이터를 저장하는 데 유용하다. 예를 들어, 클래스를 통해 외부에서 접근 불가능한 데이터를 관리할 수 있다.

```javascript
const privateData = new WeakMap();

class User {
  constructor(name) {
    privateData.set(this, { name });
  }

  getName() {
    return privateData.get(this).name;
  }
}

const user = new User("Alice");
console.log(user.getName()); // "Alice"
```

#### :pushpin: 3-2) 객체 추적하기 (WeakSet)

`WeakSEt`은 특정 객체가 이미 처리되었는지 추적하는데 유용하다.

```javascript
const visitedNodes = new WeakSet();

function traverse(node) {
  if (visitedNodes.has(node)) {
    return;
  }

  visitedNodes.add(node);
  // 노드 처리 로직
}
```

### :four: WeakMap과 WeakSet의 차별점

| 특징           | WeakMap                        | WeakSet                        |
| -------------- | ------------------------------ | ------------------------------ |
| 저장 형태      | 키-값 쌍                       | 객체의 집합                    |
| 키/값 제한     | 객체 키만 가능, 값은 제한 없음 | 객체만 저장 가능               |
| 반복 가능 여부 | 불가능                         | 불가능                         |
| 메모리 관리    | 약한 참조로 자동 가비지 컬렉션 | 약한 참조로 자동 가비지 컬렉션 |

### :fire: 회고

`WeakMap`과 `WeakSet`은 일반적인 Map과 Set에 비해 사용 빈도가 낮을 수 있지만, 메모리 관리와 비공개 데이터 관리가 필요한 경우에는 매우 유용한 도구인 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 위크맵과 위크셋](https://ko.javascript.info/weakmap-weakset)
