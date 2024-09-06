---
title: "Spoon overflow 팀 프로젝트 회고"
date: 2024-09-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - supabase
  - til
  - mini
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: REACT SPOON OVERFLOW PROJECT

## :rocket: 베포 링크

- vercel
  - [https://spoon-overflow.vercel.app/](https://spoon-overflow.vercel.app/)
- github
  - [https://github.com/rarrit/sparta-overflow](https://github.com/rarrit/sparta-overflow)

## :one: 프로젝트 설명

개발자를 위한 Q&A 형식의 온라인 커뮤니티입니다. 이 웹 사이트는 개발자들이 코딩 관련 문제를 해결하기 위해 질문을 하고, 다른 **사용자들이 그 질문에 답변을 제공**하는 플랫폼입니다. Spoon Overflow를 통해 프로그래밍 언어, 알고리즘, 트러블 슈팅 등 다양한 주제에 대한 질문과 답변이 활발하게 이루어지는 것이 목표입니다.

## :two: 사용 기술

![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![Context-API](https://img.shields.io/badge/Context--Api-000000?style=for-the-badge&logo=react) ![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white) ![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white) ![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-%23000000.svg?style=for-the-badge&logo=notion&logoColor=white)

## :three: 프로젝트 담당 역할

- vite, supabase 초기 세팅
- 게시글 추가
- 게시글 수정
- 헤더,푸터 레이아웃
- 로그인 여부 판별하여 노출,미노출

## :four: 주요 기능

### :pushpin: 4-1) 메인 페이지

![spoon-mainpage1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/4731bab3-750c-4556-9737-e21151bef302)

- `Post 리스트`
  - 전체 리스트 노출
  - 답변 전,후 탭 클릭 시 해당 데이터 리스트 노출
  - 더보기 클릭 시 현재 화면의 Post 갯수에서 +9값을 불러옴
- `글쓰기 (로그인일 경우)`
  - 비회원은 해당 버튼 미노출
  - 우측 하단의 글쓰기 버튼을 클릭 시 Post등록 페이지로 이동

### :pushpin: 4-2) 로그인, 로그아웃, 회원가입 페이지

![spoon-logut-login1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/cdc4e8d0-c50e-460e-a8e8-76e08d22a1cf)

- `회원가입`
  - profile img, id(email), password, name 입력 후 회원가입 버튼을 클릭 시 supabase에 회원 데이터가 저장된다.
- `로그인`
  - email, password 입력 후 로그인 버튼 클릭 시 supabase의 회원 데이터 값과 비교하여 일치할 경우 로그인한다.
- `로그아웃`
  - 로그인 하면 login -> logout으로 변경 후 클릭 시 로그아웃이 된다.

### :pushpin: 4-3) 상세 페이지

- 작성한 유저의 이름, 프로필 이미지, 타이틀, 상세 내용, 코드 블럭 내용 노출
- 작성한 유저만 수정,삭제 버튼 노출

### :pushpin: 4-4) 댓글, 대댓글, 채택

![spoon-comment1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/e49c87e6-381f-4f6c-86d7-065da4693ca7)

- `댓글`
  - 로그인 유저만 작성 가능
    - 작성자 본인만 수정,삭제 가능
- `대댓글`
  - 댓글에 다른 유저가 댓글 작성 가능
- `채택`
  - 글 등록한 회원이 댓글에 채택하기 버튼을 클릭하여 채택된 답변으로 변경
  - 채택된 답변이 생길 경우 해당 글은 답변이 완료된 리스트로 변경
  - 메인, 마이페이지에서 변경된 값으로 노출

### :pushpin: 4-4) 등록 페이지

![spoon-regi1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/82e2f0a6-01f5-453a-bffa-517316acacb8)

- 로그인한 유저만 등록 가능
- 상세,수정,등록 페이지 제외 모든 페이지 우측 하단에 글쓰기 버튼 노출
- 게시글 타이틀, 상세 내용, 코드 블럭 내용 작성 후 등록 버튼 클릭 시 리스트에 노출

### :pushpin: 4-5) 수정 페이지

![spoon-modi1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/f6ecde49-7bfc-4ac5-8950-c960109bcc1d)

- 로그인한 회원의 글인 경우 수정 가능
- 타이틀,상세 내용,코드 블럭 작성 후 수정 버튼 클릭시 변경된 내용으로 업데이트

### :pushpin: 4-6) 마이 페이지

![spoon-mypage1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/8f3e6cd1-b36f-4835-8d7d-aa30de93a624)

- 로그인한 유저가 작성한 게시글과 댓글을 작성한 게시글 리스트 확인가능
- 게시글,댓글 탭 클릭 시 해당 리스트 노출

### :pushpin: 4-7) 검색 페이지

![spoon-search1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/ffac194e-d095-468b-aac8-80a833d06ef5)

- 메인페이지에서 상단의 검색 바에 값을 입력하면 해당 리스트가 노출됨

## :fire: 회고

## :pushpin: Keep - 현재 만족하고 있는 부분

- 이번 팀 프로젝트를 진행할 때 모두 적극적으로 의견을 내주어서 가장 시간이 오래걸리는 프로젝트 초기 단계에서 어려움이 없었다.
- 문제가 발생할 경우 화면을 공유하여 같이 고민하고 문제를 해결해나가는 과정이 좋았다.

## :pushpin: Problem - 불편하게 느끼는 부분

- 맡은 페이지가 게시물 등록,수정 페이지였는데, 회원 정보와 게시물 정보만 전달받으면 어려움이 없을거라 생각했고 그러한 생각이 DB작업 및 리액트에서 데이터 처리하는 같은 팀원에 부담을 많이 줬을 것 같다. (예: 로그인,회원가입 DB 설정하는 과정을 몰라가지고 문제가 생겼을 때 도움과 피드백을 제공하지 못함)

## :pushpin: Try - problem에 대한 해결책, 당장 실행 가능한 것

- 앞으로 프로젝트를 진행할 때 나의 역할에 한정되지 않고, 팀 전체의 흐름과 다른 팀원들의 작업에도 더 많은 관심을 가져야 함을 깨달았다. 나의 업무가 아니더라도 팀원의 작업과 그 과정에서 발생할 수 있는 문제들을 보다 적극적으로 이해할 수 있고 협력할 수 있도록 노력하겠다.
- s`upabase`를 다시 사용해 보면서 나의 역할이 아니었던 부분을 혼자서 복기해 볼 예정이다.
