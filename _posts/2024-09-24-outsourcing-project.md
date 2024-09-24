---
title: "아웃소싱 프로젝트 (가을 축제 조회 서비스) 후기"
date: 2024-09-24
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - mini
  - til
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: 가을 축제 조회 서비스 프로젝트
지도 API와 YOUTUBE API를 활용하여 선택 할 수 있는 다양한 주제중에서 무더운 이번 여름에 야외 활동을 하지 못하는 시기를 벗어나 가을이 다가오는 만큼 가을 축제라는 주제를 가지고 프로젝트를 진행하고자 하여 선정하게 되었습니다.

### :rocket: 베포 링크

- vercel
  - [https://festival-recommendations.vercel.app/](https://festival-recommendations.vercel.app/)
  - 
- github
  - [https://github.com/rarrit/festival-recommendations](https://github.com/rarrit/festival-recommendations)

### :one: 프로젝트 설명
카카오 지도 API와 전국 문화 축제 표준 데이터의 API 및 유튜브 API를 활용하여 지도에서 축제들의 위치와 정보를 찾아 볼수 있으며 관련된 영상에도 접근 할 수 있도록 기획하였습니다.

### :two: STACK

![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![Zustand](https://img.shields.io/badge/zustand-black?style=for-the-badge&logoColor=white) ![React Query](https://img.shields.io/badge/-React%20Query-FF4154?style=for-the-badge&logo=react%20query&logoColor=white) ![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens) ![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white)  ![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-%23000000.svg?style=for-the-badge&logo=notion&logoColor=white)

### :three: 프로젝트 수행 절차 및 방법

- 2024.08.29 ~ 2024.09.03

| 구분                     | 기간                        | 활동                                                      | 비고                                            |
| ------------------------ | --------------------------- | --------------------------------------------------------- | ----------------------------------------------- |
| 기획 완료 및 협업준비         | 9/12(목)~9/13(금)            | 기획 준비 및 SA문서 작성, 프로젝트 세팅 시작 및<br/> 완료 구성 |                                                 |
| 집중 기능 개발            | 9/14(토)~9/15(일)               | 카카오 지도 메인, 디테일 페이지 노출 및<br/> 로그인/회원가입 페이지 생성                         |                            |
| 집중 기능 개발            | 9/16(월)~9/18(수)               | API(카카오 지도, 축제 데이터, 유튜브) 활용하여 각 페이지<br/> api기능 구현 완료 및 마이 페이지 구현(저장한 목록만 노출)                           |  |
| 집중 디자인 개발 | 9/18(수)~9/19(목) | 스타일 적용 및 디테일 작업                                |                                  |
| 도전기능 구현 및 트러블 슈팅           | 9/20(금)~9/22(일)               | 반응형 웹 디자인과 북마커 기능 추가 및 트러블 슈팅 분석                   |                                                 |
| 총 개발 기간             | 9/12(목)~9/22(일) 총 11일 |                                                           |


### :four: 프로젝트 세팅 및 기능 설명
#### :pushpin: 4-1) 설치 패키지

1. Json-server
2. Tanstack query
3. zustand
4. Axios
5. Tailwind
6. styled-component
7. react-router-dom
8. the-new-css-reset
9. lucide-react

#### :pushpin: 4-2) 주요 기능 소개

| 요구사항              | 선택                     | 이유                                                                                               |
| --------------------- | ------------------------ | -------------------------------------------------------------------------------------------------- |
| 상태 관리 라이브러리 | zustand, tanstack-query                 | 불필요한 전역 state를 로컬 state로 관리하고, <br/>가벼운 전역 state 관리를 위해 zustand 채택 |
| DB 활용       | json-server            | 로그인/회원가입 기능의 인증/인가 로직이 필요하고, <br/>Restful한 개발을 훈련해 볼 수 있으며,<br/> 실무에서는 거의 REST API를 사용하기 때문에 채택             |
| API                | 카카오,축제,유튜브                    | 키워드에 따른 검색 결과를 데이터로 받기 위해 youtube data api를 사용하고<br/> 그 데이터를 영상으로 띄우기 위해 iframe player api 사용, 카카오 지도의 경우 축제 주제가 국내 한정인 이유와 함께<br/> 다양한 api 기능을 보여주고 접근성이 좋기 때문에 채택하였고 축제 데이터의 경우에는 최신 상태의 다양한 축제 데이터가 존재하기 때문에 채택   |
| 코드블럭              | react-syntax-highlighter | npm.js 사이트에서 demo코드도 확인이 가능하고 설명이 잘 되어있었음                                  |
| RRD(react-router-dom)                | useNavigate, useLocation, useParams, useSearchParms         | 효율적으로 페이지를 전환하고 URL에 맞는 컴포넌트를 보여주기 위해 매우 유용하기 때문에 채택    |

### :five: 작업 목록

#### :pushpin: 5-1) 메인 페이지

![mainpage](https://github.com/user-attachments/assets/b69e279b-268c-429c-ba43-0633fbd3be8c)


- 사용자의 현재 위치 측정
- 축제 전체 리스트 노출
  - 각 게시물의 축제 정보 노출
  - 버튼
    - 카카오 지도: 새 창으로 카카오 맵 길찾기 서비스 이용 
    - 상세보기: 축제 상세페이지로 이동
    - 저장하기: 북마크에 저장 (마이페이지에서 확인 가능) 

#### :pushpin: 5-2) 로그인,회원가입

![auth](https://github.com/user-attachments/assets/de79eb2c-1ae6-4522-b531-df2c10733340)

- 회원가입
  - 사용자의 아이디, 비밀번호, 닉네임을 입력 후 등록된 아이디인지 판별하여 회원가입 처리
- 로그인
  - 사용자의 아이디, 비밀번호 판별 후 로그인 처리

#### :pushpin: 5-3) 상세 페이지

![detail](https://github.com/user-attachments/assets/528f9e60-5b55-49ad-b2e9-1fa4a5446b86)

- 선택한 축제 YOUTUBE 상위 검색 영상 노출
- 선택한 축제 KAKAO MAP 마커 등록
- 선택한 축제 상세 데이터 노출

#### :pushpin: 5-4) 마이 페이지

![mypage](https://github.com/user-attachments/assets/271c0e55-9708-4d53-83d1-9b9c809ab39f)

- 북마크에 저장한 축제 리스트를 확인할 수 있습니다.
  - 상세페이로 이동이 가능합니다.
  - 북마크 취소가 가능합니다.


## :joystick: Trouble Shooting

1. [로그인 실행 시 상태값 변환으로 리렌더링되어 publicRoute의 alert이 출력되는 현상](https://rarrit.github.io/troubleshooting/til/troubleshooting-zustand-logout/)
2. [[Error: Invalid hook call.] 훅은 항상 동기적으로 호출되어야 한다.](https://rarrit.github.io/troubleshooting/til/troubleshooting-hooks-async/)
3. [협업시 .env 환경 변수를 셋팅하며 자주 겪는 오류](https://rarrit.github.io/troubleshooting/til/api/troubleshooting-env-set/)
4. [[Mixed Content] vercel 배포시 공공데이터 API, HTTP 에러](https://rarrit.github.io/troubleshooting/til/api/troubleshooting-vercel-error/)


## :fire: 회고
이번 프로젝트에서 소통의 중요성을 정말 크게 느낀 것 같다. 새로운 팀원을 만나 이야기하며, 각자의 어려움을 이야기했었고 그러한 부분들이 기획 단계에서 많이 소극적으로 반영이 되었다. 프로젝트를 진행하며 모두 적극적으로 임하였고 추가 기능 또한 많이 이야기를 나눴다. 결과적으로는 필수 기능을 모두 구현했지만, 추석이 안겹치고 초기 기획 때 소통을 조금 더 잘했더라면 더 멋진 프로젝트가 되었을 것 같다. (지금도 멋지지만 더..!!)

### :pushpin: Keep - 현재 만족하고 있는 부분
- 초기 세팅부터 배포까지 많이 익숙해진 것 같다.
- zustand를 적극적으로 사용할 수 있게 되었다.
- tanstack query와 custom hook을 사용했다.
- 이전 프로젝트에서 불편하게 느꼈던 디렉터리 구조를 개선했으며, 이후 프로젝트에 FSD 아키텍처 적용 해보려한다.
- 팀원이 작업한 코드를 활용하여 나의 코드도 작동하게 리팩토링 할 수 있었다. (마이페이지 북마크) 

### :pushpin: Problem - 불편하게 느끼는 부분
- 카카오 API, 유튜브 API를 많이 활용하지는 못했다.
- vercel 배포할 때 공공데이터를 앞으로 잘 확인해야겠다. (트러블 슈팅 - 공공데이터 API, HTTP 에러)

### :pushpin: Try - problem에 대한 해결책, 당장 실행 가능한 것
- 개인적으로 사용해본다. (다음 개인과제에 활용할 수 있다면 BEST)