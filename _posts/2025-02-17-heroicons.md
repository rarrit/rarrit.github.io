---
title: "[React] 리액트 아이콘 라이브러리 HeroIcons 사용법"
date: 2025-02-17
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
  - lib
tags:
  - icon
  - heroicons
author_profile: true
sidebar_main: true
---

## :ledger: [React] 리액트 아이콘 라이브러리 HeroIcons 사용법

이번 프로젝트에서 `React`를 사용해 관리자 페이지와 프론트엔드를 작업하게 되었다. 관리자의 경우 템플릿을 기반으로 기본적인 코드가 세팅되어 있었고, 전반적으로 `Tailwind CSS`를 사용했다. 아이콘은 `heroicons` 라이브러리를 활용했는데, 이 라이브러리는 `React`와 `Vue`에서 사용 가능한 아이콘 라이브러리로 이번 글에서는 `Heroicons`의 설치 방법과 사용법을 정리해봤다.

### :one: Heroicons 설치

프로젝트에 기본적으로 설정되어 있었지만, 직접 설치해야 할 경우 방법은 아래와 같다.

```bash
// npm
$npm install @heroicons/react

// yarn
$yarn add @heroicons/react
```

### :two: heroicons 사용법

`heroicons`를 구글에 검색해보면 사이트가 두 가지가 있는데, [heroicons.dev](https://heroicons.dev/)에서 아이콘을 확인하고 복사할 수 있다.

![heroicons-image](https://github.com/user-attachments/assets/7faeba2d-e4ba-4a73-8d49-82437eaa6da8)

1. **좌축 VERSION**: 버전 1,2을 선택하고 `outline`, `solid` 스타일을 적용할 수 있다.
2. **좌측 LINKS**: `GitHub`, `Figma`, `Twitter` 등의 관련 링크를 확인할 수 있다.
3. **우측 영역**: 아이콘 크기 조절이 가능하며, `COPY AS` 기능을 이용해 원하는 형태의 코드를 복사할 수 있다.

### :three: 사용 화면 코드

현재 프로젝트에서는 라우터에서 페이지에 따라 아이콘이 동적으로 적용되게 설정했다.

```javascript
import {
  HomeIcon,
  UserCircleIcon,
  TableCellsIcon,
  InformationCircleIcon,
  ServerStackIcon,
  RectangleStackIcon,
} from "@heroicons/react/24/solid";
import { Home, Profile, Tables, Notifications } from "@/pages/dashboard";

const icon = {
  className: "w-5 h-5 text-inherit",
};

export const routes = [
  {
    layout: "dashboard",
    pages: [
      {
        icon: <HomeIcon {...icon} />,
        name: "dashboard",
        path: "/home",
        element: <Home />,
      },
      {
        icon: <UserCircleIcon {...icon} />,
        name: "profile",
        path: "/profile",
        element: <Profile />,
      },
      {
        icon: <TableCellsIcon {...icon} />,
        name: "tables",
        path: "/tables",
        element: <Tables />,
      },
      {
        icon: <InformationCircleIcon {...icon} />,
        name: "notifications",
        path: "/notifications",
        element: <Notifications />,
      },
    ],
  },
];

export default routes;
```

### :fire: 마무리

이전에 [Lucide Icons](https://rarrit.github.io/lib/react/til/lucide-icons/#google_vignette)를 사용했는데, 사용법이 `HeroIcons`와 비슷한 것 같다. 프로젝트에 따라 적절한 아이콘 라이브러리를 선택하면 될 것 같다.
