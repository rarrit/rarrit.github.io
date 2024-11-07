---
title: "[Next.js] 서버 액션에서 try-catch 대신 Response.ok() 사용하기"
date: 2024-11-03
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
tags:
  - try-catch
  - response
author_profile: true
sidebar_main: true
---

## :ledger: [Next.js] 서버 액션에서 try-catch 대신 Response.ok() 사용하기

프로젝트에서 서버 액션을 구현할 때, 에러 처리를 위해 `try-catch` 블록을 사용하는데 많이 익숙해졌다. 하지만 서버 액션에서는 `return Response.ok()` 방식이 더 권장되며, 특히 안정성과 코드의 일관성을 높이는 데 유리하다. 서버 액션에서 `try-catch` 대신 `Response.ok()` 방식을 사용해야 하는지 그 이유를 살펴보겠습니다.

### :one: try-catch 블록의 문제점

이 부분이 가장 헷갈렸는데, 서버액션에서 `try-catch`를 사용할 수 있기에 더 헷갈렸던 것 같다. `try-catch`의 문제점은 에러를 포착하여 특정 동작을 수행할 수 있게 해주지만, 서버 액션에서 이를 사용하는 경우 몇 가지 단점이 있다.

- `코드 복잡성 증가`
  - 여러 에러 발생 가능성을 관리하기 위해 반복적으로 try-catch를 사용하는 경우 코드가 길어지고 복잡해질 수 있음
  - 특히 비슷한 서버 액션 코드에서 반복적으로 에러 핸들링이 필요한 경우 중복이 발생할 수 있음
- `일관된 응답 형식 제공 어려움`
  - `try-catch`를 통해 다양한 에러를 각각 다른 방식으로 처리하면 클라이언트 측에서 예측 가능한 응답 형식을 유지하기 어려워짐
  - 클라이언트가 요청 성공/실패를 판단하기 위해 일관된 응답 형식을 기대하는 경우, 예외가 발생할 수 있음

### :two: return Response.ok() 방식의 장점

#### :pushpin: 2-1) 일관된 응답 제공

`Response.ok()` 방식은 항상 동일한 응답 구조를 반환하도록 하여, 클라이언트가 응답을 예측하고 처리하기 쉽게 만들어준다. 서버에서 발생한 모든 에러 상황을 `Response.error()` 또는 `Response.ok(false)`와 같은 형식으로 통일하여 응답하면 클라이언트 측 코드의 유지보수성이 높아진다.

```tsx
// 예시: 서버 액션에서 Response.ok() 방식 사용하기
export async function serverActionExample() {
  const result = await fetchData();

  if (!result) {
    return new Response("Error fetching data", { status: 500 });
  }

  return new Response(JSON.stringify(result), { status: 200 });
}
```

#### :pushpin: 2-2) 코드의 가독성과 간결성 유지

- `try-catch`를 반복적으로 사용하는 대신, 에러 상황을 하나의 `Response` 객체로 통합하면 코드가 간결해진다.
- 에러가 발생하더라도 코드 흐름을 간단히 유지하면서 필요할 때만 에러 처리를 세부적으로 수행할 수 있다.

#### :pushpin: 2-3) 일관된 방식

- `Response.ok()`를 사용하면 클라이언트는 성공/실패 여부를 HTTP 상태 코드와 함께 일관된 방식으로 확인할 수 있다.
- 이로써 서버와 클라이언트 사이의 ‘계약’이 확실해지고, 클라이언트가 서버 응답을 처리할 때 오류에 대한 예외 상황을 쉽게 예상할 수 있다.

### :three: Response.ok와 Response.error 활용

서버 액션에서 `Response.ok`와 `Response.error`를 함께 활용하여, 클라이언트와의 명확한 의사소통을 할 수 있다. 서버 액션의 결과가 성공적인 경우와 에러가 발생한 경우를 일관되게 처리할 수 있도록 아래와 같은 방식을 사용할 수 있다.

```tsx
export async function someServerAction() {
  const result = await fetchData();

  // 데이터가 없을 경우 에러 응답 반환
  if (!result) {
    return new Response("데이터를 불러올 수 없습니다.", { status: 404 });
  }

  // 성공적으로 데이터를 반환하는 경우
  return new Response(JSON.stringify(result), { status: 200 });
}
```

### :fire: 마무리

서버 액션에서 `Response.ok()` 방식을 사용함으로써 얻을 수 있는 장점은 코드 일관성, 가독성, 클라이언트와의 명확한 응답 구조이다. `try-catch`를 반복적으로 사용하는 대신, `Response.ok()`와 같은 일관된 응답 패턴을 통해 더 깔끔하고 유지보수하기 좋은 서버 액션을 작성해보도록 해야겠다.
