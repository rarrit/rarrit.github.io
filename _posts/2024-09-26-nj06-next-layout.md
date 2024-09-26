---
title: "[Next.js] 공통 layout 적용하는 방법"
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

## :ledger: Next.js에서 공통 레이아웃(헤더,푸터 등)을 설정하는 방법을 알아보자! 
Next.js에서 layout 파일은 `segment`와 그의 자식 노드에 있는 요소들이 공통적으로 적용 받게 할 UI를 정의하며 자식 노드에 있는 요소들이 공통 적용을 받아야 하기 때문에 반드시 `children prop`이 존재해야 함을 기억해야함!

### :one: layout 파일 특징
동일 `layout`안에서 다른 경로를 계속해서 왔다 갔다 할 때 `re-rendering`이 일어나지 않는다. 따라서 유저가 경로를 마음껏 탐색하고 다녀도 공통 레이아웃(헤더,푸터,사이드바 등)은 바뀔 필요가 없으므로 유용하게 사용할 수 있음.

```plaintext
app/
├── layout.tsx     # 공통 레이아웃 파일
├── page.tsx       # 기본 페이지
└── posts/
    └── page.tsx   # post 페이지
```

### :two: layout 기본 구조
- `layout.tsx` 파일을 생성하고 아래와 같이 children을 포함시켜 공통 UI를 만들어준다.
- Next.js 프로젝트를 생성하면 root 경로에 기본적으로 layout.tsx 파일이 존재함

```tsx
// app/layout.tsx
export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body
        className={`${geistSans.variable} ${geistMono.variable} antialiased`}
      >
        <header>헤더임다</header>
        {children}
        <header>푸터임다</header>
      </body>
    </html>
  );
}
```

### :three: 특정 컴포넌트 공통 레이아웃 생성 (중첩 레이아웃)
- Next.js는 중첩 레이아웃을 지원하며, 특정 페이지 그룹에 대해 별도의 레이아웃을 설정할 수 있다.
- 방법은 아래와 같이 특정 디렉토리내에 layout.tsx를 생성하면 된다.

```plaintext
app/
├── layout.tsx        # 전체 레이아웃
├── post/
│   ├── layout.tsx    # post 레이아웃 - post 하위 모든 페이지에 공통으로 사용됨
│   └── page.tsx      # post 페이지
└── page.tsx          # 기본 페이지
```

### :fire: 마무리
Next.js에서 공통 레이아웃을 쉽게 설정할 수 있으며, 중첩 레이아웃을 활용하여 세분화된 관리가 가능하다. 