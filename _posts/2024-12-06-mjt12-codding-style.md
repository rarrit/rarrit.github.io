---
title: "[mjt] 좋은 코드 스타일은 무엇인지 알아보자"
date: 2024-12-06
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - code style
author_profile: true
sidebar_main: true
---

## :ledger: 좋은 코드 스타일은 무엇인지 알아보자

개발자는 <u>간결하고 읽기 쉽게 코드를 작성</u>하는 것이 중요하다. 복잡한 문제를 간결하고 읽기 쉬운 코드로 작성해 해결하는 것이야말로 좋은 코드 스타일이며 어떤것들이 있는지 알아보자.

### :one: 좋은 코드 스타일의 특징

1. **가독성**: 코드의 목적과 동작이 명확히 드러나야 한다.
2. **일관성**: 프로젝트 내 모든 코드가 통일된 스타일을 따라야 한다.
3. **유지보수성**: 다른 개발자가 쉽게 이해하고 수정할 수 있어야 한다.

#### 예시

```javascript
// 가독성이 좋은 코드
function getUserAge(user) {
  return user.age;
}

// 가독성이 떨어지는 코드
function gua(u) {
  return u.a;
}
```

### :two: 코드 스타일을 관리하는 방법

#### :pushpin: 2-1) 규칙의 중요성

**코드 스타일에 대한 "정답"은 없다.** 프로젝트 또는 팀의 컨벤션에 따라 스타일을 정의하고 이를 따르는 것이 중요하며 스타일을 팀원들과 명확히 합의하면 협업이 더 쉬워진다

#### :pushpin: 2-2) Linter 사용

Lint 도구를 활용하면 스타일 가이드를 자동으로 준수할 수 있다. `Linter`는 코드에서 발생할 수 있는 문제를 사전에 경고하거나 수정 제안을 제공한다.

### :three: 추천 도구

아래는 자바스크립트에서 널리 사용되는 `Linter` 도구들이다.

- [ESLint](https://eslint.org/)
  - 최신 Linter로 확장성과 유연성이 뛰어남.
  - 다양한 플러그인과 규칙 설정을 지원하며, 가장 많이 사용되는 도구이다.
- [Prettier](https://prettier.io/)
  - 코드 포맷팅에 특화된 도구로, 일관된 스타일을 유지함.
  - ESLint와 함께 사용하면 서로 보완하여 완벽한 코드 스타일을 유지할 수 있다.

프로젝트를 하며 둘 다 가장 많이 사용했으며, 협업할 때 들여쓰기, 세미콜론 등 각자 스타일이 다르지만 해당 도구를 사용하면 모두 통일된 스타일을 맞출 수 있어서 정말 유용하다.

### :fire: 회고

좋은 코드 스타일을 유지하려면 `ESLint`와 `Prettier`를 적극적으로 활용하고, 팀 컨벤션을 준수하는 것이 중요핟다. 이 두 가지 도구를 통해 일관성 있고 가독성 높은 코드를 작성해보자

#### :pushpin: 참고 문서

- [ko.javascript.info - 코딩 스타일](https://ko.javascript.info/coding-style)
