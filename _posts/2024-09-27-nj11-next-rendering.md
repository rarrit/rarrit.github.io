---
title: "[Next.js] MPA, SPA와 4가지 주요 렌더링 기법(CSR,SSG,ISR,SSR) 알아보기"
date: 2024-09-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - development
tags:
  - next js
  - next component
author_profile: true
sidebar_main: true
---

## :ledger: MPA, SPA와 4가지 주요 렌더링 기법(CSR,SSG,ISR,SSR) 알아보기
MPA, SPA에 대해 알아보고 Next.js의 렌더링 기법이 무엇이 있는지 알아보자!

### :one: MPA (Multi Page Application)
- 전통적인 서버 사이드 렌더링 방식인 [MPA](https://rarrit.github.io/development/til/til-mpa/)로부터 프론트엔드 웹 개발이 시작됨
- 페이지 이동 시 및 렌더링 시 깜빡거리는 현상이 있고 컨텐츠 양에 따라 페이지별 편차가 심해져 결국 `UX가 저하`됨
- 이러한 문제 때문에 React, Angular, Vue 등 <u>[SPA(Single Page Application)](https://rarrit.github.io/development/til/til-spa/)가 등장함</u>
- **정리 글**
  - [[MPA] 멀티 페이지 애플리케이션이란? - RARRIT NOTE](https://rarrit.github.io/development/til/til-mpa/)

### :two: SPA (Single Page Application)
- 브라우저에서 동작하는 `Javascript`를 이용해 동적으로 페이지, 컴포넌트 등을 렌더링 하는 방식
- [CSR](https://rarrit.github.io/development/til/til-csr/) 이라는 개념은 기존 프론트엔드 개발자들에게 획기적 방법으로 소개
  - 최초 서버로부터 텅 빈, `root`라는 `id`를 `div`만 다운로드함(JavaScript Bundle을 통해 UI가 완성)
  - 더 이상 새로고침이나 깜빡거림 없이 웹서비스 이용이 가능하며 UX가 크게 `향상`
- **정리 글**
  - [[SPA] 싱글 페이지 애플리케이션이란? - RARRIT NOTE](https://rarrit.github.io/development/til/til-spa/)
  
#### :pushpin: 2-1) 단점은 무엇이 있을까
1. 늦는 초기 로딩 속도
  - 텅 빈 `div`만 불러오기 때문에 JavaScript의 평가, 실행이 될 때까지 하얀 화면이 유저에게 노출됨
  - 이를 보완하기 위해 `Code Spilitting` (Lazy-Loading) 방법 제시
  - 하나로 `bundle`된 코드를 여러 코드로 나눠 당장 필요한 코드가 아니면 나중에 불러옴
  - 태생적으로 완벽하게 해결되지는 못함

### :three: 4가지 주요 렌더링 기법

#### :pushpin: 4-1) CSR (Client Side Rendering)
- **특징**
  - 렌더링의 주체: `클라이언트`
  - 브라우저에서 JavaScript를 이용해 동적으로 페이지를 렌더링하는 방식
- **장점**
  - 최초 로드가 끝나면 사용자와의 상호작용이 빠르고 부드럽다. (페이지 이동시 깜빡X)
  - 서버에게 추가적인 요청을 보낼 필요가 없기 때문에, 사용자 경험이 좋음
  - 서버 부하가 적음
- **단점**
  - 첫 페이지 로딩 시간(Time To View)이 길 수 있음
  - JavaScript가 로딩 되고 실행될 때까지 페이지가 비어있어 검색 엔진 최적화(SEO 불리함)
- **정리 글**
  - [[CSR] 클라이언트사이드 랜더링이란? - RARRIT NOTE](https://rarrit.github.io/development/til/til-csr/)

```jsx
// React
import React from 'react';
import ReactDOM from 'react-dom';

function App() {
  return <h1>Hello, Client Side Rendering!</h1>;
}

// index.js
ReactDOM.render(<App />, document.getElementById('root'));
```

#### :pushpin: 4-2) SSG (Static Site Generation)
- **특징**
  - <u>서버에서 페이지를 렌더링</u> 하여 클라이언트에게 HTML을 전달하는 방식
  - `최초 빌드 시에만 생성이 됨`
- **동작방식**
  - 사전에 미리 정적 페이지를 여러개 만들어 놓음 -> 클라이언트가 홈페이지 요청을 하면, 서버에서 이미 만들어져 있는 사이트를 바로 제공 -> 클라이언트는 표시만함
- **장점**
  - `첫 페이지 로딩 시간이 매우 짧아(TTV)` 사용자가 <u>빠르게 페이지를 볼 수 있습니다.</u> 또한, **SEO**에 유리합니다.
  - CDN(Content Delivery Network) 캐싱 가능
- **단점**
  - 정적인 데이터에서만 사용이 가능함
  - 사용자와의 상호작용이 서버와의 통신에 의존하므로, 클라이언트 사이드 렌더링보다 상호작용이 느릴 수 있으며 서버 부하가 클 수 있음
  - 마이페이지처럼 데이터에 의존하여 화면을 그려주는 경우 사용 불가

#### :pushpin: 4-3) ISR (Incremental Static Regeneration)
- **특징**
  - <u>서버에서 페이지를 렌더링</u> 하여 클라이언트에게 HTML을 전달하는 방식
  - 설정한 주기만큼 페이지를 계속 생성해 줌
    - 설정한 주기가 10분이라면, 10분마다 데이터베이스 또는 외부 영향 때문에 변경된 사항을 반영하는 역할
  - 정적 페이지를 먼저 보여주고, 필요에 따라 서버에서 페이지를 재생성하는 방식임
- **장점**
  - 정적 페이지를 먼저 제공하므로 사용자 경험이 좋으며, 콘텐츠가 변경되었을 때 서버에서 페이지를 재생성하므로 최신 상태를 (그나마) 유지할 수 있음
  - CDN(Content Delivery Network) 캐싱 가능
- **단점**
  - 동적인 콘텐츠를 다루기에 한계가 있음 (실시간 페이지는 아님)
  - 마이페이지 처럼 데이터에 의존하여 화면을 그려주는 경우 사용 불가함

#### :pushpin: 4-4) SSR (Server Side Rendering)
- **특징**
  - SSG, ISR처럼 렌더링 주체가 서버임
  - 클라이언트의 요청 시 렌더링함
    - C -> S : 페이지줘
    - S -> C : (데이터베이스 읽고 등등 후) html 파일 제공
- **장점**
  - 빠른 로딩 속도(TTV)와 높은 보안성을 제공
  - SEO 최적화 좋음
  - 실시간 데이터를 사용
  - 마이페이지 처럼 데이터에 의존한 페이지 구성 가능
  - CDN 캐싱 불가
- **단점**
  - 사이트의 콘텐츠가 변경되면 전체 사이트를 다시 빌드해야 하는데, 이 과정이 시간이 오래 걸릴 수 있음. → ***서버 과부하***
  - 요청할 때 마다 페이지를 만들어야 함
- **정리 글**
  - [[SSR] 서버사이드 랜더링이란? - RARRIT NOTE](https://rarrit.github.io/development/til/til-ssr/)

#### :pushpin: 4-5) 4가지 렌더링 기법 비교 표

| **구분**               | **CSR (Client Side Rendering)**                                    | **SSG (Static Site Generation)**                                  | **ISR (Incremental Static Regeneration)**                        | **SSR (Server Side Rendering)**                                 |
|------------------------|-------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|----------------------------------------------------------------|
| **렌더링 주체**         | 클라이언트                                                       | 서버 (사전 빌드 시)                                               | 서버 (사전 빌드 + 주기적 갱신)                                    | 서버 (클라이언트 요청 시)                                       |
| **로딩 속도**           | 최초 로딩 속도 느림, 이후 빠름                                     | 최초 로딩 매우 빠름                                               | 최초 로딩 매우 빠름, 주기적 업데이트                               | 요청마다 렌더링, 빠름                                            |
| **SEO**                | 불리 (자바스크립트 실행 후 콘텐츠 표시)                             | 유리 (정적 HTML 제공)                                              | 유리 (정적 HTML 제공)                                              | 매우 유리 (즉시 HTML 제공)                                       |
| **동적 콘텐츠**         | 지원 (클라이언트에서 API 호출로 동적 콘텐츠 생성)                     | 미지원 (정적 콘텐츠만 가능)                                         | 일부 지원 (주기적 업데이트로 동적 콘텐츠 반영 가능)                   | 지원 (실시간으로 동적 콘텐츠 렌더링 가능)                         |
| **사용 사례**           | 대부분의 SPA 애플리케이션                                          | 블로그, 문서 사이트, 마케팅 페이지                                 | 업데이트 주기가 긴 콘텐츠가 있는 사이트                             | 실시간 데이터 제공이 중요한 웹사이트 (마이페이지, 대시보드 등)    |
| **CDN 캐싱**            | 가능                                                              | 가능                                                              | 가능                                                              | 불가                                                               |
| **서버 부하**           | 낮음                                                              | 매우 낮음                                                          | 낮음                                                              | 높음                                                               |


### :fire: 마무리
MPA와 SPA의 차이를 알아보고 Next.js에서 제공하는 4가지 주요 렌더링 기법(CSR, SSG, ISR, SSR)에 대해 알아보았다. 각 기법은 사용자의 요구사항과 웹 애플리케이션의 특성에 따라 적합한 방식이 달라지며, 상황에 맞게 적절한 렌더링 기법을 선택하는 것이 중요할 것 같다.