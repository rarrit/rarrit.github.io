---
title: "[mjt] 화살표 함수 다시 살펴보기"
date: 2025-01-20
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - arrow functions
author_profile: true
sidebar_main: true
---

## :ledger: 화살표 함수 다시 살펴보기

### :one: 함수를 짧게 쓰기 위한 용도로 사용되지 않는다.

화살표 함수는 몇 가지 독특하고 유용한 기능을 제공한다.

- `arrow.forEach(func) - func`는 `forEach`가 호출될 때 배열 `arr`의 요소 전체를 대상으로 실행된다.
- `setTimeout(func) - func`는 내장 스케줄러에 의해 실행된다.

자바스크립트에선 함수를 생성하고 그 함수를 어딘가에 전달하는 것이 자연스럽다. 하지만 어딘가에 함수를 전달할 때 **함수의 컨텍스트를 잃을 수 있다.** 이럴 때 `화살표 함수`를 사용하면 현재 컨텍스트를 잃지 않아 편리하다.

### :two: 화살표 함수에는 this가 없다.

화살표 함수 본문에서 `this`에 접근하면, 외부에서 값을 가져온다. 이런 특징은 객체의 메서드안에서 동일 객체의 프로퍼티를 대상으로 순회하는데 사용할 수 있다.

```javascript
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    // forEach에서 화살표 함수를 사용했기 때문에 본문에 있는 this.title은
    // 화살표 함수 바깥에 있는 메서드인 showList가 가리키는 대상과 동일함
    // 즉 this.title은 group.title과 같다.
    // 일반함수로 사용한다면 typeError가 노출됨
    this.students.forEach((student) => alert(this.title + ": " + student));
  },
};

group.showList();
```

### :three: new와 함께 실행할 수 없다.

`this`가 없기 때문에 화살표 함수는 생성자 함수로 사용할 수 없다는 제약이 있다. 화살표 함수는 `new`와 함께 호출할 수 없다.

### :four: vs bind

화살표 함수와 일반 함수를 `.bind(this)`를 사용해서 호출하는 것 사이에는 미묘한 차이가 있다.

- `.bind(this)`는 함수의 '한정된 버전'을 만든다.
- 화살표 함수는 어떤 것도 바인딩 시키지 않는다. 화살표 함수엔 단지 `this`가 없을 뿐, 화살표 함수에서 `this`를 사용하면 일반 변수 서칭과 마찬가지로 `this`의 값을 외부 렉시컬 환경에서 찾는다.

### :five: 화살표 함수엔 arguments가 없다.

- 화살표 함수는 일반 함수와는 다르게 모든 인수에 접근할 수 있게 해주는 유사 배열 객체 `arguments`를 지원하지 않는다.
- 이러한 특징은 현재 `this` 값과 `arguments` 정보를 함께 실어 호출을 포워딩해 주는 데코레이터를 만들 때 유용하게 사용된다.

```javascript
// 화살표 함수 예시
// 데코레이터 defer(f, ms)는 함수를 인자로 받고 이 함수를 래퍼로 감싸 변환하며 함수 f는 ms밀리초 후에 호출됨
function defer(f, ms) {
  return function () {
    setTimeout(() => f.apply(this, arguments), ms);
  };
}

function sayHi(who) {
  alert("안녕, " + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수"가 출력됩니다.

// 일반 함수 예시
// 일반 함수에선 setTimeout에 넘겨주는 콜백 함수에서 사용할 변수 ctx와 args를 반드시 만들어줘야함
function defer(f, ms) {
  return function (...args) {
    let ctx = this;
    setTimeout(function () {
      return f.apply(ctx, args);
    }, ms);
  };
}
```

### :fire: 요약

- `컨텍스트 보존`
  - 화살표 함수는 this를 가지지 않으며, 외부 렉시컬 환경에서 this를 찾기 때문에 컨텍스트를 잃지 않는다. 이는 객체 메서드나 콜백에서 유용하다.
- `생성자 함수로 사용 불가`
  - 화살표 함수는 new 키워드와 함께 사용할 수 없다.
- `arguments 없음`
  - 화살표 함수에는 arguments 객체가 없으며, 필요한 경우 나머지 매개변수(...args)를 사용해야 한다.
- `bind와의 차이`
  - .bind(this)는 함수의 새로운 한정된 버전을 생성하지만, 화살표 함수는 this 자체가 없고 외부 컨텍스트에서 값을 찾는다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 화살표 함수 다시 살펴보기](https://ko.javascript.info/arrow-functions)
