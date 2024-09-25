---
title: "터미널 npm EACCES 권한 문제 해결하기"
date: 2024-09-25
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - npm
  - eacces
author_profile: true
sidebar_main: true
---

## :ledger: npm EACCES 권한 문제, 왜 발생할까?
이번 타입스크립트 투두 리스트를 만드는 과정에서 npm create vite@latest 명령어를 실행할 때, 정말 난처하게 만든 권한 오류를 해결한 경험을 정리한 글이다.

### :one: 개요

이번 타입스크립트를 진행할 때 항상 yarn 으로 세팅했던 지난번과 다르게 npm으로 세팅하니 아래와 같은 `EACCES` 권한 오류가 발생했다. 이는 주로 npm 캐시 폴더의 파일이 루트 권한으로 설정된 경우 발생하는 문제로, 어떻게 해결했는지 정리해 보았다.

```bash
npm ERR! Error: EACCES: permission denied
npm ERR! syscall open
npm ERR! path /Users/gimmingyu/................
```


### :two: 원인

이 오류는 이전 npm 버전에서 발생한 버그로 인해, npm 캐시 폴더 내 일부 파일이 root 소유로 설정되어 생긴 문제라고 한다. 이로 인해 현재 사용자 권한으로 npm 캐시 파일에 접근할 수 없게 된다... (내가 뭘 했길래..?)

### :three: 해결방법
해결 방법은 매우 간단하다. 캐시 폴더 내 루트 소유 파일을 현재 사용자에게 소유권을 변경해주면 되는데... 사실 아래와 같이 실행하고 정말 쉽게 해결해서 나중에 다른 오류가 생기지 않길 기도중이다..

#### :pushpin: 3-1) 권한 변경 명령어 실행
에러 메시지가 왔을 때 아래의 최하단 에러를 보면 `npm error   sudo chown -R 501:20 "/Users/gimmingyu/.npm"` 와 같이 되어있어 터미널에 `sudo chown -R 501:20 "/Users/gimmingyu/.npm`를 입력해주고 다시 `npm create vite@latest react-ts-todo-study`을 입력하니 정상적으로 프로젝트가 설치되었다.

```bash
gimmingyu@gimmingyuui-MacBookPro-2 ~ % npm create vite@latest react-ts-todo-study
npm error code EACCES
npm error syscall open
npm error path /Users/gimmingyu/.npm/_cacache/index-v5/8e/2b/6606333763561b3b1b957af88e679ff83ee16842c3301b9f01b817993f82
npm error errno EACCES
npm error
npm error Your cache folder contains root-owned files, due to a bug in
npm error previous versions of npm which has since been addressed.
npm error
npm error To permanently fix this problem, please run:
npm error   sudo chown -R 501:20 "/Users/gimmingyu/.npm"

```

### :four: 요약

- npm 캐시 폴더 내 파일 소유권 문제로 EACCES 오류 발생
- sudo chown -R $(whoami):$(id -gn) ~/.npm 명령어로 해결
- 문제 해결 후 Vite 프로젝트 생성 명령어 정상 실행 가능

### :fire: 마무리

처음에 에러 메세지를 보고 당황해서 구글에 검색해도 별다른 소득이 없었고 쥐피티형도 2~3번은 이상한 방법을 알려줘서 멘탈이 많이 나가있었다. 마음을 다스리고 에러메세지에 있는걸 그냥 따라해봤는데 해결되었으며, 뒤의 일은 미래의 나에게 맡긴다.