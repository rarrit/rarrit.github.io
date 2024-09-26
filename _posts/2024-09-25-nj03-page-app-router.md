---
title: "[Next.js] App Router와 Pages Router 차이"
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
  - routing
  - app router
  - pages router
author_profile: true
sidebar_main: true
---

## :ledger: 두 라우터의 차이를 알고, 어떤 라우터를 선택할지 알아보자.
Next.js의 라우팅에는 크게 두 가지 방식이 존재한다. 그것은 바로 **Pages Router**와 **App Router**이다. 이제 이 두 라우터의 차이점을 알아보고, 어떤 라우터를 선택할지 함께 고민해보자!

## :one: 주요 의사결정의 핵심, 라우팅

일반적으로 Next.js 프로젝트를 시작할 때 가장 먼저 결정해야 하는 사항은 아래 두 가지가 있다. 근데 어떤걸 사용해야하는지 아래의 내용을 확인해보고 생각해보자.

1. **Pages Router를 사용할 것인가?**  
  - Next.js 12 버전까지 사용되던 방식
  - 의견
    - ?? : "Next.js는 이미 오래됐는데, 기존 프로젝트에 도입하기엔 Pages Router가 더 직관적이지 않나요?"
    - ?? : “`Next.js`가 나온지 얼마나 오래됐는데! 이미 존재하는 프로젝트에 투입되면 어쩌려고 그래?”
    - ?? : “더 직관적이야! `app router`에서 새롭게 제시하는 내용이 정말 효용이 있나?? 난 잘 모르겠는데;;”
2. **App Router를 사용할 것인가?**
  - Next.js 13 버전부터 새롭게 도입된 방식
  - 의견
    - ?? : “이제 안정화도 되었는데, 자꾸 이전 버전을 사용하는 것은 마치 `React`에서 `class`형 컴포넌트를 쓰는 것과 같아!”
    - ?? : “추가기능이 얼마나 많은데, 당연히 `최신 버전` 사용해야 하는 것 아님??”
    - ?? : “공식 홈페이지에서도 이제 웬만하면 `app router` 쓰라고 하는데?”

### :pushpin: 1-1) ?? : ㅋㅋㅋ 뭐 써야함?
사실 내가 선택할 수 없을 확률이 크다. 그러므로 둘 다 사용할 수 있도록 이해해야 하는게 중요하다 생각한다.

- **Routing, Rendering, Data Fetching** 등의 핵심 개념만 잘 이해한다면 어느 쪽이든 잘 사용할 수 있다고함 
- 공식 문서에서는 App Router를 권장하므로 새로운 프로젝트에선 `App Router`를 사용하는 것이 바람직함

### :pushpin: 1-2) 두 라우터의 특징 비교

- `비교 표`
| **특성** | **Pages Router** | **App Router** |
| --- | --- | --- |
| **디렉토리 구조** | `pages/` 디렉토리 사용 | `app/` 디렉토리 사용 |
| **라우팅 방식** | 파일 기반 라우팅 (`pages/about.js` -> `/about`) | 폴더 기반 라우팅 (`app/about/page.js` -> `/about`) |
| **동적 라우팅** | `[param]` 문법 (`pages/posts/[id].js`) | `[param]` 폴더 (`app/posts/[id]/page.js`) |
| **레이아웃 관리** | 전역 또는 개별 페이지 설정 | `layout.js` 파일을 통한 폴더별 설정 |
| **데이터 페칭** | `getStaticProps`, `getServerSideProps`, `getStaticPaths` | React 18의 데이터 페칭 메서드 사용 가능 |
| **서버 컴포넌트** | 클라이언트 중심 | 서버 컴포넌트와 클라이언트 컴포넌트 분리 가능 |
| **사용 용이성** | 간단하고 직관적 | 유연하지만 초기 설정이 복잡할 수 있음 |
| **적용 프로젝트** | 기존 프로젝트 | 신규 프로젝트 및 최신 기능 필요 시 |

- `실제 코드 예시`
```bash
# Pages Router 예시
pages/
├── about.js  # /about 페이지
└── posts/
    └── [id].js  # 동적 라우팅

# App Router 예시
app/
├── about/
│   └── page.js  # /about 페이지
└── posts/
    └── [id]/
        └── page.js  # 동적 라우팅
```

### :fire: 마무리
음 내용을 이해하기 까지 어려움이 많은 것 같다. 그런데 중요한 것은 Next.js의 본질을 이해하는 것으로 Pages Router와 App Router 중 어떤 라우팅 방식을 선택하더라도, 라우팅, 렌더링, 데이터 페칭 같은 핵심 개념을 잘 이해하고 있으면 쉽게 적응할 수 있을 것 같다. 