---
title: "e.preventDefault와 e.stopPropagation 차이"
date: 2024-08-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - e.preventDefault
  - e.stopPropagation
author_profile: true
sidebar_main: true
---

## :ledger: 이벤트를 처리하는 e.preventDefault()와 e.stopPropagation()의 차이를 알아보자

이벤트를 처리할 때, `e.preventDefault()`와 `e.stopPropagation()`은 매우 유용한 메서드다. 이 두 메서드는 이벤트의 기본 동작을 막거나 이벤트가 부모 요소로 전파되는 것을 막을 때 사용되는데, 이번에 사용하면서 헷갈려서 차이점과 사용 사례 알아보며 다시 한번 정리해보았다.

### :one: 개요

상위 태그에서 onClick이 있고 하위 태그에 버튼 요소가 있는데, 하위 태그의 버튼을 클릭할 때 상위 요소의 이벤트를 막아주려고 했었다. 그런데 오랜만에 사용하니 너무 헷갈렸던 것이었고 `e.preventDefault()`를 남발했었다. `e.stopPropagation()`을 사용하여 문제를 해결했는데, 무지성으로 `e.preventDefault()`를 남발했던 과거에 속죄하고자 둘의 사용법과 차이점을 알아보도록 한다.

### :two: e.preventDefault() : 너, 하지마!

이 친구는 이벤트의 기본 동작을 취소한다. 링크 클릭 시 페이지 이동, 폼 제출 시 페이지 새로 고침 등의 기본 동작이 수행되지 않도록 한다.

#### :pushpin: 2-1) 링크 이동

페이지가 이동하는 기본 동작을 막고 싶을 때 사용한다.

```jsx
const navigate = useNavigate();
const handleSubPageMove = (e) => {
  // 응 안돼.
  e.preventDefault();

  // 가고 싶은데..
  navigate("/sub-page");
};
```

#### :pushpin: 2-2) 폼 제출

```jsx
const navigate = useNavigate();

const handleSubmit = (e) => {
  // 너도 안됌.
  e.preventDefault();

  // 폼 데이터 처리 로직 작성
  // ....

  // ㅠㅠ
  navigate("/success");
};
```

### :three: e.stopPropagation() : 나만 할꺼임

`e.stopPropagation()`은 이벤트가 상위 요소로 전파되는 것을 막는다. 이벤트가 현재 요소에서 처리된 후, 그 이벤트가 부모 요소나 상위 요소로 전달되지 않도록 한다. 즉, 상위 요소에 바인딩된 이벤트 핸들러가 호출되지 않도록 한다.

첫 번째 테스트는 컴포넌트 내에서 사용하고 있던 `useNavigate`, `useLocation`을 context에 옮겼을 때 에러가 노출되었다.

#### :pushpin: 3-1) 버튼 클릭 시 부모 요소의 클릭 이벤트 방지

이번에 헷갈렸던 부분이다. `StCardItem`은 해당 영역을 클릭하면 데이터를 추가하는 이벤트가 적용되어 있고, `StButton`은 상세페이지로 이동하는 이벤트가 적용되어있었다. 웃긴게 내가 원했던 것은 페이지 이동이었는데, e.preventDefault로 페이지 이동을 막아놓고선 왜 안되는거지? 하고 있었던것..! 이럴 경우 `e.stopPropagation()`을 사용하여 `StCardItem`의 이벤트를 막아주었다.

```jsx
const handleMove = (e) => {
  e.stopPropagation();
  navigate(`/detail/${pokemon.id}`);
};

return (
  <StCardItem onClick={handleAdd}>
    <StButton onClick={handleMove}></StButton>
  </StCardItem>
);
```

### :four: 요약

- `e.preventDefault()`

  - 이벤트의 기본 동작을 취소함
  - 링크 클릭 시 페이지 이동, 폼 제출시 페이지 새로 고침 등 클릭한 이벤트의 동작을 막아줌

- `e.stopPropagation()`
  - 이벤트가 상위 요소로 전파되는 것을 막아줌
  - 특정 요소에서만 이벤트를 처리하고자 할 때 사용함

### :fire: 마무리

정말 예전에 이벤트 버블링을 공부하며 분명 `e.stopPropagation`을 배웟는데 `e.PreventDefault`를 많이 사용하다보니, <u>이거안되면 저거로하면 됨</u>으로 무식하게 사용해왔었다. 지난 과거를 속죄하며, 앞으로는 잘 기억해서 용도에 맞게 사용하도록 하겠다.
