---
title: "리액트(react.js) React Router DOM이란 무엇인가?"
date: 2024-08-19
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
  - react router hooks
author_profile: true
sidebar_main: true
---

## :ledger: React Router DOM에 대해 알아보자

리액트 프로젝트에서 라우팅을 가능하게 하는 라이브러리인 `react-router-dom`에 대해 알아보자!

### :one: React-router-dom이란 무엇인가?

이 페이지 저 페이지 왔다갔다 하고싶다? `React-router-dom`을 배우면 된다. 즉, 페이지를 이동을 구현할 수 있게 해주는 패키지다~ 라고 이해하면 된다.

#### :pushpin: 1-1) Routing이 뭐야?

사용자가 웹 애플리케이션에서 특정 경로(URL)에 접근할 때 그 경로에 맞는 컴포넌트를 렌더링하는 것을 말함

### :two: 설치 방법

터미널을 열어 npm, yarn 둘 중 사용하면 된다.

#### :pushpin: 2-1) npm

```bash
npm install react-router-dom
```

#### :pushpin: 2-2) yarn

```bash
yarn add react-router-dom
```

### :three: 사용방법

#### :pushpin: 3-1) 페이지 생성

페이지는 `src`폴더에 pages 폴더를 생성 후 jsx로 생성해줌. 파일 네이밍은 가독성 좋게 적용하면 될 듯!

#### :pushpin: 3-2) Router.js 생성 및 route 설정 코드 작성

중요한 부분임. 브라우저에 url을 입력하고 이동했을 때 우리가 원하는 페이지 컴포넌트로 이동하게 만드는 부분!

1. `src` 안에 `shared` 폴더를 생성 후 `router.jsx`파일 생성
2. 아래의 코드를 작성함

- `<Route>` 의 `path` 에 경로를 지정하고 `element`에 페이지로 생성한 컴포넌트를 넣어주면 된다.

```jsx
import React from "react";
// react-router-dom을 사용하기 위해서 아래 API들을 import 해야함
import { BrowserRouter, Route, Routes } from "react-router-dom";

// 2. Router 라는 함수를 만들고 아래와 같이 작성합니다.
// BrowserRouter를 Router로 감싸는 이유는,
// SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어줌!
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<컴포넌트 />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

#### :pushpin: 3-3) App.jsx에서 Router import

App.jsx 파일에서도 Router를 import 후 `return <router/>` 해주면 끝!

- `Router.jsx`를 App컴포넌트에 넣어주는 이유는 최상이 컴포넌트여서 그렇다.

```jsx
import Router from "./shared/Router";
const App = () => {
  return <Router />;
};
export default App;
```

### :four: react-router-dom hooks

`react-router-dom`에서도 useState와 같은 Hook을 제공함. 자주 사용하는 훅만 정리해보았다.

#### :pushpin: 4-1) useNavigate

- 다른 페이지로 보내고자 할 때 사용함
- 위에서 기본적으로 `path="/"`를 사용했지만 `useNavigate`를 통해 onClick 사용 가능

```jsx
// useNavigate 사용하려면 역쉬 import 해줘야함.
import { useNavigate } from "react-router-dom";

const Home = () => {
  // navigate 변수에 넣어줌
  const navigate = useNavigate();

  return (
    <button
      onClick={() => {
        {
          /* onClick을 통해서도 쉽게 사용 가능~ */
        }
        navigate("/works");
      }}
    >
      works로 이동
    </button>
  );
};

export default Home;
```

#### :pushpin: 4-2) useLocation

- 현재 위치하고 있는 페이지의 여러가지 정보를 추가적으로 얻을 수 있음
- 이 정보들을 이용해서 페이지 안에서 조건부 렌더링에 사용할 수 있음

```jsx
// src/pages/works.js
import { useLocation } from "react-router-dom";

const Works = () => {
  // 현재 경로에 대한 정보를 가진 객체를 location에 담아줌
  const location = useLocation();

  // pathname, search, hash, state, key 확인 가능!
  console.log("location :>> ", location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
    </div>
  );
};

export default Works;
```

#### :pushpin: 4-3) Link

- 이 친구는 사실 훅이 아님 하지만 반드시 알아야 할 API임
- `Link`는 html의 a태그를 대체하는 기능임. <u>a태그 사용하려면 반드시</u> `Link`로 사용해야함
  - 이유는 a태그를 사용하면 브라우저가 새로고침이 되기 때문에 `Link`를 사용해서 새로고침을 막아주자.

```jsx
// 마찬가지로 Link import 해야함
import { Link, useLocation } from "react-router-dom";

const Works = () => {
  const location = useLocation();
  console.log("location :>> ", location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
      {/* a tag 사용해야 겠~~다면 Link 를 사용하도록 ! */}
      <Link to="/contact">contact 페이지로 이동하기</Link>
    </div>
  );
};

export default Works;
```

#### :pushpin: 4-4) children

개인적으로 가장 궁금했던 부분이었다. 리액트에선 헤더,푸터를 어떻게 구조화 했을 까(?) 였는데, `children`을 알게되고 궁금증이 쏵~ 사라짐! 사용 방법은 아래와 같다.

- 아래의 Layout.jsx에서 헤더,푸터를 생성 후 Layout에 넣어준다. `{children}`은 아래의 코드의 Routes,Route인데, 이 두가개의 컴포넌트가 `<Header/>,<Footer/>`사이에 배치되도록 하는거다. 내가 이해한게 맞는지 잘 모르겠지만 그렇게 렌더링됨!

```jsx
// src/shared/Layout.jsx
function Header(){return ( 헤더 맘껏 꾸며주셈! )}
function Footer(){return ( 푸터 맘껏 꾸며주셈! )}
function Layout({ children }) {
  return (
    <Header/>
    {children}
    <Footer/>
  )
}
export default Layout;

// src/shared/Router.jsx
const Router = () => {
  return (
    <BrowserRouter>
      {/* Layout을 */}
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};
```

### :fire: 마무리

리액트 라우터를 통해 <u>페이지를 슉슉 쇽쇽 할 수 있게 되었다.</u> 지금 개인 과제를 진행하면서도 유용하게 사용을 해보았으나, 동적 라우팅에 대해 분명 강의를 봤는데 기억이 잘 나지 않아서 다시 보고 `Dynamic Route`에 대해 글을 작성해보려한다.
