---
title: "[Next.js] Next.js란 무엇인지 알아보자!"
date: 2024-09-25
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
tags:
  - next js
author_profile: true
sidebar_main: true
---

## :ledger: Next.js가 무엇인지 알아보는 시간!

### :one: Next.js 란?

![img-next-js-mim](https://github.com/user-attachments/assets/72fe6170-7ea9-4107-b2c1-1475844d4320)

Next.js는 웹을 위한 <u>React 프레임워크</u>다. 

#### :pushpin: high-quality 웹사이트 제작 가능
Next.js를 사용하면 `High-Quality` 아래의 요구사항을 충족하는 고품질의 웹사이트를 쉽게 만들 수 있다!

1. **성능 (Performance)**:
  - **서버 사이드 렌더링 (SSR)** 및 **정적 사이트 생성 (SSG)** 기능을 제공하여 초기 로드 시간을 단축함
  - 코드 스플리팅 및 이미지 최적화와 같은 기능으로 런타임 성능을 향상시킨다.
2. **SEO (Search Engine Optimization)**:
  - 서버 사이드 렌더링을 통해 웹페이지의 콘텐츠가 초기 로드 시 완전히 렌더링 되므로, 검색 엔진이 콘텐츠를 쉽게 인덱싱할 수 있음.
  - 정적 사이트 생성으로 미리 생성된 HTML 파일을 제공하여 SEO 성능을 극대화한다.
3. **개발자 경험 (Developer Experience)**:
  - **자동 코드 분할 (Automatic Code Splitting)**, **핫 리로딩 (Hot Reloading)**, **타입스크립트 지원** 등 개발자가 생산성을 높일 수 있는 다양한 기능을 지원함.
  - **API 라우트** 기능으로 백엔드와의 통합을 쉽게 하고, **환경 설정이 적은**(configuration-less) 개발 환경을 제공한다.
4. **확장성 (Scalability)**:
  - **서버리스 함수**와 **데이터 페칭** 기능을 활용해 유연한 확장성을 제공한다.
5. **유연성 (Flexibility)**:
  - React와 완전히 호환되며, 원하는 다양한 툴 및 라이브러리와 함께 사용할 수 있다.

### :two: 라이브러리와 프레임워크란?
가장 중요한 것은 어플리케이션의 <u>제어 흐름의 권한</u>을 누가 가지고 있는지가 핵심임 

1. `프레임워크`
  - 개발자가 기능 구현에만 집중할 수 있도록 모든 프로그래밍적 재원을 지원하는 '기술의 조합'
  - 비유를 하자면 설계도로 표현할 수 있다. 이미 기본적인 구조(설계도)가 만들어져 있어 그 틀 안에서 작업을 해야함
  - vue.js, Angular.js 등이 있다.
2. `라이브러리`
  - 공통 기능의 모듈화가 이루어진 프로그램의 집합
  - 비유를 하자면 도구 상자와 같다. 여러가지 도구들을 필요할 때마다 사용할 도구를 조합해서 사용하는 것으로 이해하면 됌
  - React.js, react-router-dom, redux 등이 있다.

### :three: Next.js의 주요 특징

| --- | --- |
| 특징 | 설명 | 
| `Routing` | 레이아웃, 중첩 라우팅, 로딩 상태, 오류 처리 등을 지원하는 서버 구성 요소를 기반으로 구축된 파일 시스템 기반 라우터임 |
| `Rendering` | 클라이언트 및 서버 구성 요소를 사용한 클라이언트 측 및 서버 측 렌더링. Next.js를 사용한 서버에서 정적 및 동적 렌더링으로 더욱 최적화. Edge 및 Node.js 런타임에서 스트리밍. |
| `Data Fetching` | 서버 구성 요소에서 async/await를 사용하여 간소화된 데이터 가져오기와 fetch요청 메모 생성, 데이터 캐싱 및 재검증을 위한 확장된 API가 제공됨 |
| `Styling` | CSS 모듈, Tailwind CSS, CSS-in-JS를 포함한 선호하는 스타일링 방법 지원 |
| `Optimizations` | 이미지, 글꼴, 스크립트를 최적화하여 애플리케이션의 Core Web Vitals와 사용자 경험을 개선함 |
| `TypeScript` | 더 나은 유형 검사와 더 효율적인 컴파일, 그리고 사용자 정의 TypeScript 플러그인과 유형 검사기를 통해 TypeScript에 대한 지원이 개선됨 |

### :four: Next.js의 장점
1. 환경설정이 적은 개발환경이다.
  - 개발에만 집중할 수 있도록 프레임워크로서 역할을 충실히 수행함
  - Next.js는 웹개발에 필요로 하는 <u>거의 모든 기능을 디폴트로 가지고 있음</u>
2. 유용한 기법 제공
  - 렌더링 방식
  - 코드 스플리팅
3. 쉬운 서버 로직 개발
  - `API Route`, `server action`등 지원해서 가벼운 서버 개발이 가능함
4. 매우 쉬운 배포
  - Vercel에서 만든 만큼 배포가 굉장히 쉬움

### :fire: 마무리
Next.js를 사용해보며 아직 많이 배우지는 못했지만 개인적으로 프레임워크에 매력을 많이 경험한 것 같다. 여러가지 기능을 지원해주고 있고 좀 더 배우면 어렵게 느껴질 것 같은데 재밌게 해 볼 예정!