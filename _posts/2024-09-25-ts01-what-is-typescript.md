---
title: "[TS] 타입스크립트란 무엇인지 알아보자!"
date: 2024-09-25
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - ts  
tags:
  - typescript
author_profile: true
sidebar_main: true
---

## :ledger: 타입스크립트라는 언어가 무엇인지 알아보자!

![img-typescript-start](https://github.com/user-attachments/assets/358bd633-bbb5-4c19-ad78-50ee76e6daa1)

이번주차에 타입스크립트를 배웠고 오늘 점심에 라면을 먹었다. 그렇다. 타입스크립트는 `맛있는 라면`이다. 라면을 끓일 때 물의 양을 정확하게 맞추지 못해 망한 적이 있다? 자바스크립트는 그런 느낌이다. 하지만 타입스크립트를 사용하면, 정확한 물의 양을 미리 확인하고 맞추는 것처럼, 코드에서 실수할 가능성을 줄일 수 있다! 맛있는 라면을 끓이듯, 타입스크립트를 이해하면 더 `안정적`이고 `예측 가능`한 코드를 작성할 수 있다! 하하하하 

### :one: TypeScript란?

![img-typescript-mim](https://github.com/user-attachments/assets/54d3126f-66b5-42c0-a641-06c3d12412bd)

- 타입스크립트는 <u>자바스크립트의 모든 기능을 포함하는 상위 집합의 언어</u>다.
- 타입스크립트 = 자바스크립트 + <b>정적 타입 시스템</b>

#### :pushpin: 1-1) 타입 시스템이란?
프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙 집함이며, 아래와 같은 타입이 있다.

1. null
2. undefined
3. boolean
4. string
5. number

#### :pushpin: 1-2) 동적 타입 언어
- 변수의 타입이 런타임에 결정되며 코드를 실행하는 동안에 변수가 어떤 타입인지를 해석하고, 그에 맞게 처리함
- 같은 변수에 여러 타입의 값을 할당할 수 있다. 예를들면 처음 문자열 할당 이후 숫자 할당 가능
- 지금까지 써왔던 JavaScript가 예시임

```javascript
let x = 1; // 숫자
x = "string"; // 문자열
```

#### :pushpin: 1-3) 정적 타입 언어
- 변수의 타입이 컴파일 타임에 결정되며 코드를 작성할 때 변수가 어떤 타입인지를 명시하고 그 타입을 준수해야함
- 변수의 타입은 고정되며, 다른 타입의 값을 할당하려하면 컴파일 오류가 발생함

```typescript
let x: number = 1; // x 넌 이제부터 숫자다 
x = "string" // 오류남 
```

#### :pushpin: 1-4) 타입스크립트 장점
타입스크립트를 쓰면 코드 작성 중에 오류를 미리 잡을 수 있어, 디버깅 시간이 줄어들고 코드의 안정성이 올라감
- [TypeScript Deep Div - 심층 분석](https://basarat.gitbook.io/typescript/getting-started/why-typescript)

### :two: TypeScript 동작 원리
타입스크립트는 `컴파일(영->국 번역처럼 생각하면 됨)` 이라는 과정안에서 문법, 타입 체크 등을 수행해 줍니다.

1. 타입스크립트 컴파일러가 코드를 분석함
  - <u>이런 내용을 보고 우리는 컴파일 타임에 분석한다</u>라고도 표현함
2. 분석 이후 오류가 없다면 JavaScript 코드로 ***전부*** 변경함
  - 즉, 런타임(<->컴파일 타임)에는 전부 JavaScript와 동일하게 동작함

위와 같이 동작하며 **타입스크립트는 자바스크립트의 모든 기능을 포함하는 상위 집합의 언어**라고 할 수 있다.

### :fire: 마무리
이렇게 타입스크립트를 설치하고 사용하는 방법을 알았다면, 이제 실수를 줄이며 더 안전한 코드를 작성할 준비가 된 것이다. 맛있는 라면처럼, 타입스크립트로 더 완성도 높은 프로젝트를 만들어 보자..!