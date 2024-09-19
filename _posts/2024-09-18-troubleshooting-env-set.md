---
title: "협업시 .env 환경 변수를 셋팅하며 자주 겪는 오류"
date: 2024-09-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:  
  - troubleshooting
  - til
tags:
  - troubleshooting
author_profile: true
sidebar_main: true
---

## :ledger: 이번 아웃소싱 프로젝트와 이전 프로젝트를 진행하며 자주 겪었던 에러다.

### :one: 개요

![youtube-api-error](https://github.com/user-attachments/assets/90ea8b0a-37a7-447e-86d6-6ef949fbff8c)

추석동안 각자 프로젝트 진행 후 merge하는 과정에서 유튜브 API가 정상적으로 호출되지 않는 문제가 발생했다. 이전에도 비슷한 상황을 겪었지만 당시에는 문제를 해결하지 못했고, 결국 API가 정상적으로 호출되는 동료의 코드를 clone해서 작업한 경험이 있다.

### :two: 분석 및 처리
이전과 같은 문제가 다시 발생했을 때, 원인을 분석한 결과 `.env`파일이 제대로 설정되지 않은 것이 원인이었다. 협업 과정에서 `env`파일은 보안상의 이유로 Git에 업로드되지 않기 때문에 pull 받은 사람이 따로 설정해야한다.

### :three: 해결 방법
#### :pushpin: 3-1) env 사용 이유
[DH의 개발 공부로그 글](https://shape-coding.tistory.com/entry/React-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-env-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
> env 사용이유 개발을 하다보면 외부로 알려지면 안되는 API_KEY 나 db관련 정보 등등 보안이 필요한 값들이 있습니다. 이러한 값들을 보안이나 유지보수를 용이하게 하기 위해 .env 파일에 환경변수로 만들어 변수를 꺼내와 사용하는 것 입니다.

#### :pushpin: 3-2) 해결 과정
1. `환경 변수 공유`
  - API를 설정한 팀원은 env에 값을 설정할 때 동료에게 알려주고 동일한 API_KEY값이 적용되었는지 확인

### :fire: 마무리
다른 API_KEY 값을 세팅하면서 쉽게 겪을 수 있는 문제라 생각한다. 이전에는 해결 못 했지만 동료의 도움을 받아 이번에 쉽게 처리했으며, 다음에 협업할 때 같은 문제에 대해서 쉽게 처리할 수 있게되었다.