---
title: "리액트(react.js) Styled Component"
date: 2024-08-16
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
  - style component
  - css in js
author_profile: true
sidebar_main: true
---

css-in-js
공식문서를 보면 css for the <component> age 즉 css를 꾸미기 위한 컴포넌트라 되어있음
1. 설치방법 터미널: yarn add styled-components
2. 브이에스코드 익스텐션(마켓) - vscode-styled-components 설치
3. app에서 `import styled from "styled-components";`
4. const 컴포넌트명 = styled.태그`style 작성`;



## :ledger: 리액트의 Styled Components에 대해 알아보자!
숙련 주차에 오면서 가장 먼저 배운 React의 Styled Components! 강의를 보고 정말 자바스크립트로 다 해먹겠다란 강력한 의지가 보여서 신박했다.

### :one: Styled Components란?
자바스크립트에서 CSS를 작성할 수 있도록 해주는 라이브러리이며 이를 통해 스타일 정의를 컴포넌트 내부에 포함시키고 각각의 컴포넌트가 고유한 스타일을 가질 수 있다. 리액트의 Styled Components를 알아보기 위해 공식문서를 보면 <u>css for the `<component>` age</u> 즉 css를 꾸미기 위한 컴포넌트라 되어있음. 

### :two: Styled Components 설치
VSCode를 사용한다면 styled-components를 편하게 사용하기 위해 마켓플레이스에서 `vscode-styled-components`를 다운받는다.
#### :pushpin: 2-1) npm
```bash
npm install styled-components
```

#### :pushpin: 2-2) yarn
```bash
yarn add styled-components
```

### :three: Styled Components 사용법
터미널에서 styled-components를 설치 후 사용할 컴포넌트 최상단에 기본적으로 `import styled from "styled-components";`를 작성한다.
#### :pushpin: 3-1) 기본적인 사용법
아래의 문법은 그냥 외우는게 좋을 듯!

```jsx
// styled-components 사용할거임~
import styled from "styled-components";

// styled 키워드를 사용하여 styled-components 방식대로 컴포넌트 생성함
const StBox = styled.div`
  width: 100%;
  height: 100%;
  ... css 코드 입력
`

const App = () => {
  // 위에서 만든 styled-components를 JSX에서 html 태그 사용하듯이 사용함
  return <StBox>쉽죵?</StBox>;
}
export default App;
```

#### :pushpin: 3-2) 조건부 스타일링
이것이 자바스크립트가 다 해쳐먹겠다는 강력한 의지가 돋보인게 아닐까 싶음.. 쉽게 설명하면 props로 전달하여 조건에 맞춰 스타일을 적용이 가능함

```jsx
import React from "react";
import styled from "styled-components";

// 전달 받은 props를 사용함
const StBox = styled.div`
  width: ${props => props.width}px;
  height: ${props => props.height}px;
`;

const App = () => {
  return (
    <div>
			{/* props 를 통해 width,height 값을 전달함 */}
      <StBox width="100" height="100">정사각형</StBox>
      <StBox width="200" height="100">직사각형</StBox>      
    </div>
  );
};

export default App;
```

#### :Pushpin: 3-3) 자바스크립트 활용
정말 자바스크립트는 유연 그자체인 것 같다. 이렇게 활용하다니 대단.. 
1. 박스의 색상을 배열로 담아줌
2. `switch`문을 사용하여 색상의 값에 따라 이름을 반환하는 함수를 만듬
3. 컴포넌트 내에서 `map`메서드를 사용하여 [1]에서 담아준 색상을 props로 전달하여 `switch`문으로 반환되는 값을 랜더링하게 해줌

```jsx
// src/App.js

import React from "react";
import styled from "styled-components";

const StContainer = styled.div`
  display: flex;
`;

const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid ${(props) => props.borderColor};
  margin: 20px;
`;
// 박스의 색을 배열에 담습니다.
const boxList = ["red", "green", "blue"];

// 색을 넣으면, 이름을 반환해주는 함수를 만듭니다.
const getBoxName = (color) => {
  switch (color) {
    case "red":
      return "빨간 박스";
    case "green":
      return "초록 박스";
    case "blue":
      return "파란 박스";
    default:
      return "검정 박스";
   }
};
const App = () => {
  return (
    <StContainer>
			{/* map을 이용해서 StBox를 반복하여 화면에 그립니다. */}
      {boxList.map((box) => (
        <StBox borderColor={box}>{getBoxName(box)}</StBox>
      ))}
    </StContainer>
  );
};

