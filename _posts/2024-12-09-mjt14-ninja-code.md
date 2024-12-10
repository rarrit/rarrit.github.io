---
title: "[mjt] 닌자 코드와 클린 코드에 대해 알아보자"
date: 2024-12-09
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - ninja code
  - clean code
author_profile: true
sidebar_main: true
---

## :ledger: 닌자 코드와 클린 코드에 대해 알아보자

읽기 어렵고 유지보수성이 낮은 코드를 `닌자 코드`라고 비유한다. 이러한 코드 작성 방식에 대해 간단히 알아보고 반대의 클린 코드는 무엇인지 알아보자.

### :one: 닌자 코드 예시

닌자 코드는 작성자 본인만 이해할 수 있는 코드로, 일반적으로 다음과 같은 특징을 가진다

- 의미 없는 변수명
- 길고 복잡한 함수
- 주석 부족 또는 부적절한 주석
- 비직관적인 로직

```javascript
// 함수명 x와 변수명 a, b는 의미가 모호함.
function x(a) {
  let b = [];
  for (let i = 0; i < a.length; i++) {
    if (a[i] % 2 === 0) {
      b.push(a[i]);
    }
  }
  return b;
}

console.log(x([1, 2, 3, 4, 5, 6]));
```

위 코드는 암호화를 위한 로직으로 보일 수 있지만, 변수명이나 로직이 명확하지 않아 코드를 이해하기가 어렵다.

### :two: 클린 코드 예시

클린 코드는 읽기 쉽고 유지보수하기 용이하며, 협업에 적합한 코드를 의미한다. 클린 코드를 작성하기 위한 기본 원칙은 다음과 같다.

- 의미 있는 변수명 사용
- 짧고 명확한 함수 작성
- 적절한 주석 작성
- 일관된 코드 스타일
- 직관적인 로직 구현

```javascript
// 함수명 filterEvenNumbers와 변수명 numbers, evenNumbers는 함수의 의도와 역할을 명확히 전달한다.
function filterEvenNumbers(numbers) {
  const evenNumbers = [];

  for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % 2 === 0) {
      evenNumbers.push(numbers[i]);
    }
  }

  return evenNumbers;
}

console.log(filterEvenNumbers([1, 2, 3, 4, 5, 6])); // [2, 4, 6]
```

### :three: 닌자 코드가 발생하는 이유는 뭘까?

1. `시간 압박`: 빠른 개발을 위해 코드 가독성을 희생할 때 발생할 것 같다.
2. `코딩 습관`: 설계와 문서화보다는 빠른 결과를 중시하는 경우 (경험도 있었던 것 같은..)
3. `경험 부족`: 가독성이나 유지보수성의 중요성을 인식하지 못한 상태에서 작성. (역시 나..!)

#### :four: 클린 코드를 잘 작성하려면?

- `[SOLID 원칙](https://f-lab.kr/insight/understanding-and-applying-solid-principles?gad_source=1&gclid=Cj0KCQiAx9q6BhCDARIsACwUxu6Q_BaKuJvnW2fW8dZUWeIJ9N4ioW0dyEaciSFqZuwPY_EkUYjXQYUaArJkEALw_wcB) 준수`: 객체지향 설계의 5가지 원칙을 따르기.
- `리팩토링`: 주기적으로 코드 개선 작업 수행.
- `테스트 작성`: 기능을 검증하고 코드 품질을 유지.
- `리뷰 요청`: 다른 개발자와 코드 리뷰를 통해 개선.

### :fire: 회고

닌자 코드와 클린 코드는 단순히 코드를 작성하는 방식만이 아니라, 개발자의 사고방식과 작업 스타일을 반영한다. 읽기 쉽고 유지보수하기 쉬운 코드를 작성하면 팀 전체의 생산성이 향상될 뿐만 아니라 프로젝트 또한 성공적으로 마무리할 확률이 높을 것 같다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 닌자 코드](https://ko.javascript.info/ninja-code)
