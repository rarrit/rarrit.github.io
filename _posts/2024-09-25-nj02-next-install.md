---
title: "[Next.js] 설치 방법 및 파일구조 알아보기"
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
  - next.js
author_profile: true
sidebar_main: true
---

## :ledger: Next.js 설치 방법 및 파일 구조를 알아보자
정~말 쉽고 편해요!

### :one: Next.js 설치 방법
공식 홈페이지 내용 참조한 설치 방법은 아래와 같다.

```bash
npx create-next-app@latest
```

- TypeScript : Y
- ESLint : Y
- Tailwind CSS : Y
- `/src` directory : Y
- **App Router : Y**
- import alias : N

### :two: Next.js 파일 구조
1. src > app
2. 위에서 TypeScript : Y를 했으므로 TypeScript 기반으로 만들어ㅣㅁ
3. 기본 파일로는 layout, page가 있음
  - layout
    - layout.tsx는 React 컴포넌트와 똑같은 모양을 가지고 있음
    - children이 존재함
  - page
    - page.tsx 또한 React 컴포넌트의 형태를 가지고 있음
4. tailwind.config.ts도 선택하면 세팅되어 있음
5. package.json
  - dev: 개발자가 개발하는 중 사용하게 될 방법
  - build: production 레벨로 배포하기 전 필요한 빌드 작업 과정을 실행하는 방법
  - start: 만들어진 build 파일을 이용하여 실행하는 방법


### :fire: 마무리
next.js를 사용하러 가봅시다잉