---
title: "TTI, TTV, Hydration이란 무엇인가?"
date: 2024-10-17
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - development
  - til
tags:
  - tti
  - ttv
  - hydration
author_profile: true
sidebar_main: true
---

## :ledger: TTI, TTV, Hydration 개념에 대해 알아보자!
이전 스탠다드반 퀴즈에서 아예 작성도 못해가지고 글을 작성해서 다시 복습하는 시간을 갖기위해 TTI, TTV, Hydration에 대해 알아보았다.

### :one: TTI (Time to Interactive)
`TTI (Time to Interactive)`는 웹 <u>페이지가 완전히 로드되고 사용자가 상호작용할 수 있는 시점을 의미</u>한다. 즉, 페이지가 시각적으로 렌더링될 뿐만 아니라, JavaScript도 실행되어 버튼을 클릭하거나 폼을 제출하는 등의 사용자 액션에 반응할 수 있을 때까지 걸리는 시간을 측정합니다.

#### :pushpin: 1-1) TTI 최적화 방법
1. `코드 스플리팅` 
  - 불필요한 JavaScript 로드를 피하기 위해 중요한 부분만 로딩
2. `Lazy Loading`
  - 이미지, 비디오, 컴포넌트를 필요한 시점에 로드하여 초기 로드 속도 개선
3. `Critical Rendering Path 최적화` 
  - CSS, JS 파일의 크기 및 순서를 최적화하여 렌더링을 빠르게 함

### :two: TTV (Time to View)
`TTV (Time to View)`는 페이지가 처음으로 사용자에게 시각적으로 표시되는 시점을 말한다. TTV는 페이지의 중요한 콘텐츠가 화면에 렌더링되는 순간에 집중하기 때문에, 사용자가 페이지가 로드되고 있다는 것을 빠르게 인식하게 한다.

#### :pushpin: 2-1) TTV 최적화 방법
1. `CSS 최적화` 
  - 불필요한 스타일을 줄이고, CSS 파일을 압축하거나 인라인으로 넣어 로딩 속도를 향상시킴
2. `Preload/Prefetch` 
  - 중요한 리소스를 미리 로드해 초기 화면 렌더링 속도를 높임
3. `서버 측 렌더링 (SSR)` 
  - 페이지를 서버에서 렌더링하여 첫 화면 표시를 빠르게 보여줌


### :three: Hydration
`Hydration`은 서버에서 렌더링된 HTML 페이지에 클라이언트 측에서 JavaScript를 연결해 동적인 상태로 전환하는 과정이다. Hydration은 주로 **서버 사이드 렌더링 (SSR)**과 함께 사용되며, 서버에서 렌더링된 정적인 HTML에 클라이언트 측 로직을 붙여줌으로써, 페이지가 처음에 빠르게 로딩된 후 상호작용이 가능하도록 한다.

#### :pushpin: 3-1) Hydration 최적화 방법
1. `리소스 최소화` 
  - 클라이언트 측에서 로드해야 할 JavaScript 크기를 줄여 Hydration 속도 향상
2. `서버 사이드 렌더링과 클라이언트 측 하이드레이션의 분리` 
  - SSR이 너무 많은 데이터를 렌더링하지 않도록 제한
3. `React에서의 Hydration` 
  - React는 SSR과 클라이언트 렌더링을 결합해 빠르게 인터랙티브한 웹페이지를 만들 수 있음

### :fire: 마무리
위와 같이 TTI, TTV, Hydration을 고려한 웹 개발을 통해 사용자 경험을 극대화할 수 있음