---
title: "콘솔(`console`)의 정의와 역할 (feat. 강아지 출력하기)"
date: 2024-07-11
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
  - js
tags:
  - console
author_profile: true
sidebar_main: true
---

## :ledger: 콘솔(`console`)은 무엇인가?

### :one: 콘솔(`console`)의 정의와 역할

**콘솔(console)** 은 개발자가 웹 애플리케이션을 디버깅하고 상호작용할 수 있는 강력한 도구다.<br/>
웹 브라우저의 개발자 도구(Developer Tools) 중 하나로, JavaScript를 실행하고 로그를 출력하는데 사용된다.

#### :pushpin: 1-1) 콘솔(`console`)의 정의

콘솔은 텍스트 기반의 인터페이스로, 사용자가 명령어를 입력하고 그에 대한 출력을 즉시 확인할 수 있는 환경을 제공함<br/>
브라우저의 콘솔은 웹 페이지의 상태를 **모니터링하고, 디버깅하며, 테스트할 수 있는 기능**을 갖추고 있다.

#### :pushpin: 1-2) 콘솔(`console`)의 역할

1. 디버깅 (Debugging)

   - 오류 확인: 콘솔은 웹 페이지에서 발생하는 오류를 실시간으로 표시해준다. 이를 통해 오류의 원인을 빠르게 파악하고 수정이 가능하다.
   - 로그 출력: `console.log()`, `console.error()`, `console.warn()`등 메서드를 사용하여 코드 실행 중 발생하는 다양한 메시지를 출력할 수 있다.

2. 실시간 코드 실행

   - JavaScript 코드를 실시간으로 실행할 수 있는 환경을 제공하며 이를 통해 페이지를 새로고침하지 않고 코드를 테스트하고 결과를 확인할 수 있다.

3. 변수와 객체 조사

   - 변수와 객체의 현재 상태를 확인할 수 있으며 `console.dir()`메서드를 통해 객체의 속성과 메서드를 구조적으로 볼 수 있다.

4. 성능 분석

   - `console.time()`과 `console.timeEnd()`메서드를 사용하여 코드의 실행 시간을 측정하고 성능을 분석할 수 있는 기능도 있다.

5. 데이터 시각화

   - `console.table()` 메서드를 사용하여 배열이나 객체 데이터를 테이블 형태로 시각화 할 수 있다.

6. 네트워크 요청 모니터링
   - 콘솔은 웹 페이지에서 발생하는 모든 네트워크 요청을 모니터링하고, 요청과 응답의 세부 정보를 제공한다.

### :two: 콘솔(`console`) 사용 예시

1. 로그 출력

```javascript
console.log("안녕 !"); // 일반 로그 출력
console.error("이건 에러 메시지에요!"); // 에러 메시지 출력
console.warn("이건 경고 메시지에요!"); // 경고 메시지 출력
console.info("이건 정보 메시지에요!"); // 정보 메시지 출력
```

2. 실시간 코드 실행

```javascript
let a = 1,
  b = 2,
  sum = a + b;
console.log("sum :", sum); // 3
```

3. 변수와 객체 조사

```javascript
let rarrit = {
  name: "min kyu",
  age: 30,
};
console.dir(rarrit); // 객체의 속성과 메서드를 구조적으로 출력해줌
```

4. 데이터 시각화

```javascript
let dog = [
  { name: "maru", age: 14 },
  { name: "kkambo", age: 12 },
];
console.table(dog); // 데이터가 테이블 형태로 출력됨
```

5. 성능 측정

```javascript
console.tiem("a");
// 실행 코드
console.timeEnd("a"); // a의 실행 시간을 측정하고 출력
```

### :three: 콘솔(`console`)을 사용하여 강아지 그리기

#### 요구사항

<img width="385" alt="console-dog-ex" src="https://github.com/rarrit/TIL/assets/94345781/6bc5bd45-3c06-4737-9489-c2cbe135e8a9"><br/>

1. `console.log`로 특수 문자를 출력하여 강아지를 그립니다.
2. 그림에 대한 설명도 출력합니다.
3. 그림에 대한 경고 메시지도 추가합니다.
4. 오류 메시지도 출력합니다.
5. 마지막으로, 강아지를 그리는 데 걸린 시간을 출력합니다.

#### 결과물

<img width="710" alt="console-dog-suc" src="https://github.com/rarrit/TIL/assets/94345781/fc2d6d06-49d9-4a71-944a-3435214da098">

```javascript
console.time("그리는데 걸린 시간");
console.log(`
               /\\____
              (    ₩ \\_____
              /            O
              /    (______/
              /______/  U
                `);
console.log("이것은 단순한 SCII 아트 강아지입니다.");
console.warn("주의: 이 강아지는 그림입니다. 실제 강아지가 아닙니다!");
console.error("오류: 실제 강아지 데이터를 불러오지 못했습니다!");
console.timeEnd("그리는데 걸린 시간");
```

### :fire: 마무리

스파르타 코딩클럽 데일리 미션의 `console`을 활용하여 강아지를 출력하기를 진행했다.<br/>
처음에 요구사항을 보고 당황을 했는데, 지금까지 `console.log`는 정말 익숙하게 많이 사용해봤지만, `console.time`, `console.warn`등 다른 메서드를 알 지 못했어서 구글에 검색해서 진행했다.<br/>
그 중 `time()`과 `timeEnd()` 사용법이 처음에 이해가 안되가지고 시간을 조금 잡아먹힌..!<br/>
위와 같이 강아지를 그리면서 콘솔에 대해 알아보았는데 다양한 메서드를 알게 되었고 재밌는 시간이었다.

#### 적용한 코드

**깃헙 링크**: [https://github.com/rarrit/TIL/tree/main/Project/consoleDog](https://github.com/rarrit/TIL/tree/main/Project/consoleDog)
**화면 링크**: [https://rarrit.github.io/TIL/Project/consoleDog](https://rarrit.github.io/TIL/Project/consoleDog)
