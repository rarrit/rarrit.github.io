---
title: "[mjt] 가비지 컬렉션에 대해 알아보자"
date: 2024-12-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - garbage-collection
author_profile: true
sidebar_main: true
---

## :ledger: 가비지 컬렉션에 대해 알아보자

자바스크립트는 메모리 관리를 자동으로 수행하는 언어다. 이 덕분에 개발자는 메모리를 수동으로 할당하거나 해제하지 않아도 된다. 이러한 자동화된 메모리 관리를 가능하게 하는 것이 바로 **가비지 컬렉션(Garbage Collection)**이다.

### :one: 가비지 컬렉션이란?

가비지 컬렉션은 프로그램 실행 중 더 이상 사용되지 않는 메모리를 자동으로 해제하여 메모리 누수를 방지하는 기술이다.

#### :pushpin: 1-1) 사용되지 않는 메모리란?

- 더 이상 접근할 수 없는 변수나 객체를 말한다.
- 예를 들어, 어떤 객체를 참조하는 변수가 사라진다면, 그 객체는 사용할 수 없게 되고, 가비지 컬렉션의 대상이 된다.

```javascript
// user엔 객체 참조 값이 저장됨.
let user = {
  name: "John",
};

let admin = user;
user = null; // user의 값을 null 로 변경

// admin을 통해 John을 접근할 수 있기에 John은 메모리에서 삭제 되지 않음
// 이 상태에서 admin 또한 null로 변경하면 John은 가비지 컬렉션의 대상이 됨
```

#### :pushpin: 1-2) 자바스크립트에서 메모리

1. **_할당_**: 변수나 객체를 생성할 때 메모리가 할당된다.
2. **_해제_**: 더 이상 필요하지 않은 메모리를 회수한다.

### :two: 가비지 컬렉션의 동작 원리

자바스크립트는 `Mark-and-Sweep Algorithm(표시-제거 알고리즘)`을 사용해 메모리를 관리한다.

#### :pushpin: 2-1) Mark(표시) 단계

- 실행 중인 변수 및 객체가 **도달 가능**한 상태인지 확인한다.
- 글로벌 객체(`window`), 스코프 내 변수, 그리고 이들로부터 참조되는 객체는 도달 가능하다고 판단된다.

#### :pushpin: 2-2) Sweep(제거) 단계

- 도달 불가능한 객체를 찾아 메모리에서 제거한다.

#### :pushpin: 2-3) 예시: 도달 가능성과 가비지 컬렉션

```javascript
function createObject() {
  let obj = { name: "John" }; // 객체가 생성되고 obj가 이를 참조
  return obj;
}

let user = createObject(); // user는 obj를 참조
user = null; // obj를 참조하는 변수가 없어짐 -> obj는 가비지 컬렉션 대상
```

### :three: 메모리 관리 주의점

#### :pushpin: 3-1) 참조 카운트 문제

- 순환 참조가 발생할 경우 객체가 서로 참조를 하고 있어도 외부에서는 접근할 수 없음.
- 이 경우에도 가비지 컬렉션은 동작하므로 대부분의 상황에서 문제가 되지 않는다.

```javascript
function circularReference() {
  let obj1 = {};
  let obj2 = {};
  obj1.ref = obj2;
  obj2.ref = obj1;
}

circularReference();
// obj1과 obj2는 서로를 참조하지만, 외부에서 접근할 수 없으므로 가비지 컬렉션 대상
```

#### :pushpin: 3-2) 전역 변수 사용 주의

- 전역 변수는 프로그램이 종료될 때까지 메모리가 해제되지 않으므로 필요 이상으로 많은 전역 변수를 사용하지 않는 것이 좋다.

#### :pushpin: 3-3) 이벤트 리스너와 메모리 누수

- DOM 노드가 이벤트 리스너에 의해 참조되고 있으면 가비지 컬렉션되지 않을 수 있음
- 위와 같은 이유로 이벤트 리스너를 적절히 제거하는 습관을 들이는 것이 중요함

#### :four: 메모리 관리 최적화를 위한 팁

- 변수를 필요 이상으로 남겨두지 말자.
  - 사용이 끝난 변수는 `null`로 설정하거나 스코프에서 벗어나도록 관리한다.
- 전역 변수를 최소화하자.
  - 클로저나 블록 스코프를 활용해 필요하지 않은 변수가 전역으로 노출되지 않도록 한다.
- 이벤트 리스너를 제거하자.
  - addEventListener로 등록한 이벤트는 removeEventListener를 통해 해제한다.
- 메모리 사용량을 모니터링하자.
  - 브라우저의 개발자 도구(Chrome DevTools)에서 메모리 사용량을 확인하고 누수가 발생하는지 점검한다.

### :fire: 회고

가비지 컬렉션은 자바스크립트의 강력한 기능 중 하나로, 개발자가 메모리 관리에 신경 쓰지 않아도 안정적인 애플리케이션을 개발할 수 있게 해주는 것 같다. 위의 메모리 관리 최적화를 위한 팁처럼 개발자도 메모리 누수가 발생하지 않도록 최적화하는 작업도 중요하다고 생각이 든다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 가비지 컬렉션](https://ko.javascript.info/garbage-collection)
