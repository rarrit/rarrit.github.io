---
title: "React 포켓몬 도감 만들기(리팩토링) 회고"
date: 2024-08-26
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
  - mini
tags:
  - mini project
  - prop drilling
  - context api
  - redux toolkit
  - refactoring
author_profile: true
sidebar_main: true
---

## :ledger: REACT POKEMON DEX PROJECT

## :rocket: 베포 링크
- vercel
  - [https://pokemon-dex-rarrit.vercel.app/](https://pokemon-dex-rarrit.vercel.app/)
- github
  - [https://github.com/rarrit/react-pokemon-dex](https://github.com/rarrit/react-pokemon-dex)  

## :one: 프로젝트 설명
- ***프로젝트 기간***
  - 2024.08.16 ~ 2024.08.27
- ***프로젝트 명*** 
  - Pokemon Dex (포켓몬 도감) 만들기
- ***프로젝트 목표***
  - 단계별 리팩토링 (깃 브랜치, 아래 순서로 진행)
    - prop drilling
    - context API
    - RTK (Redux Toolkit)
- ***구현 기능***
  - 도감 페이지로 이동되는 랜딩 페이지 생성
  - 상세 페이지 
    - 대시보드: 선택한 6개의 포켓몬을 표시,선택,해제 가능 
    - 리스트: 포켓몬 리스트 노출, 각 포켓몬을 클릭하면 대시보드에 추가
    - 포켓몬 카드: 각 포켓몬의 이미지,이름,타입 표시 및 추가,삭제 기능
  - 디테일 페이지 
    - 디테일 페이지에서도 선택,삭제 기능 적용
    - 디테일 페이지내에서 이전,다음 포켓몬 이동 기능 적용
    - 선택된 리스트 노출


## :two: 사용 기술
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white)
![Context-API](https://img.shields.io/badge/Context--Api-000000?style=for-the-badge&logo=react)
![Redux](https://img.shields.io/badge/redux-%23593d88.svg?style=for-the-badge&logo=redux&logoColor=white)
![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)


## :three: 프로젝트 구조
📦src<br/>
 ┣ 📂assets<br/>
 ┃ ┣ 📂img<br/>
 ┃ ┃ ┣ 📂bg<br/>
 ┃ ┣ 📂js<br/>
 ┃ ┃ ┗ 📜types.js<br/>
 ┃ ┗ 📜react.svg<br/>
 ┣ 📂components<br/>
 ┃ ┣ 📜Dashboard.jsx<br/>
 ┃ ┣ 📜GlobalStyle.jsx<br/>
 ┃ ┣ 📜PokemonCard.jsx<br/>
 ┃ ┗ 📜PokemonList.jsx<br/>
 ┣ 📂feature<br/>
 ┃ ┗ 📂pokemons<br/>
 ┃ ┃ ┗ 📜pokemonsSlice.js<br/>
 ┣ 📂pages<br/>
 ┃ ┣ 📜Detail.jsx<br/>
 ┃ ┣ 📜Dex.jsx<br/>
 ┃ ┗ 📜Home.jsx<br/>
 ┣ 📂shared<br/>
 ┃ ┣ 📜Layout.jsx<br/>
 ┃ ┗ 📜Router.jsx<br/>
 ┣ 📂store<br/>
 ┃ ┗ 📜index.js<br/>
 ┣ 📜App.css<br/>
 ┣ 📜App.jsx<br/>
 ┣ 📜index.css<br/>
 ┣ 📜main.jsx<br/>
 ┗ 📜mock.js

## :four: 주요 기능
### :pushpin: 4-1) Main Page
- 버튼을 클릭하면 페이지 이동 전 애니메이션 작업 후 이동합니다.

![intro](https://github.com/user-attachments/assets/21db6f94-52a8-4dc1-a66f-f7776fc25f9c)

### :pushpin: 4-2) Dex Page
- DashBoard, PokemonList 컴포넌트로 구성된 페이지입니다.
- 리스트에서 포켓몬을 선택하면 대시보드에 저장됩니다.
- 선택,삭제가 가능하며 최대 6마리의 포켓몬을 등록할 수 있습니다.
  - 선택,삭제,최대6마리 적용 시 우측 하단에 알림 문구가 노출됩니다.

![add-remove](https://github.com/user-attachments/assets/222dec78-60fa-462c-8c4d-8ede0167df63)

### :pushpin: 4-3) 상세페이지
- 선택한 포켓몬의 데이터를 보여줍니다.
- 버튼 기능
  - 디테일 내 이전, 다음 포켓몬 이동 
  - 등록, 삭제 
  - 전체 리스트 버튼 클릭 시 좌측 fixed 리스트 노출

![detail](https://github.com/user-attachments/assets/ff3d7afd-9b4f-4aee-b2e9-68c4178fb73c)
<br/>
![detail-action](https://github.com/user-attachments/assets/af39fc7f-4505-4f11-a5aa-eda85a3610fe)


## :five: 실행
- `git clone`: 폴더 복사
- `npm install -g yarn`: yarn 설치
- `yarn dev`: 로컬에서 프로젝트 실행

## :fire: 회고
이번 과제를 진행하며, 리액트에 조금 더 친숙해진 것 같았다. 이전에는 prop-drilling 으로만 작업했었는데, 이번 과제를 통해 context API로 리팩토링을 해보며 왜 사용하는지 직접 편리함을 느끼게 되었고 RTK로 리팩토링하며 context API 와 RTK의 차이점에 대해 이해할 수 있었다.
- `prop-drilling`
  - props를 계속 생성해 하위 컴포넌트에 전달하다 보니, props가 많아지면 가독성이 떨어졌고, 네이밍이 적절하지 않으면 상태나 함수명과 혼동되기 쉬웠다. 이로 인해 불편한 점이 많았다.
- `Context API`
  - Context API를 사용하면서 무분별한 props 사용이 사라졌고, 필요한 props와 이벤트를 직접 가져와 사용하다 보니 하위 컴포넌트로 props를 전달하는 불필요한 작업이 줄어들었다. 덕분에 코드의 가독성도 훨씬 좋아졌다.
- `RTK`
  - 처음에는 다소 어렵게 느껴졌지만, 실제로 적용해보면서 RTK의 강력함을 배울 수 있었다. store를 생성해 중앙에서 상태를 관리하고, dispatch와 action을 통해 Reducer로 상태를 업데이트하는 구조가 매우 흥미로웠다. 이 과정을 통해 리액트에서의 상태 관리가 얼마나 체계적이고 효율적일 수 있는지 깨닫게 되었다.
- `Context API 와 RTK 차이`
  - Context API는 주로 전역에서 상태를 쉽게 전달하는 용도로 사용되며, 데이터 컴포넌트 트리에서 전달하는 것에 중점을 둔다. <u>상태가 변경되면 `context`를 구독하고 있는 모든 컴포넌트가 재랜더링이 된다.</u>
  - RTK 는 상태가 변경될 때, 그 상태를 실제로 구독하고 있는 컴포넌트만 재렌더링이 된다. 성능적으로도 효율적이지만 작은 규모의 프로젝트에선 Context API가 효율적일수도..?

### :pushpin: 5-1) 어려웠던 점
개인적으로 Context API까지는 재밌게 진행했으나, RTK로 리팩토링하는 과정에서 이해해야 하는 부분이 정말 많았다. flux패턴이 도움이 많이 되었지만 이해하는것과 사용해보는 것은 또 다른 느낌이었으니.. 많이 사용해보며 익숙해지도록 노력했다.

### :pushpin: 5-2) 알게된 점
리팩토링 과정을 진행하며 리액트 문법에 익숙해지기도 했고, props-drilling 으로 프로젝트를 진행하면 불편함이 뭔지 왜 context API, Redux, RTK가 생겨났는지 이해할 수 있는 좋은 계기가 된 것 같다. 