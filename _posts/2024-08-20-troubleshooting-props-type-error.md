---
title: "props TypeError: Cannot destructure property"
date: 2024-08-20
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
tags:
  - props type error
author_profile: true
sidebar_main: true
---

## :ledger: TypeError: Cannot destructure property

상위 컴포넌트에서 데이터를 추가 후 해당 데이터를 props로 하위 컴포넌트에 전달하는 과정에서 겪었던 에러입니다.

### :one: 개요

리스트 컴포넌트에서 하위 컴포넌트로 값을 전달 중 개발자 도구를 끈 상태에선 괜찮은데, 개발자 도구를 확인하면 아래와 같은 에러가 노출되고 디버거로 해당 위치를 알려주었음.

### :two: 코드 설명

#### :pushpin: 2-1) 기존 코드 설명

1. 리스트에서 카드를 클릭하면 해당 카드의 데이터가 있는지 판별하여 대시보드에 추가하는 기능임
2. `addPokemonHandler`를 통해 카드가 6개 이하면 클릭한 카드의 데이터가 추가가 되며 하위 컴포넌트(대시보드)에 전달함
3. 하위 컴포넌트(대시보드)에선 해당 데이터를 전달 받은 후 `map`메서드를 통해 클릭한 카드를 화면에 출력해줌.

```jsx
// 카드 추가 함수
const addPokemonHandler = (pokemon) => {
  setSelectPokemon((prevSelectPokemon) => {
    // 클릭한 포켓몬이 이미 있으면, 상태를 변경하지 않음
    if (prevSelectPokemon.some((list) => list.id === pokemon.id)) {
      return prevSelectPokemon;
    }
    if (prevSelectPokemon.length < 6) {
      return [...prevSelectPokemon, pokemon];
    } else {
      alert("포켓몬이 꽉 차버렸슈. 등록은 불가능하오!");
      return prevSelectPokemon;
    }
  });
};

// props를 통해 전달하는 코드
<Dashboard
  selectPokemon={selectPokemon}
  removePokemon={removePokemonHandler}
/>;
```

### :three: 에러

`TypeError: Cannot destructure property` 번역하면 응~ 속성을 분해할 수 없어! 인데, 디버거에서 친절하게 `selectPokemon` 을 가르켰다. 에러가 노출되기 까지 순서대로 정리해봤음

1. 현재 `Dashboard` 컴포넌트에서 `selectPokemon`와 `removePokemon`을 props으로 전달 받고 있음.
2. `addPokemonHandler` 함수가 실행되면 `selectPokemon`이 typeError을 출력함.

#### :pushpin: 3-1) 에러 캡쳐 화면

![component props error](https://github.com/user-attachments/assets/594f3af2-f6d5-4191-a14b-9eabc2ea1bce)

### :four: 해결 과정

1. 먼저 부모 컴포넌트에서 props를 통해 제대로 값을 전달하는지 콘솔로 확인해봄
2. 처음 랜더링 될 때 `selectPokemon`을 콘솔로 찍어서 state에서 초기화한 [] 배열로 되어있는걸 확인
3. 그리고 props로 전달되는 건 배열(`addPokemonHandler`함수를 통해 `selectPokemon`에 객체를 추가해줌)이 맞는데, 잘 되고있고 뭐가 문제인지 싶었음
4. 쥐피티형한테 물어보니 처음 렌더링될 때 `selectPokemon`이 빈 배열이 아닌 `undefined`, `null`일 수 있는 상황을 고려해보라함

#### :pushpin: 4-1) 쥐피티 힌트 이후

1. 초기 렌더링 시에 `selectPokemon`은 빈 배열로 초기화됨 `const [selectPokemon, setSelectPokemon] = useState([]);` 이 시점에서 undefined, null은 아닌게 확실한 것이 console로 찍어보니 []로 잘 나옴
2. [1] 시점에서 이미 멘탈이 터져버렸다. 시간도 너무 많이 사용했고 튜터님께 직행
3. 같이 코드를 분석 중 개발자 도구에서 `Pause on Caught Exceptions`가 체크가 되어있었는데, 실제로 문제가 되었던 부분은 다른 컴포넌트였던 것. 해당 부분을 해결하니 더 이상 type error 메시지도 볼 수 없었다.
4. 난 분명 `Pause on Caught Exceptions`를 체크한 기억이 없는데.. 왜 체크가 되어있었는지, 결론은 코드에 문제는 크게 없었고 다른 컴포넌트에서 발생한 에러로 `selectPokemon`에서 type Error가 노출되었던 것이여서 정말 힘빠졌다..

### :five: 해결 요약

- 현재 진행하고 있던 프로젝트에서 생성한 함수를 테스트하던 중 디버그와함께 `type error` 노출
- 구글링 & 쥐피티를 통해 해결하는 과정 중 잘못된 점을 모르겠었음
- 튜터님께 가봐서 물어봤는데, 왜 디버그가 찍히는지에 대해 알고있냐고 하셔서 나도 (?) 시전
- 알고보니 다른 컴포넌트에서 문제가 생겼는데 `Pause on Caught Exceptions`가 체크되어있어서 뭔가 연결되어있는 곳과 내가 바라본 컴포넌트에서 무언가 연결된 것이 있기에 에러가 노출된 거 같음
- `Pause on Caught Exceptions` 해제하니 정상적으로 잘 작동됨 (문제되었던 컴포넌트를 해결해도 잘 작동됨)

### :fire: 마무리

리액트로 프로젝트를 만들면서 다양한 에러를 만나는 것 같다. 나름 열심히 해결해보려 콘솔로 전부 찍어보며 테스트했으나 구글과 쥐피티형이 힌트 준 부분은 전부 문제가 없었고 뜬금없이 체크되어있는 `Pause on Caught Exceptions`로 인해 정상적인 코드를 믿지못하고 에러를 찾고있던 시간이 허탈했다.. 사실 처음에 디버그가 왜 뜨지? 리액트는 이런것도 캐치해주는구나 좋다~ 하고 넘어갔던 나의 안일한 생각으로 인해 이렇게 된 것 같은데, 내가 내 코드에 문제가 없어! 라고 자신하지 못하니 생긴 문제인 것 같기도하다.
