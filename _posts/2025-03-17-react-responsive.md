---
title: "[React] 반응형, 조건부 렌더링 react-responsive 사용하기"
date: 2025-03-17
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - lib
tags:
  - react responsive
author_profile: true
---

## :ledger: [React] 조건부 렌더링 react-responsive 사용하기

`React`에서 반응형 디자인을 구현하려면 화면 크기에 따라 UI를 다르게 렌더링해야 하는 경우가 많다. 이를 효율적으로 처리할 수 있는 라이브러리인 `react-responsive` 사용 방법을 알아보자

### :one: 설치 방법

```bash
# npm
npm install react-responsive
# yarn
yarn add react-responsive
```

### :two: 사용 방법

1. `useMediaQuery` 훅 사용
2. `MediaQuery` 컴포넌트 사용
3. custom 컴포넌트 사용

### :pushpin: 2-1) useMediaQuery 훅 사용

- 화면 크기가 `767px` 이하일 경우 MOBILE
- 화면 크기가 `768px` 이상일 경우 DESKTOP

```jsx
import { useMediaQuery } from "react-responsive";

const TestReactResponsiveComp = () => {
  const isMobile = useMediaQuery({ query: "(max-width: 767px)" });
  return <div>{isMobile ? <div>MOBILE</div> : <div>DESKTOP</div>}</div>;
};

export default TestReactResponsiveComp;
```

### :pushpin: 2-2) MediaQuery 컴포넌트 사용

`MediaQuery` 컴포넌트 내에서 컴포넌트로 조건부 렌더링을 처리할 수 있음

```jsx
import { MediaQuery } from "react-responsive";

const TestReactResponsiveComp = () => (
  <div>
    <MediaQuery query="(max-width: 767px)">
      <p>MOBILE</p>
    </MediaQuery>
    <MediaQuery query="(min-width: 768px)">
      <p>DESKTOP</p>
    </MediaQuery>
  </div>
);
```

### :pushpin: 2-3) Custom 컴포넌트 사용

컴포넌트를 재사용하기 위해 아래와 같이 디바이스별 렌더링을 처리함

```jsx
// utils/MediaQuery.js

import { useMediaQuery } from "react-responsive";

const Desktop = ({ children }) => {
  const isDesktop = useMediaQuery({ minWidth: 1024 });
  return isDesktop ? children : null;
};

const Tablet = ({ children }) => {
  const isTablet = useMediaQuery({ minWidth: 768, maxWidth: 1023 });
  return isTablet ? children : null;
};

const Mobile = ({ children }) => {
  const isMobile = useMediaQuery({ maxWidth: 767 });
  return isMobile ? children : null;
};

const Default = ({ children }) => {
  const isNotMobile = useMediaQuery({ minWidth: 768 });
  return isNotMobile ? children : null;
};

export { Desktop, Tablet, Mobile, Default };
```

`MediaQuery.js`로 분리 후 아래와 같이 컴포넌트에서 사용

```jsx
import { Default, Desktop, Mobile, Tablet } from "../utils/MediaQuery";
const TestReactResponsiveComp = () => {
  return (
    <>
      <Desktop>Desktop</Desktop>
      <br />
      <Tablet>Tablet</Tablet>
      <br />
      <Mobile>Mobile</Mobile>
      <br />
      <Default>Not Mobile</Default>
      <br />
    </>
  );
};

export default TestReactResponsiveComp;
```

### :three: 장단점

#### :pushpin: 3-1) 장점

- 설치 및 사용방법이 간단하다.
- 조건부 렌더링이 가능하다.
- 재사용 가능하며 중복 코드를 줄일 수 있다.

#### :pushpin: 3-2) 단점

- 화면 크기 변화에 따라 상태 업데이트가 발생하고, 그에 따라 컴포넌트 리렌더링이 발생한다. (성능 저하가 있을 수 있음)
- `useMediaQuery`를 사용하려면 외부 라이브러리(`react-responsive`)에 의존하게 되므로, 라이브러리를 설치하고 관리해야 한다. (버전에 따른 호환성 문제가 생긴다면? )

### :fire: 요약

`react-responsive`는 조건부 처리 및 반응형 디자인을 구현할 때 유용한 라이브러리다. 화면 크기에 따라 동적으로 렌더링할 수 있고 상황에 맞게 커스텀해서 사용하기에 좋다. 하지만 프로젝트 규모와 어떻게 사용함에 따라 성능적인 이슈가 있을 수 있다.

#### :pushpin: 참고

- ![[React] react-responsive 반응형 라이브러리 + Typescript](https://myung-ho.tistory.com/86)
- ![react-responsive 공식 문서](https://www.npmjs.com/package/react-responsive)
