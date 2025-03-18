---
title: "[React] 반응형 처리 및 조건부 렌더링 하는 방법"
date: 2025-03-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
tags:
  - responsive
  - react responsive
  - custom hook
author_profile: true
---

## :ledger: [React] 반응형 처리 및 조건부 렌더링 하는 방법

리액트 프로젝트를 진행하던 중 브라우저 넓이에 따라 UI가 변경되는 작업을 진행해야 했다. 역시나 다양한 방법이 있으며, 개인적인 생각을 정리해봤다.

1. `CSS`(`display: none/block`) 사용
2. `useEffect`와 `useState`를 사용
3. `react responsive` 라이브러리 사용
4. `custom hook` 사용

### :one: CSS (none/block)

일반적으로 대부분 알 고 있는 방법으로 CSS `@media`를 사용하여 조건부 처리를 했다.

```scss
@media (min-width: 1024px) {
  .isMobile,
  .isTablet {
    display: none;
  }
  .isDesktop {
    display: block;
  }
}

@media (min-width: 768px) and (max-width: 1023px) {
  .isDesktop,
  .isMobile {
    display: none;
  }
  .isTablet {
    display: block;
  }
}
@media (max-width: 767px) {
  .isDesktop,
  .isTablet {
    display: none;
  }
  .isMobile {
    display: block;
  }
}
```

#### :pushpin: 1-1) 장점

1. `CSS`에서 직접 `display`속성을 처리하여 간단하게 반응형 UI를 구현할 수 있다.
2. `CSS` 속성만 바뀌기 때문에 리렌더링 없이 빠르게 UI를 수정할 수 있다.
3. `display:none`이 적용된 요소는 레이아웃 계산에서 제외되므로 성능에 유리하다.

#### :pushpin: 1-2) 단점

1.  화면에 요소만 보이지 않지만 DOM에 존재하고 개발자 도구에서 쉽게 확인이 가능하다. 민감한 요소나 숨기고 싶은 요소에 대해서는 보안적으로 문제가 될 수 있다.
2.  `display: none`을 사용하면 브라우저가 요소를 숨기지만 리렌더에 따른 복잡한 UI와 자주 변경되면 레이아웃 계산(Reflow)이 발생하여 성능이 저하될 수 있다.

#### :pushpin: 1-3) 개인적인 의견 및 평가

간단한 ui를 제어하거나, `display: none`으로 모바일에서 요소를 가릴 경우에는 성능적으로 이점이 있다. 하지만 MOBILE -> DESKTOP, DESKTOP -> MOBILE으로 변경할 때 마다 ui가 변경되어야하고 변경되는 레이아웃이 많을 경우 성능 저하를 유발할 수 있다.

### :two: useEffect, useState

`useEffect`와 `useState`를 활용하여 브라우저 크기 변경을 감지하고 반응형 상태를 관리한다.

```jsx
import { useEffect, useState } from "react";

const TestUseEffectComp = () => {
  const [isMobile, setIsMobile] = useState(window.innerWidth < 767);
  useEffect(() => {
    const handleResize = () => setIsMobile(window.innerWidth < 767);
    window.addEventListener("resize", handleResize);

    // 중복 이벤트 리스너 방지
    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, []);

  return <div>{isMobile ? <div>MOBILE</div> : <div>PC</div>}</div>;
};

export default TestUseEffectComp;
```

#### :pushpin: 2-1) 장점

- 화면 크기에 따라 상태값을 업데이트하고, 이를 기반으로 조건부 렌더링을 할 수 있다.
- `useEffect`를 호라용하면 미디어 쿼리와 다른 상태 로직과 결합하여 복잡한 로직을 처리할 수 있다.

#### :pushpin: 2-2) 단점

- 상태값이 변경될 때 마다 리렌더링이 발생한다. (성능 저하가 있을 수 있음)
- 다른 컴포넌트에서 필요할 때 재사용이 불가능하다. (사용할 때 마다 작성 필요)

#### :pushpin: 2-3) 개인적인 의견 및 평가

라이브러리 없이 간단하게 사용할 순 있으나, 조건부로 처리해야될 부분이 많아질 경우 재사용이 불가능하여 훅으로 처리하는 방법을 고려해야 될 것 같다. 또한 처리해야하는 컴포넌트가 많아질 경우 리렌더링으로 인한 성능 이슈도 고려해야한다.

### :three: react-responsive 라이브러리

`react-responsive`를 사용하여 손쉽게 반응형 UI를 구현한다.

