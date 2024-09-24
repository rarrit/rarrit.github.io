---
title: "[Mixed Content] vercel 배포시 공공데이터 API, HTTP 에러"
date: 2024-09-23
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:  
  - troubleshooting
  - til
  - api
tags:
  - troubleshooting
author_profile: true
sidebar_main: true
---

## :ledger: 아웃소싱 팀 프로젝트 vercel로 배포할 때 겪은 에러입니다.

### :one: 개요

팀 프로젝트를 완료 후 vercel로 배포했는데, 축제 API가 받아와지를 경험함

![api-http-error](https://github.com/user-attachments/assets/0f9da476-ebed-46ff-a2a5-af7fd9eca134)

콘솔에 첫 번째로 찍힌 `Mixed Content` 에러를 번역하면 "https://festival-recommendations.vercel.app/" 페이지가 HTTPS를 통해 로드되었지만 안전한 XMLHttpRequest 엔드포인트 'http://api.data.go.kr/...'을 요청했습니다. 이 요청은 차단되었습니다. 콘텐츠는 HTTPS를 통해 제공되어야 합니다." 와 같다.

### :two: 분석 및 처리
Vercel에서 배포한 프로젝트의 콘솔을 확인한 결과, `Mixed Content` 에러가 발생함. 이는 HTTPS로 제공되는 페이지가 HTTP 프로토콜을 사용하는 외부 API를 호출했을 때 발생하는 오류이며, `Vercel`은 기본적으로 `HTTPS`를 사용하여 페이지를 제공하기 때문에, 보안되지 않은 HTTP 요청을 차단했다. 이러한 상황에서는 API 요청을 HTTPS로 변경하거나, 다른 방식으로 문제를 해결해야 함.

1. HTTP와 HTTPS 혼용 문제: 배포된 Vercel 사이트는 HTTPS로 제공되었으나, 사용된 공공데이터 API는 HTTP로만 제공됨.
2. 브라우저 보안 정책: 브라우저는 HTTPS 페이지에서 HTTP 리소스를 요청하는 경우 'Mixed Content'로 간주하여 보안상의 이유로 요청을 차단함.

### :three: 해결 방법
결과적으로 해결은 하지 못했다. 정X오 튜터님의 추천으로 `프록시 서버`를 설정하거나 `데이터베이스`를 사용해서 문제를 해결하는 방법이 있는데, 당일 제출날이어서 적용하기 어려운 부분이 컸다. 

1. `프록시 서버 설정:` Vercel에서 자체적으로 API 요청을 받아서 HTTPS로 변환하여 다시 호출할 수 있도록 프록시 서버를 설정하는 방법이며 이는 API 호출을 직접 HTTPS로 만들지 못하는 경우 많이 사용하는 방법이라 한다. 이 방법을 통해 Vercel이 HTTPS 환경에서도 안전하게 HTTP API를 사용할 수 있다.
2. `데이터베이스 사용:` 공공데이터 API를 주기적으로 호출하여, 필요한 데이터를 데이터베이스에 저장한 후, 데이터베이스에서 필요한 정보를 가져오는 방법이며, 이 방법은 API 호출 빈도를 줄이고, 안전한 HTTPS 환경에서 데이터를 사용할 수 있도록 보장한다.

### :fire: 마무리
이번 경험을 통해, 배포 환경에서 HTTP와 HTTPS 간의 보안 차이가 어떻게 문제가 될 수 있는지를 배울 수 있었다. `Vercel`처럼 `HTTPS`를 강제하는 플랫폼에서는 <u>API 호출 시에도 HTTPS를 사용해야 한다는 점을 인식하는 것이 중요</u>하며, 프록시 서버 설정이나 백엔드를 통해 안전한 데이터 전송 방법을 적용할 필요가 있었지만, 시간 제약으로 인해 적용하지 못한 것이 아쉬웠다. 앞으로는 API 선택 시에도 HTTPS 지원 여부를 미리 확인하는 것이 정말 정말 정말 중요하다는 것을 느꼈다.