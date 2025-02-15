---
title: "[1차] 스파르타 최종 프로젝트 - 주제 및 기획 선정"
date: 2024-10-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: [1차] 스파르타 최종 프로젝트 - 주제 및 기획 선정

스파르타 부트캠프 프론트엔드 과정이 벌써 끝나간다. 마지막 프로젝트는 디자이너도 같이 협업할 기회가 있고 기간또한 긴 만큼 모두가 얻어갈 수 있는게 많은 프로젝트가 되었으면 좋겠다.

### :one: 필수 기능 가이드

필수 기능 가이드는 아래와 같다.

1. 반응형 웹페이지 서비스 구현
2. 성능최적화 후 분석리포트 작성

최종 프로젝트인 만큼 반응형은 필수로 구현이 되어야하고 성능최적화를 위해 lighthouse를 활용해서 성능을 최적화하는 경험이 필수로 되어있었다. 이전에 워드프레스 블로그를 운영하며 lighthouse를 많이 이용했는데, 이번 팀 프로젝트를 통해 내가 팀에 도움이 될 수 있으면 좋을거란 생각이 들었다.

### :two: 주차별 타임라인

마지막 프로젝트인 만큼 지금까지 했던 것 보다 일정이 많이 길어졌다. 일정은 가이드로 제공되어서 팀원과 잘 조율해서 진행하면 좋을 것 같다.

- **개발자 데드라인**

  - **주제 및 기획 선정 완료 `10/21(월)까지`**
  - MVP 기능 구현 완료 및 배포 **`11/6(수)까지`**
  - UT 테스트 시작 **`11/8(금)~`**
  - UT 테스트 기반 기능 개선 및 버그 수정 **`11/11(월)~`**
  - 최종발표 **`11/21(목)`**

- **디자이너 데드라인**

  - **주제 및 기획 선정 완료 `10/21(월)까지`**
  - 웹/모바일 시안 1가지 완료 **`11/4(금) 19:00까지`**
  - 나머지 디바이스 대응 포함 최종 디자인 **`11/11(월) 19:00까지`**
  - 사용자 피드백 기반 디자인 개선 작업 **`11/15(금) 19:00까지`**

- **주차별 작업 내용**
  - `10/18~10/25` 1주차: 주제 및 기획 선정 완료
  - `10/28~11/04` 2주차 : UI 컴포넌트의 기본 기능 개발
  - `11/04~10/08` 3주차 & 중간발표회(11/07) : MVP 기능 구현 완료 및 배포
  - `11/11~11/15` 4주차 : UT 테스트 기반 기능 개선 및 버그 수정
  - `11/18~10/21` 최종 발표회(11/21)

### :three: 프로젝트 관리 툴

팀이 결정되고, 프로젝트 관리 툴은 어떤걸 사용할 지 이야기를 나눴고 아래와 같이 사용하기로 했다.

- `Notion`: 지금까지 사용했던 툴이고, 스파르타에서 세팅을 잘 해주었지만 최종 프로젝트에서는 기본 정보와 5분 기록보드만 작성하기로 했다.
- `Trello`: 트렐로는 현재까지도 팀원 협업 툴로 잘 사용되어있고, 원래 `Jira`를 사용하려 했으나, 프로젝트 단위가 크지 않다는점에서 가볍고 손쉽게 사용할 수 있는 트렐로를 선택했다. 또한 현재 팀에서 사용하고있는 슬랙과 깃허브에도 통합해서 파일을 공유할 수 있다는 점이 컸다.

### :four: 주제 정하기

현재 팀 구성은 디자이너분들까지 합쳐서 총 7명이다. 주제를 정하기 전 각각 어떤 주제로 서비스를 만들면 좋을지 고민해오고 회의를 통해 이야기했다.

#### :pushpin: 4-1) 논의된 주제들

1.  러닝서비스
2.  반려동물 병원 서비스
3.  나의 모든 처음을 도와주는 서비스
4.  사회적 약자를 도와주는 서비스
5.  IT 통합 커뮤니티
6.  수영 커뮤니티
7.  국내 섬 정보 커뮤니티
8.  반려인 지식 공유 기반 커뮤니티
9.  캠핑장 정보 앱 커뮤니티

#### :pushpin: 4-2) 주제 선정과 이유

이번 최종 프로젝트의 주제는 `캠핑장 정보 및 커뮤니티` 였다. 선정한 이유는 아래와 같다.

- 활용할 수 있는 데이터가 충분히 준비되어 있음
- 데이터가 쌓여있지 않은 상태에서도 기능 이용이 가능함
- 다양한 레퍼런스가 존재함
- 타 캠핑 사이트는 기능적으로 문제가 되는 요소가 많았음
- 이유: 기존 서비스를 이용하면서 사용자가 불편함을 겪어 이러한 이슈들을 개선 해서 서비스를 만들어보려함 (앱에 사용자 불만 댓글 많음)
  - 캠핏: 뒤로가기 하면 바로 닫혀버리는 이슈, 로그인 상태 처리 이슈
  - 땡큐캠핑: 캘린더 이슈, 버퍼링 이슈, 검색 이슈(필터링 처리)
  - 캠핑지도: 필터링 이슈, 중복된 캠핑장 이슈
- 회원, 팔로우, 좋아요, 커뮤니티, 캠핑장 정보, API 활용성, 실시간 피드 등이 잇음

### :fire: 회고

주제를 정하고 간단하게 기능을 정의하는 시간까지 총 합쳐서 10시간 이상이 걸렸다. 모두 적극적으로 소통하여, 의견이 다를 경우 각자 어떤 부분에서 반대하는지 이야기하고 설득하는 과정이 너무 좋았다. 무엇보다 팀이 결정된지 시간이 별로 되지 않았음에도 서로 어색한 분위기 없이 열정적으로 임하였고 주제도 잘 정하게 된 것 같다.
