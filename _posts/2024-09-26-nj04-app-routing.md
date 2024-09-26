---
title: "[Next.js] Next.js 라우팅에 대해 알아보자"
date: 2024-09-26
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
author_profile: true
sidebar_main: true
---

## :ledger: Next.js 라우팅 개념과 용어를 알아보자
Next.js 라우팅의 핵심인 파일 기반 라우팅의 개념과 예시 및 용어들을 알아보자

## :one: 파일(폴더) 기반 라우팅

#### :pushpin: 1-1) Tree 관련 용어

![img-next-tree](https://github.com/user-attachments/assets/fbfe2d9a-090c-46ea-98ac-59ca9364958d)

1. Tree
  - 계층 구조를 시각적으로 잘 보기 위한 규칙을 이야기함 (위 -> 아래)
  - Dom tree도 같은 구조
2. Subtree
  - tree의 한 부분
  - root부터 시작해서 leaf 들에 이르기까지의 범위
3. Root
  - Tree 또는 Subtree의 첫 번째 노드
  - root layout 처럼 가장 첫 번째에 있는 노드, 요소
4. Leaf
  - children이 더 이상 없는 끝 Node를 뜻함 (src > app > 여기임)

#### :pushpin: 1-2) URL 관련 용어

![img-next-routing-url](https://github.com/user-attachments/assets/e714ba46-bdf6-4595-90c9-23c390281276)

1. URL Segment
  - 슬래시(/)로 분류된 URL path의 한 부분을 뜻함
2. URL path, pathname
  - 도메인 이후 따라오는 URL 부분을 뜻함 (www.rarrit.com/여기임)

### :two: 라우팅 설정 방법

#### :pushpin: 2-1) 파일 시스템 기반 라우팅 (File System Based Routing)
1. **설명**
  - Next.js의 기본 라우팅 방식으로, 디렉토리와 파일 구조를 기반으로 경로가 자동으로 생성되며, 각 폴더와 파일이 URL 경로와 직접적으로 연결되며, 폴더를 생성하고 그 안에 page.tsx 파일을 추가하면 라우트가 만들어진다.
2. **특징**
  - 모든 경로가 정적이며, URL 경로는 디렉토리 구조에 맞춰 미리 정의된 구조로 나타남.
3. **예시**
  - `/about → app/about/page.tsx`
  - `/posts → app/posts/page.tsx`

```bash
app/
├── about/
│   └── page.tsx  # /about 페이지
└── post/
    └── page.tsx # /post 페이지        
```

#### :pushpin: 2-2) 정적 라우팅 (Static Routing)
1. **설명**
  - Next.js에서 미리 정의된 경로에 대응되는 라우팅 방식으로 경로가 고정되어 있으며, 해당 경로로의 접근이 항상 같은 파일로 매핑된다.
2. **특징** 
  - 경로가 미리 고정되어 있고, 변하지 않는 정적 페이지에 대한 라우팅이다.
3. **예시** 
  - about 폴더에 있는 page.tsx 파일은 항상 /about 경로에 대응되며, URL 파라미터나 동적 경로는 포함되지 않습니다.

```bash
app/
└── about/
    └── page.tsx  # /about 경로에 고정된 페이지
```

#### :pushpin: 2-3) 동적 라우팅 (Dynamic Routing)
1. **설명**
  - 경로의 특정 부분을 변수로 설정하여 다양한 값을 URL에 포함시킬 수 있는 라우팅 방식으로 폴더 이름이 대괄호([])를 사용해서 변수화한다.
2. **특징**
  - 경로의 특정 부분을 변수로 설정하여 다양한 값을 처리할 수 있다.
3. **예시**
  - /posts/1, /posts/2처럼 id 값이 변하는 경로에서 해당 값을 변수로 받아 처리할 수 있음.

```bash
app/
└── about/
    └── [aboutId]/
        └── page.tsx  # /posts/[id] 경로로 동적 라우팅
```

#### :pushpin: 2-4) 중첩 라우팅 (Nested Routing)
1. **설명**
  - 특정 경로 안에 여러 하위 경로를 중첩시켜 라우팅을 처리함
2. **특징**
  - 중첩 라우팅은 부모 페이지와 자식 페이지의 관계를 표현해서 더 복잡한 URL 구조를 처리할 수 있음
  - 중첩된 경로는 부모 폴더 경로에 포함되며 관련 페이지들이 계층 구조를 이룸
3. **예시** 
  - 아래의 파일 구조는 `/posts/list`와 같은 중첩된 경로를 처리할 수 있음

```bash
app/
└── posts/
    ├── page.tsx  # /posts 페이지
    └── list/
        └── page.tsx  # /posts/list 페이지
```

### :fire: 마무리
Next.js의 라우팅의 개념과 용어들을 알아보았는데, React 라우팅 보다 정~말 편한 것 같다. 다양한 라우팅 방법을 지원하므로 프로젝트 구조와 요구 사항에 맞춰 효율적으로 사용해보도록 하자!