export default App;
```

### :four: Styled Components 전역 스타일링
styled components를 보면 컴포넌트 내에서만 활용할 수 있다. 그렇지만 공통적으로 들어가야 하는 스타일은 global로 지정해줘야하는데 어떻게 하는지 알아보자.

#### :pushpin: 4-1) GlobalStyle.jsx 생성
GlobalStyle.jsx 파일을 생성 후 아래와 같이 작성해준다. 외울 수 있을 것 같지만 기억안나면 널리널리 잘 알려져있기에 c+c, c+v로 해치우자.

```jsx
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  body {
    ...작성
  }
`;

export default GlobalStyle;
```

#### :pushpin: 4-2) App.jsx에서 Import 해준다.
GlobalStyle도 결국 하나의 컴포넌트로 다른 컴포넌트와 똑같이 App.jsx에서 Import해준다.

```jsx
import GlobalStyle from "./GlobalStyle";

function App() {
  return (
    <>
      <GlobalStyle />
    </>
  );
}

export default App;
```

### :five: SCSS 와 Styled Components
이전에 SCSS를 사용한 경험이 있는데, 리액트에서 사용하는 Styled Components와 어떤 차이가 있는지 알아보자.
#### :pushpin: 5-1) 개념 및 사용 방식 
- `SCSS`
  - SCSS는 Sass의 확장 문법으로 CSS를 더 효율적으로 작성하기 위해 변수,중첩,믹스인,함수 등의 기능을 제공한다. SCSS파일은 별도로 작성되며, 이 파일을 CSS로 컴파일하여 HTML에서 사용함
- `Styled Components`
  - 리액트와 같은 자바스크립트 기반 프레임워크에서 CSS-in-JS 방식을 사용하여 스타일을 작성한다. 스타일을 컴포넌트와 함께 자바스크립트 파일 내에서 정의하고, 동적으로 스타일을 변경하거나 컴포넌트에 props를 전달할 수 있다.

#### :pushpin: 5-2) 스타일 관리 방식
- `SCSS`
  - ***전역 스타일*** : SCSS는 전역적으로 스타일을 적용하는 방식으로 작성된 SCSS파일은 컴파일 된 후 HTML문서에 포함되어 모든 요소에 영향을 미칠 수 있음
  - ***모듈화*** : 파일을 분리하고 `@import` 를 사용하여 모듈화된 스타일을 관리할 수 잇음
  - ***재사용성*** : mixin, 함수, 변수 등을 사용하여 스타일의 재사용성을 높일 수 있음
- `Styled Components`
   - ***컴포넌트 기반 스타일*** : 각 컴포넌트에 스타일을 묶어 관리해서 컴포넌트 내부에서만 스타일이 적용되므로, 다른 컴포넌트와 스타일 충돌을 방지할 수 있음
   - ***동적 스타일링*** : 자바스크립트와 Props를 활용하여 동적으로 스타일을 변경 가능함
   - ***고유 클래스명 생성*** : 자동으로 고유한 클래스명을 생성하여, 전역 네임스페이스의 충돌을 방지해줌

#### :pushipin: 5-3) 결론
- SCSS는 기존 CSS 관리 방식에 확장된 기능을 제공하고 정적인 스타일 관리에 강함
- Styled Components는 리액트와 같은 컴포넌트 기반 개발에서 스타일링을 직관적이고 효율적이게 관리해줌

사실 두 가지를 보면서 느낀건 어느것이 더 좋다! 라고는 확실히 모르겠다. 결국 css-in-css와 css-in-js의 차이일 뿐이고 속도면에서 js가 조금 느리다곤 하지만 그냥 취향 차이일 듯 싶은데, 아무래도 리액트를 공부하는 입장에서 styled components를 사용해보는 것이 좋을 것 같다.

### :fire: 마무리
Styled Components에 대해 알아보았는데, 정말 매력적인 라이브러리인 것 같다. JavaScript가 성장하면서 또 어떤것들이 생겨날 지 기대가 되면서도 배울게 많아질 거 같은 걱정..이 들지만 재미가 있어서 다행이다..!