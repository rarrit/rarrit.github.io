---
title: "React + Supabase 시작하기 (1), 설치 및 세팅 방법"
date: 2024-08-30
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - supabase
  - til
tags:
  - supabase
author_profile: true
sidebar_main: true
---

## :ledger: React + Supabase 시작하기

리액트에서 `Supabase` 설치 및 세팅 방법입니다.

### :one: Supabase 가입

1. 공식 홈에서 회원가입을 진행한다. [https://supabase.com/](https://supabase.com/)
2. 로그인 후 `Start your project`를 클릭하여 프로젝트를 생성한다.
3. 생성한 프로젝트를 클릭하여 좌측 메뉴 `Home`에서 스크롤을 내리면 `Project Url`과 `API Key`를 확인한다.
4. url, key값을 확인하면 react를 세팅한다.

![supabase 가입 이미지1](https://github.com/user-attachments/assets/9f709258-ced9-4489-86c9-ef8440f598a3)

### :one: React 설치 및 세팅

본인은 vite로 생성했으며, 프로젝트 생성 후 `supabase`를 추가하고 url,key값을 연결한다.

### :pushpin: 1-1) 프로젝트 설정

1. vite 생성: `yarn create vite`
2. router 추가: `yarn add react-router-dom`

#### :pushpin: 1-2) supabase 터미널 세팅 방법

아래의 명령어를 터미널에 입력한다.

```bash
yarn add @supabase/supabase-js
npm install @supabase/supabase-js
```

#### :pushpin: 1-3) supabaseClient.js 생성

src 폴더에 `supabaseClient.js`를 생성하여, url,key값을 연결해준다.

```jsx
import { createClient } from "@supabase/supabase-js";

const supabaseUrl = "url 값";
const supabaseKey = "key 값";

export const supabase = createClient(supabaseUrl, supabaseKey);
```

### :two: 폴더 구조

- `components`: 컴포넌트를 관리할 폴더
- `context`: context API 관리 폴더
  - UserContext.jsx
  - 이 프로젝트 전체에서 사용할 유저 전역 상태를 지정해줌
- `pages`: 라우팅에서 페이지로 처리할 폴더
- `shared`: 라우팅 파일이 들어갈 폴더
- `supabaseClient.js`: 수파베이스 처리할 파일

### :three: 페이지 구조

페이지를 구분할 때 로그인 여부에 따라 확인이 필요함

1. `Main` : 메인 페이지 (로그인 상관 없음)
2. `signin`: 로그인 페이지 (비로그인만 가능)
3. `signup`: 회원가입 페이지 (비로그인만 가능)
4. `mypage`: 마이페이지 (로그인만 가능)
5. `write`: 글 작성 페이지 (로그인만 가능)

### :fire: 마무리

프로젝트 세팅은 비교적 쉬웠다. 로그인,회원가입도 영상을 보며 따라할 때 쉬워보였는데, 초기 세팅해야할 부분이 있어서 다음 글에서 추가적으로 작성해볼 예정이다.
