---
title: "json-sever란 무엇인지 알아보자! (설치 및 실행 방법)"
date: 2024-09-06
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - api
  - lib
  - development
  - til
tags:
  - json-server
author_profile: true
sidebar_main: true
---

## :ledger: json-server를 알아보자!

## :one: json-server란?

`json-server`란, <u>아주 간단한 ‘DB와 API 서버를 생성해주는 패키지’다.</u> `json-server` 를 사용하는 이유는 Backend(이하 BE)를 구축할 수 없기 때문! Backend 환경이 세팅 전 FakeServer, MOCK Server를 이용해서 Backend 환경을 구축함.

- 굉장히 가볍기에 제어하기도 편함
- FE에서 BE하고 있는 작업을 기다리지 않고, FE의 로직과 환면을 구현할 수 있음 (API 명세서만 완성되어 있다면 먼저 개발이 가능함)

## :two: 설치 방법

### :pushpin: 2-1) npm

```bash
npm install json-server
npm install json-server -D # 개발 환경인 경우, -D 옵션을 함께 입력.
```

### :pushpin: 2-2) yarn

```bash
yarn add json-server
yarn add json-server -D # 개발 환경인 경우, -D 옵션을 함께 입력.
```

## :three: 실행 방법

### :pushpin: 3-1) json된 데이터베이스 생성 (db.json)

root 경로에 db.json 생성 및 json 형식으로 입력

```jsx
// db.json
{
  "todos": [],
  "users": []
}
```

### :pushpin: 3-2) json-server 실행

db.json 파일이 없으면, `File db.json not found` 노출되니 꼭 생성 후 실행하도록 한다.

- 실행하면 귀여운 이모티콘과 함께 `http://localhost:4000/todos`, `http://localhost:4000/users` 연결된 걸 알 수 있다.

```jsx
// 터미널 입력
yarn json-server db.json --port 4000
```

### :pushpin: 3-3) package.json 설정 및 실행

db.json 생성 후 `package.json` 파일을 열어 `"scripts"`안에 `"server": "json-server --watch db.json --port 4000",`을 넣어주고 터미널에서 `yarn server`를 입력하면 똑같이 json-server가 실행된다.

```jsx
// package.json
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "server": "json-server --watch db.json --port 4000",
    "lint": "eslint .",
    "preview": "vite preview"
  },
```

```jsx
// 터미널 입력
yarn server
```

### :fire: 마무리

`json-server`를 통해 간단하게 가짜 서버를 만들고, 프론트엔드 개발을 독립적으로 진행하는 방법을 알아보았다. 다음 글에서는 `fetch`, `axios`를 사용해서 간단한 todoList를 만들어보며, 좀 더 익숙해져보려 한다.

#### :pushpin: 관련 링크

- [json-sever + fetch를 사용한 TodoList CRUD](https://rarrit.github.io/api/library/development/json-server-1-todo-fetch/)
- [json-sever + axios를 사용한 TodoList CRUD](https://rarrit.github.io/api/library/development/json-server-2-todo-axios/)
