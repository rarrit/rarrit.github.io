---
title: "리액트(react.js) Dynamic Route란 무엇인가?"
date: 2024-08-21
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
tags:
  - react
  - react router
  - react dynamic route
  - useParams
author_profile: true
sidebar_main: true
---

## :ledger: Dynamic Route에 대해 알아보자

리액트의 동적 라우팅인 Dynamic Route에 대해 알아보자!

### :one: Dynamic Route란 무엇인가?

- path에 유동적인 값을 넣어서 특정 페이지로 이동하게 구현하는 방법을 `Dynamic Route`라고 한다.

#### :pushpin: 1-1) 사용 이유

- **_유연한 URL 구조_**
  - 경로상의 일부분을 변수로 처리하여, 동일한 컴포넌트를 다양한 데이터로 렌더링할 수 있음.
    - 예를들어 `/pokemon/1`, `/pokemon/2` 같은 URL을 각각 다른 상세페이지로 연결할 수 있음
  - 별도의 페이지를 생성하지 않고 다양한 데이터를 쉽게 처리할 수 있음
- **_재사용성_**
  - `/pokemon/1`에 대한 컴포넌트를 생성하지 않고 동적으로 데이터만 출력해주는 거라 중복 코드를 줄이고 재사용 가능하며 유지보수가 쉬움
- **_SEO_**
  - `/pokemon/1` 이 아닌 `/pokemon/피카츄`와 같이 의미있는 URL을 구성할 수 있어 SEO(검색 엔진 최적화)에 유리함

#### :pushpin: 1-2) 예시를 통한 이해

- 만약 `detail`페이지에 여러개의 `pokemon`이 1억개가 있다면? 어떻게 할 것인가?
- 아래의 코드를 보면 이해하기 쉽다. `Route`를 1억개까지 추가하는건 비효율적이여서 `Dynamic Route`를 사용해야 되는 것!

```jsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "@/pages/Home";
import Dex from "@/pages/Dex";
import Layout from "./Layout";
import Detail from "@/pages/Detail";

const Router = ({ pokemonList }) => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="dex" element={<Dex pokemonList={pokemonList} />} />
          {/* detail이 10000000000 개면? Route는 10000000000개가 됨 */}
          <Route
            path="detail/1"
            element={<Detail pokemonList={pokemonList} />}
          />
          <Route
            path="detail/2"
            element={<Detail pokemonList={pokemonList} />}
          />
          <Route
            path="detail/3"
            element={<Detail pokemonList={pokemonList} />}
          />
          <Route
            path="detail4"
            element={<Detail pokemonList={pokemonList} />}
          />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};
export default Router;
```

### :two: Dynamic Route 사용법

#### :pushpin: 2-1) 기본 설정

- `detail/:id` :id 라는 것이 바로 동적인 값을 받겠다라는 의미다.
- :id 값으로 설정하면, detail/1, detail/2 ... 각 path값은 다르지만 보여지는 컴포넌트는 같음
- 보여지는 컴포넌트가 같으니 :id 값에 맞춰 어떻게 데이터를 출력하느냐 고민해보면 됨
- 아래는 요번 과제를 진행하며 `props drilling`으로 작업했던 예시이다.

```jsx
// app.jsx
// pokemonList 를 router로 전달함
function App() {
  // 전체 포켓몬 목록
  const [pokemonList, setPokemonList] = useState(Poketmon);
  return (
    <>
      <GlobalStyle />
      <Router pokemonList={pokemonList}/>
    </>
  )
}

// router.jsx
import { BrowserRouter, Route, Routes } from "react-router-dom"
import Home from "@/pages/Home";
import Dex from "@/pages/Dex";
import Layout from "./Layout";
import Detail from "@/pages/Detail";

const Router = ({ pokemonList }) => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
          {/* App.jsx에서 전달받은 pokemonList를 Dex,Detail 페이지에 전달 */}
          <Route path="dex" element={<Dex pokemonList={pokemonList}/>} />
          <Route path="detail/:id" element={<Detail pokemonList={pokemonList}/>} />
        </Routes>
      </Layout>
    </BrowserRouter>
  )
}
export default Router

// detail.jsx

import { useParams } from 'react-router-dom';

const Detail = ({ pokemonList }) => {
  // URL에서 id를 추출
  const { id } = useParams();
  console.log( "useParams id =>", id );

  // 포켓몬 목록에서 해당 id에 맞는 포켓몬을 찾기
  const selectedPokemon = pokemonList.find(pokemon => pokemon.id === parseInt(id));

  if (!selectedPokemon) {
    return <div>포켓몬을 찾을 수 없습니다.</div>;
  }

  return (
    <div>
      <h1>{selectedPokemon.korean_name}</h1>
      <img src={selectedPokemon.img_url} alt={selectedPokemon.korean_name} />
      <p>{selectedPokemon.description}</p>
    </div>
  );
};

export default Detail;
```

#### :pushpin: 2-2) 요약

1. `Detail.jsx`에서 pokemonList를 props를 통해 전달받음
2. `useParams`를 통해 id 값을 확인
3. props를 통해 전달받은 pokemonList의 pokemon.id와 현재 `useParams`의 id값을 비교하여 선택된 포켓몬이 있는지 확인 후 해당 객체를 변수(`selectPokemon`)에 담아줌
4. 변수에 담아준 포켓몬을 출력해줌

![dynamic route example image](https://github.com/user-attachments/assets/7e489ec3-ca7a-4225-ab82-98ab9eaaa62f)

### :fire: 마무리

리액트의 동적 라우팅인 `Dynamic Route`를 사용해보았다. 예전부터 상세페이지가 많을 경우 어떻게 처리했는지 궁금했었는데, 강의를 볼 때와 내가 작업하고 있는 파일 구조가 달라서 조금 어려움도 있었으나 정상적으로 구현된것을 보니 뿌듯했다.
