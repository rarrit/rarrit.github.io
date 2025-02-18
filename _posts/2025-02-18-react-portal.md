---
title: "[React] Portal 사용 이유, 사용법을 알아보자 (Modal 예시)"
date: 2025-02-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
tags:
  - portal
author_profile: true
sidebar_main: true
---

## :ledger: [React] Portal 사용 이유, 사용법을 알아보자 (Modal 예시)

리액트 공통 모달을 만들기 위해 구글링을 해봤는데, 많은 사람들이 `React Portal`을 이용하여 모달을 만들었다. 처음 알게되어서 무엇인지 자세히 알아보도록 했다.

### :one: React Portal이란?

`React Portal`은 `React`의 일반적인 렌더링 흐름을 따르지 않고, 특정 DOM 노드에 컴포넌트를 렌더링할 수 있도록 하는 기능이다. `ReactDOM.createPortal` 함수를 사용하여 구현된다.

- `Portal`은 기존의 DOM 구조를 따르지 않고 원하는 위치(보통 body 아래)에 렌더링할 수 있다.
- 주로 모달, 툴팁, 드롭다운 메뉴 등의 UI 요소를 구현할 때 유용하다.

이전에 공통 모달을 만들 때 부모의 스타일에 영향을 받고 렌더링이 되는 위치도 일정하지 않아서 css 처리를 해줘야했는데 `Portal`의 경우 DOM 구조에 벗어나 별도의 위치에서 렌더링되므로, 스타일링이 훨씬 수월해지는 것 같다.

### :two: Portal 사용 방법

#### :pushpin: 2-1) index.html 설정

`index.html`에서 `Portal`을 삽입할 DOM 요소를 생성해준다.

```html
<body>
  <div id="root"></div>
  <!-- root 아래에 portal-modal 생성 --->
  <div id="portal-modal"></div>
  <script type="module" src="/src/main.jsx"></script>
</body>
```

#### :pushpin: 2-2) Portal을 사용한 컴포넌트 생성

`ReactDOM.createPortal`을 활용하여 특정 DOM 요소에 컴포넌트를 렌더링한다.

- `ReactDOM.createPortal(element, container)`
  - `element`: 렌더링할 JSX 요소
  - `container`: 해당 요소를 렌더링할 DOM 요소 (`document.getElementById("portal-modal")`)

```jsx
import ReactDOM from "react-dom";

const Modal = ({ children }) => {
  return ReactDOM.createPortal(
    <div className="modalWrap">
      <div className="modalCont">{children}</div>
    </div>,
    document.getElementById("portal-modal")
  );
};

export default Modal;
```

#### :pushpin: 2-3) 사용 및 확인

원하는 위치에 `Modal`컴포넌트를 넣어도 렌더링될 때 처음 `index.html`에 지정해줬던 위치에서 렌더링된다.

![portal-code-image](https://github.com/user-attachments/assets/0b84f023-e541-4911-8e59-33d15950363b)

### :three: 이벤트 버블링

`Portal`을 사용하면 부모 컴포넌트가 아닌 다른 DOM 위치에 렌더링되지만, `React`의 **이벤트 버블링 특성**은 유지된다. 즉, 부모 요소의 `onClick` 핸들러가 실행될 수 있다.

- 이를 방지하려면 `event.stopPropagation()`을 활용하자.

```jsx
const Modal = ({ children, onClose }) => {
  return ReactDOM.createPortal(
    <div className="ModalWrap" onClick={onClose}>
      <div className="ModalCont" onClick={(e) => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.getElementById("portal-modal")
  );
};
```

### :fire: 정리

- `React Portal`은 `ReactDOM.createPortal`을 사용하여 특정 DOM 요소에 컴포넌트를 렌더링할 수 있다.
- `Portal`을 활용하면 부모의 스타일 영향을 받지 않고 원하는 위치에 UI 요소를 렌더링할 수 있다.
- 이벤트 버블링을 주의해야 하며, `stopPropagation()`을 활용하여 해결할 수 있다.
