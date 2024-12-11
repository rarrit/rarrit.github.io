---
title: "[mjt] 테스트 자동화와 Mocha에 대해 알아보자"
date: 2024-12-10
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - testing-mocha
author_profile: true
sidebar_main: true
---

## :ledger: 테스트 자동화와 Mocha에 대해 알아보자

테스트 자동화는 개발 과정에서 뿐만 아니라 현업에서도 효율성과 품질 향상을 위해 필수적으로 사용된다. 테스트의 중요성과 이를 효율적으로 관리할 수 있는 테스트 자동화 도구인 Mocha를 중심으로 알아보자.

### :one: 테스트는 왜 해야 하는가?

함수를 구현할 때, 우리는 일반적으로 매개변수와 결과 간의 관계를 구상하면서 코드를 작성한다. 작성한 코드가 의도한 대로 동작하는지 확인하기 위해 보통 콘솔 로그나 간단한 출력 테스트를 사용하는데, 이런 수동적인 테스트 방식은 다음과 같은 문제점을 갖고 있다.

#### :pushpin: 1-1) 수동 테스트는 시간이 많이 든다.

- 매번 코드를 수정할 때마다 이전의 모든 유스 케이스를 수동으로 테스트해야 한다.
- 테스트 범위가 늘어날수록 확인해야 할 시나리오가 기하급수적으로 증가한다.

#### :pushpin: 1-2) 테스트 누락 가능성이 높다.

- 특정 케이스(f(1), f(2))는 확인했지만, 코드 수정 후 이전에 동작하던 케이스를 다시 검증하지 않을 수 있다.
- 특히 협업 환경에서는 다른 개발자가 추가한 기능이 기존의 동작을 깨트릴 위험이 크다.

#### :pushpin: 1-3) 유지보수가 어렵다.

- 시간이 지나면 어떤 조건에서 테스트를 진행했는지 기억하기 어렵다.
- 개발자는 새로운 기능에 집중하다 보니 이전 기능을 간과할 가능성이 크다.

### :two: 테스트 자동화의 필요성

테스트 자동화는 이런 문제를 해결하는 데 핵심적인 역할을 한다. 자동화된 테스트 코드를 작성하면 다음과 같은 장점이 있음.

#### :pushpin: 2-1) 반복 가능한 테스트

- 모든 유스 케이스를 빠짐없이 재검증할 수 있다.
- 한 번 작성한 테스트 코드는 언제든 재사용 가능하므로 유지보수 시 큰 도움을 준다.

#### :pushpin: 2-2) 신뢰할 수 있는 코드

- 코드 변경 후에도 기존 기능이 깨지지 않았음을 자동으로 확인할 수 있다.
- 협업 시 다른 개발자가 새로운 기능을 추가하더라도 전체 코드의 안정성을 유지할 수 있음.

#### :pushpin: 2-3) 빠른 피드백

- 코드 작성 후 바로 테스트를 실행해 결과를 확인할 수 있음.
- 버그를 초기에 발견할 수 있어 개발 속도를 높이고, 디버깅 비용을 줄여준다.

### :three: Mocha를 활용한 테스트 자동화

[Mocha](https://mochajs.org/)는 Node.js 환경에서 널리 사용되는 JavaScript 테스트 프레임워크다. 간단한 설정으로 빠르게 테스트를 자동화할 수 있어 인기 있는 도구 중 하나다.

#### :pushpin: 3-1) Mocha의 주요 특징

- `유연한 테스트 작성`
  - BDD(Behavior Driven Development)와 TDD(Test Driven Development) 스타일을 모두 지원한다.
- `다양한 Assertion Library 지원`
  - 기본 제공하지 않지만, [Chai](https://www.chaijs.com/)와 같은 라이브러리를 함께 사용할 수 있다.
- `비동기 테스트 지원`
  - async/await이나 콜백을 사용하는 비동기 코드 테스트에 적합하다.

#### :pushpin: 3-2) Mocha 예제 코드

간단한 덧셈 함수를 테스트하는 코드로 아래의 예시를 확인해보자.

```javascript
// 덧셈 함수
function add(a, b) {
  return a + b;
}
module.exports = add;

// 테스트 코드 (test.js)
const assert = require("chai").assert;
const add = require("../add");

describe("Add Function", () => {
  it("should return 5 when adding 2 and 3", () => {
    const result = add(2, 3);
    assert.equal(result, 5);
  });

  it("should return 0 when adding 0 and 0", () => {
    const result = add(0, 0);
    assert.equal(result, 0);
  });

  it("should handle negative numbers correctly", () => {
    const result = add(-2, -3);
    assert.equal(result, -5);
  });
});
```

**_실행 방법_**

1. Mocha와 Chai 설치 (`npm install mocha chai --save-dev`)
2. 테스트 실행 (`npx mocha`)

### :fire: 회고

개인적으로 테스트 자동화는 작업시에 추가 작업처럼 느껴지고 시간이 더 든다고 생각하지만, 장기적으로 보았을 때 코드의 안정성과 확장성이 높아 협업에서 크게 도움이 될 거 같다고 생각이 들었다. 작은 단위의 프로젝트에서 사용하기에는 조금 불필요할 수 있지만 규모가 큰 프로젝트에서 큰 도움이 될 거라 생각한다.

#### :pushpin: 참고 문서

- [ko.javascript.info - 테스트 자동화와 Mocha](https://ko.javascript.info/testing-mocha)
