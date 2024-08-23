---
title: "리액트(react.js) Router Hooks에 대해 알아보자"
date: 2024-08-22
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
tags:
  - react router
  - react router hooks
  - use location
  - use params
author_profile: true
sidebar_main: true
---

## :ledger: React Router DOM에 대해 알아보자

리액트에서 라우팅을 쉽게 관리할 수 있게 해주는 (Hook) 중 `useLocation`과 `useParams`에 대해 알아보자.

### :one: 개요

`useLocation`과 `useParams`이 훅들은 라우팅을 관리하고, 다양한 컴포넌트 간의 데이터를 효율적으로 전달하는데 필수적으로 사용된다고 한다. 리액트에서의 역할과 활용방법에 대해 알아보자!<br/>

### :two: useLocation

`useLocation`은 <u>현재 페이지의 위치 정보(경로,검색 문자열,해시값 등)</u>를 가져오는 훅이다.

#### :pushpin: 2-1) 사용방법

```jsx
import { useLocation } from "react-router-dom";

const 컴포넌트 = () => {
  const location = useLocation();
  return (
    <>
      <p>현재 경로: {location.pathname}</p>
      <p>쿼리 문자열: {location.search}</p>
      <p>해시값: {location.hash}</p>
    </>
  );
};
```

#### :pushpin: 2-2) 주요 속성

- `pathname` : 현재 URL의 경로
- `search` : 쿼리 문자열 (예: `?name=Rarrit`)
- `hash` : URL의 해시값 (예: `#section1`)
- `state` : 페이지 이동 시 전달된 상태 값

#### :pushpin: 2-3) useLocation - pathname 예제 (조건부 렌더링)

`pathname`을 사용하여 현재 URL의 경로를 추출 후 조건부 렌더링을 구현해본다.

- location.pathname을 통해 현재 경로를 확인함
- 현재 경로가 '/'이라면 노출될 수 있게 적용함

```jsx
import { useLocation } from "react-router-dom";

const Header = () => {
  const location = useLocation();

  return (
    <header>
      <h1>Logo</h1>
      {/* 루트 페이지에서만 아래의 문구가 노출된다. */}
      {location.pathname === "/" && <p>홈페이지에 오신 것을 환영합니다!</p>}
    </header>
  );
};

export default Header;
```

#### :pushpin: 2-4) useLocation - search 예제 (검색기능)

`search`를 사용하여 쿼리 문자열을 추출 후 검색 기능을 구현해본다.

- location.search에서 쿼리 문자열을 추출한다.
- URLSearchParams 객체를 사용해 특정 키워드를 가져온다.

```jsx
import { useLocation } from "react-router-dom";

const SearchResults = () => {
  const location = useLocation();
  const query = new URLSearchParams(location.search);
  const keyword = query.get("q");

  return <div>{`검색 결과: ${keyword}`}</div>;
};

export default SearchResults;
```

#### :pushpin: 2-5) useLocation - hash 예제 (특정 섹션 확인)

`hash` 값을 활용하여 페이지 내 특정 섹션으로 갔을 때 조건을 줄 수 있음.

- location.hash를 사용하여 특정 url의 해쉬값을 비교해 조건을 줄 수 있음

```jsx
import { useLocation } from "react-router-dom";

const SectionHighlight = () => {
  const location = useLocation();

  return <div>{location.hash === "#section1" && <p>섹션 1</p>}</div>;
};

export default SectionHighlight;
```

### :three: useParams

`useParams`는 URL의 파라미터를 가져오는 훅이다. 이를 통해 동적 경로에서 전달된 값을 손쉽게 사용할 수 있음

- useLocation와는 다르게 `useParams`는 특정 속성은 없지만 동적 파라미터를 추출하여 객체 형태로 반환함

#### :pushpin: 3-1) 사용방법

```jsx
// detail.jsx : url/detail/id

// URL에서 id를 추출
const { id } = useParams();

// 포켓몬 목록에서 해당 id에 맞는 포켓몬을 찾기
const selectedPokemon = pokemonList.find(
  (pokemon) => pokemon.id === parseInt(id)
);
```

### :fire: 마무리

`useLocation`과 `useParams`훅을 활용하면, 리액트에서 현재 위치나 동적 경로의 파라미터를 손쉽게 관리할 수 있다.
