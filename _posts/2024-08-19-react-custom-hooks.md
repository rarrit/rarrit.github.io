---
title: "리액트(react.js) Custom Hooks이란 무엇인가?"
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
  - custom hooks
  - react hooks
author_profile: true
sidebar_main: true
---

## :ledger: 리액트의 커스텀 훅에 대해 알아보자

리엑트에서 커스텀 훅(Custom Hooks)은 여러 컴포넌트에서 공통으로 사용되는 로직을 재사용할 수 있게 해주는 방법이다. 커스텀 훅을 사용하면 코드의 중복을 줄이고, 더 간결하고 유지보수하기 쉬운 코드를 작성할 수 있다! 커스텀 훅에 대해 자세히 알아보자.

### :one: 커스텀 훅이란 무엇인가?

커스텀 훅(Custom Hook)은 리액트의 훅을 사용하여 만든 사용자 정의 함수다. `use`로 시작하는 이름을 가지며, 기존의 훅들을 조합하거나 새로운 로직을 추가하여 새로운 훅을 만들 수 있음.

### :two: 커스텀 훅의 필요성

리액트 컴포넌트에서 동일한 로직이 반복될 때가 많음. 예를들면 데이터 페칭,폼 상태 관리, 이벤트 리스너 관리 등을 여러 컴포넌트에서 사용할 수 있는데 이때 동일한 로직을 각 컴포넌트에 반복해서 작성하는 대신, 컴스텀 훅을 만들어서 로직을 재사용할 수 있음

### :three: 커스텀 훅의 사용법

#### :pushpin: 3-1) 기본 사용법

커스텀 훅은 기본적으로 훅의 로직을 하나의 함수로 감싸는 것이다. 이 함수는 다른 훅들과 마찬가지로 상태를 관리하거나, 부수적인 효과(side effect)를 처리할 수 있음!

- 커스텀 훅의 함수 이름은 `use`로 시작하는 것이 좋다. (예: useInput, useButton)
- 파일 이름은 `use`로 시작할 필요는 없으며, 원하는 대로 지정할 수 있음

```jsx
import { useState, useEffect } from "react";

function useCustomHook() {
  const [state, setState] = useState(initialValue);

  useEffect(() => {
    // 부수 효과 처리 로직
    return () => {
      // 클린업 로직
    };
  }, []);

  // 커스텀 훅이 반환할 값
  return [state, setState];
}
```

#### :pushpin: 3-2) 예시 사용법

input 갯수만큼 늘어나는 useState, event Handler을 만들어보자.

- 아래와 같이 작성하면 input 개수가 증가할 경우 useState, 이벤트 핸들러도 증가하게됨 (코드 중복!)

```jsx
import React from "react";
import { useState } from "react";

const App = () => {
  // input의 갯수가 늘어날때마다 state와 handler가 같이 증가한다.
  const [title, setTitle] = useState("");
  const onChangeTitleHandler = (e) => {
    setTitle(e.target.value);
  };

  // input의 갯수가 늘어날때마다 state와 handler가 같이 증가한다.
  const [body, setBody] = useState("");
  const onChangeBodyHandler = (e) => {
    setBody(e.target.value);
  };

  return (
    <div>
      <input
        type="text"
        name="title"
        value={title}
        onChange={onChangeTitleHandler}
      />
      <input
        type="text"
        name="title"
        value={body}
        onChange={onChangeBodyHandler}
      />
    </div>
  );
};

export default App;
```

#### :pushpin: 3-3) 예시 사용법 개선

1. hooks 폴더에 useInput 파일을 생성 후 아래의 코드를 작성

```jsx
import React, { useState } from "react";

const useInput = () => {
  // 2. value는 useState로 관리하고,
  const [value, setValue] = useState("");

  // 3. 핸들러 로직도 구현합니다.
  const handler = (e) => {
    setValue(e.target.value);
  };

  // 1. 이 훅은 [ ] 을 반환하는데, 첫번째는 value, 두번째는 핸들러를 반환합니다.
  return [value, handler];
};

export default useInput;
```

2. App.jsx 에서 커스텀 훅을 import 해서 원래 있던 훅처럼 사용함

```jsx
// src/App.jsx
import React from "react";
import useInput from "./hooks/useInput";

const App = () => {
  // 우리가 만든 훅을 마치 원래 있던 훅인것마냥 사용해봅니다.
  const [title, onChangeTitleHandler] = useInput();
  const [body, onChangeBodyHandler] = useInput();

  return (
    <div>
      <input
        type="text"
        name="title"
        value={title}
        onChange={onChangeTitleHandler}
      />

      <input
        type="text"
        name="title"
        value={body}
        onChange={onChangeBodyHandler}
      />
    </div>
  );
};

export default App;
```

### :four: 커스텀 훅의 장단점

#### :pushpin: 4-1) 장점

- **_재사용성_**
  - 공통 로직을 훅으로 추출하여 여러 컴포넌트에서 재사용할 수 있음
- **_코드 가독성_**
  - 복잡한 로직을 훅으로 분리하여 컴포넌트의 코드 가독성을 높일 수 있음
- **_테스트_**
  - 로직을 분리하여 개별적으로 테스트할 수 있음

#### :pushpin: 4-2) 단점

- 커스텀 훅 또한 너무 많이 사용하면 관리가 어려워 질 수 있음

### :fire: 마무리

커스텀 훅은 자주 사용되는 공통된 로직을 재사용할 수 있게 해주는 좋은 도구인 것 같다. 과도하게 사용하면 관리가 어렵다는데, 아직 과도하게 사용을 못해봐서 중복되는 로직이 있으면 커스텀 훅을 사용해볼 수 있을까 고민해보는 것도 좋을 것 같다고 생각이 든다.
