---
title: "[CSS] React TailWind CSS 구글 폰트 전역 설정 방법"
date: 2025-02-20
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - css
  - react
tags:
  - tailwind
author_profile: true
sidebar_main: true
---

## :ledger: [CSS] React TailWind CSS 구글 폰트 전역 설정 방법

리액트 프로젝트에서 구글 폰트를 사용하여 전역 폰트 설정하는 방법을 정리해봤다.

### :one: 구글 폰트 선택

[google font](https://fonts.google.com/) 사이트에 들어가서 맘에 드는 폰트를 고른다. 이번에 적용했던 폰트는 `Noto Sans KR` 이어서 해당 폰트를 예시로 하겠다.

#### :pushpin: 1-1) Get font 클릭

![google-font-image01](https://github.com/user-attachments/assets/0dfd7f73-ef70-44cf-8b85-d3662318eb64)
원하는 구글 폰트를 클릭하면 우측 상단에 `Get font`가 보일 것이다. 클릭

#### :pushpin: 1-2) Get embed code 클릭

![google-font-image02](https://github.com/user-attachments/assets/1a355437-0903-481a-aa6e-60c0135a5d70)

나의 프로젝트에서는 CDN 방식을 선택해서 `Get embed code`를 선택했다. 이유는 프로젝트 완성 기간이 정말 짧아서, 빨리 작업하고 이후에 로컬에 직접 업로드하고 수정할 예정이었음. 둘의 차이는 아래와 같다.

- `Get embed code (CDN)`
  - CDN 방식은 구글 폰트 서버에서 폰트를 웹으로 불러오는 방식으로 **따로 파일 다운로드**할 필요가 없다
  - 최신 폰트 업데이트를 자동으로 반영한다.
  - 여러 브라우저에서 폰트 최적화를 지원한다.
  - 하지만 외부 서버(구글 폰트)에 의존해서 인터넷 연결이 필요함 (근데 안되는곳이 있으려나)
  - `FOUT (Flash of Unstyled Text)` 현상이 발생할 수 있음 (폰트 로딩전에 기본 폰트가 먼저 보일 수 있음)
- `Download all (로컬 파일 방식)`
  - `ttf` 으로 모든 폰트를 받을 수 있음
  - `woff`, `woff2`등 파일 변환해서 CSS에 직접 선언해야함
  - 인터넷 연결 없이 폰트 적용 가능
  - 외부 서버 요청이 없어서 로딩 속도가 향상됨
  - 최신 폰트 업데이트를 수동으로 관리해야 함

#### :pushpin: 1-3) weight 선택 후 @import

![google-font-image03](https://github.com/user-attachments/assets/87b017b2-7a9b-44ae-b363-3079a6130261)

사용할 폰트 굵기를 선택하고 `@import ~~~샬라샬라` 부분만 복사해둔다.

- 폰트 굵기는 `wght@`뒤에 직접 넣어줄 수 있음
- 예시: `wght@400;500;600&display=swap`

```css
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;600&display=swap");
```

## :two: TailWind 설정

`@import`까지 복사를 완료했으면 아래의 순서대로 작업하면된다.

#### :pushpin: 2-1) tailwind.config.js 설정

`tailwind.config.js`에서 설정하면 아래와 같이 사용할 수 있음

- `className="font-sans"`로도 사용 가능함

```jsx
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      fontFamily: {
        sans: ["Noto Sans KR", "sans-serif"],
      },
    },
  },
  plugins: [],
};
```

#### :pushpin: 2-2) index.css 설정

처음에 `@tailwind base;`아래에 `@import` 코드를 삽입했더니 적용이 않되었고, `@apply font-sans`에 `!important`를 하지않으면 적용되지 않아서 아래와 같이 적용했다. 나중에 `!important`안해도 되는 방법 찾을 예정

```css
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;600&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply font-sans !important;
  }
}
```

#### :pushpin: 2-3) 확인

![google-font-image04](https://github.com/user-attachments/assets/7651b8f8-d648-4092-8b93-0166b0dfa3ef)

개발자 도구를 열어서 `<body>` 태그를 클릭하면 잘 적용된 것을 확인할 수 있다.

### :fire: 마무리

요즘 라이브러리도 이것저것 사용하다보면서 다시 세팅할 때 방법을 까먹는 것 같다. ㅂㄷㅂㄷ