- 설치 및 사용방법 참고: [반응형, 조건부 렌더링 react-responsive 사용하기](https://rarrit.github.io/react/lib/react-responsive/)

`react-responsive`는 `useMediaQuery`를 사용하여 쉽게 조건부 렌더링 처리가 가능하다. 아래와 같이 Custom 컴포넌트를 생성 후

```js
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

필요한 컴포넌트 내에서 `import` 해서 사용하면 된다.

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

#### :pushpin: 3-1) 장점

- 화면 크기에 맞춰 컴포넌트를 조건부로 렌더링할 수 있다.
- 리액트 상태를 사용하여 조건부 렌더링을 구현하므로 UI와 상태를 관리하기에 유용하다.
- `Desktop`, `Tablet`, `Mobile`, `Default` 같은 컴포넌트 형태로 추상화 되어있어 사용하기 간편함
- 재사용 가능함, 코드 일관성 유지
- SSR 환경에서도 사용 가능 (Next.js, `ssr` 옵션을 사용하면 더욱 안전)

#### :pushpin: 3-2) 단점

- 라이브러리를 추가해야함 (사용하는 것에 비해 불필요한 부분이 있을 수 있음)
- `useMediaQuery`는 `useState`를 사용하여 상태가 변경될 때 마다 리렌더링이 발생함 (성능 최적화는 `matchMedia` API를 활용하여 훅 으로 사용)

#### :pushpin: 3-3) 개인적인 의견 및 평가

반응형 처리에 있어 깔끔하게 적용할 수 있어 유지보수하기에 좋다. 또한 `Next.js`에서도 사용이 가능하며, `matchMedia` API를 활용하여 성능을 최적화할 수 있다. 비록 라이브러리를 설치해서 사용해야 하지만 프로젝트 규모에 따라 선택하면 좋을 것 같다.

### :four: 커스텀 훅

`react-responsive`라이브러리 또한 훌륭하지만, 외부 라이브러리를 설치할 필요 없이 사용할 수 없을까 해서 피티형님께 여쭤봤더니 불가능은 없다고 하셨다. 먼저 아래와 같이 커스텀 훅(`useResponsive`)을 만들어준다.

```js
// hooks/useResponsive.js
import { useState, useEffect } from "react";

const useResponsive = () => {
  // 미디어 쿼리 조건 정의
  const queries = {
    isDesktop: "(min-width: 1024px)",
    isTablet: "(min-width: 768px) and (max-width: 1023px)",
    isMobile: "(max-width: 767px)",
  };

  // 각 미디어 쿼리의 상태를 관리하는 state
  const [matches, setMatches] = useState({
    isDesktop: false,
    isTablet: false,
    isMobile: false,
  });

  useEffect(() => {
    // 클라이언트에서만 실행되도록
    if (typeof window !== "undefined") {
      // 각 미디어 쿼리의 상태를 관리하는 변수
      const mediaQueryLists = Object.entries(queries).reduce(
        (acc, [key, query]) => {
          acc[key] = window.matchMedia(query);
          return acc;
        },
        {}
      );

      // 상태 업데이트 함수
      const updateMatches = () => {
        setMatches({
          isDesktop: mediaQueryLists.isDesktop.matches,
          isTablet: mediaQueryLists.isTablet.matches,
          isMobile: mediaQueryLists.isMobile.matches,
        });
      };

      // 초기 실행
      updateMatches();

      // 이벤트 리스너 등록
      Object.values(mediaQueryLists).forEach((mql) => {
        mql.addEventListener("change", updateMatches);
      });

      // 이벤트 리스너 해제
      return () => {
        Object.values(mediaQueryLists).forEach((mql) => {
          mql.removeEventListener("change", updateMatches);
        });
      };
    }
  }, []);

  return matches;
};

export default useResponsive;
```

이후 사용할 컴포넌트에서 `import`해서 사용해주면 된다.

```jsx
import useResponsive from "../hooks/useResponsive";

const TestCustomHookComp = () => {
  const { isDesktop, isTablet, isMobile } = useResponsive();

  return (
    <>
      {isDesktop && <div>Desktop</div>}
      {isTablet && <div>Tablet</div>}
      {isMobile && <div>Mobile</div>}
    </>
  );
};

export default TestCustomHookComp;
```

#### :pushpin: 4-3) 개인적인 의견 및 평가

테스트 해봤을 때 정상적으로 동작하지만, 깊게 파헤쳐보지 못해서 어떠한 이슈가 있을지는 모르겠다. 외부 라이브러리 없이 사용이 가능하고 재사용성, 유지보수성은 훌륭한 것 같다. 개인적으로 많은 사람들이 사용하는 라이브러리가 더 안전하다 생각되며, 이와같이 커스텀 훅으로 사용한다면 다양한 상황에서 테스트를 진행해보고 사용하면 좋을 것 같다.

### :fire: 결론

리액트에서 반응형 및 조건부 렌더링 처리하는 방법은 다양하게 있는 것 같다. 개인적으로 안전하게 사용하고 싶으면 `react-responsive`, 간단한 구현 및 테스트는 `CSS, useEffect, useState`, 규모가 큰 프로젝트에서는 커스텀 훅으로 생성 후 테스트 및 최적화 후 사용하는 것이 베스트일 것 같다.

#### :pushpin: 테스트

- [https://github.com/rarrit/react-repo/tree/feat/responsive](https://github.com/rarrit/react-repo/tree/feat/responsive)
