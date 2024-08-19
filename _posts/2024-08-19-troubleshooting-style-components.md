---
title: "styled components - transient props"
date: 2024-08-19
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
tags:
  - styled components
  - transient props
author_profile: true
sidebar_main: true
---

## :ledger: styled components - transient props

리액트로 개인 프로젝트를 진행하며 사용한 `styled components`에서 겪은 에러입니다.

### :one: 개요

`styled components`를 사용하여 컴포넌트를 생성 후 props를 통해 background의 url에 값을 넣었었더니 콘솔에 에러가 출력되었음.

### :two: 코드 설명,에러,해결 과정

#### :pushpin: 2-1) 기존 코드 설명

1. 상위 컴포넌트에서 `pokemon` 데이터를 props를 사용하여 하위 컴포넌트에 전달
2. 하위 컴포넌트내에서 `styled components`를 생성 => `<ImgBox/>`
3. 상위 컴포넌트에서 전달받은 `pokemon`의 데이터 중 img_url을 스타일 컴포넌트로 생성한 ImgBox에 props로 전달

```jsx
import styled from "styled-components";

const PokemonCard = ({ pokemon, isSelected }) => {
  console.log(pokemon);
  return (
    <>
      <CardItem>
        <ImgBox imgUrl={pokemon.img_url} />
        <p>{pokemon.korean_name}</p>
        <p>{pokemon.description}</p>
        {isSelected ? <Button>삭제</Button> : <Button>추가</Button>}
      </CardItem>
    </>
  );
};

const ImgBox = styled.div`
  background-image: url(${(props) => props.imgUrl});
`;
```

### :three: 에러 설명

에러를 번역기로 돌려보니 `imgUrl`이 DOM을 통해 전송이되어 React 콘솔 오류가 발생 어쩌구저쩌구~ **_접두사 사용을 고려(달러표시)_**할 수 있습니다. 라고 설명을 하고 있는데, `$`를 어디에 넣어야 하는지 몰랐다.

#### :pushpin: 3-1) 그래서 왜 에러가 발생함?

React에서는 div, span 같은 HTML 요소에 대해 src, alt, href 등 표준적인 HTML 속성만을 인식한다.
`styled-components`를 사용할 때, `imgUrl` 같은 prop은 HTML 요소에 대한 표준 속성이 아니므로, 이를 그대로 DOM에 전달하려고 하면 React가 "알 수 없는 prop" 경고를 발생 시키기때문에 에러가 노출된거다. 한마디로 너가 넣은 속성 이상함 삑! 느낌~

#### :pushpin: 3-2) 콘솔에 노출된 에러

![styled components error](https://github.com/user-attachments/assets/4fa62d5e-24eb-4ffb-a44c-f373d62e8462)

#### :pushpin: 3-3) 에러 내용

```bash
styled-components.js?v=400c0a89:1201 styled-components: it looks like an unknown prop "imgUrl" is being sent through to the DOM, which will likely trigger a React console error. If you would like automatic filtering of unknown props, you can opt-into that behavior via <StyleSheetManager shouldForwardProp={...}> (connect an API like @emotion/is-prop-valid) or consider using transient props ($ prefix for automatic filtering.)
```

### :four: 해결 과정

`$` 를 어디에 사용해야하는거지 혼잣말하면서 구글링 하고 돌고 돌아 스타일 컴포넌트의 공식문서까지 갔는데 `styled-components`에서 제공하는 기능 중 하나로, 특정 prop이 DOM에 전달되지 않도록 하는 안전한 방법이라고 한다. 물론 `$`없이 해결하는 방법도 있으나 `$`를 사용한다고 문제가 될 건 없고 가독성도 좋아서 채택! 아래는 해결한 코드이다. `[!]` 을 확인하면됨

```javascript
import styled from "styled-components";

const PokemonCard = ({ pokemon, isSelected }) => {
  console.log(pokemon);
  return (
    <>
      <CardItem>
        {/* [!] imgUrl 을 $imgUrl 로 변경 */}
        <ImgBox $imgUrl={pokemon.img_url} />
        <p>{pokemon.korean_name}</p>
        <p>{pokemon.description}</p>
        {isSelected ? <Button>삭제</Button> : <Button>추가</Button>}
      </CardItem>
    </>
  );
};

export default PokemonCard;

const ImgBox = styled.div`
  background-image: url(${(props) => props.$imgUrl});
`;
```

### :five: 에러 및 해결 요약

- `styled components`에서 DOM요소에 props로 값을 전달할 때 내가 작성한 `imgUrl`이라는 prop은 HTML에서 유효한 속성이 아니여서 에러가 노출되었음.
- 해결 방법은 `imgUrl`앞에 `$`를 넣어주면 되는데, `$`를 넣는 이유는 `styled-components`가 자동으로 해당 prop을 DOM에게 전달하지 않고 스타일링에만 사용하게 해주어서 React에서 오류로 출력된 **_알 수 없는 prop_**경고를 방지하는데 유용함

### :six: Transient Props란?

- `Transient props`는 `styled-components`에서 제공하는 기능으로, 특정 props를 스타일링 목적으로만 사용하고, 이를 DOM 요소에 전달되지 않도록 하는 방식이다.
- `Transient props`는 prop 이름 앞에 `$` 기호를 붙여 사용하며, 이로써 해당 prop이 HTML 요소로 전달되지 않고, 오로지 styled-components 내부에서만 사용됨.

### :fire: 마무리

강의를 들으며, 기본적으로 borderColor나 color등 prop으로 전달할 때는 HTML에서 유효한 속성이여서 문제가 되지 않았다. 하지만 리액트에서 사용되는 props는 내가 이름을 작명해서 작성한 적이 있어 styled components에도 똑같이 내맘대로 네이밍을 하다가 오류가 생겼는데, 이해하는 과정이 조금 어려웠다. 쉽게 정리하면 그냥 리액트가 HTML 속성 중 너가 이름으로 만든 속성은 없어! 였는데.. 그래도 해결했으니 만족..!
