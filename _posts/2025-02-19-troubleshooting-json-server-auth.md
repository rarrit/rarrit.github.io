---
title: "json-server-auth Cannot find module express 에러 해결 과정"
date: 2025-02-19
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - json-server-auth
author_profile: true
sidebar_main: true
---

## :ledger: json-server-auth express 에러 해결 과정

프로젝트 인증,인가 테스트를 하기 위해 `json-server-auth`를 사용하다 겪은 에러 해결 과정이다.

### :one: 개요

- 프로젝트를 진행하며, `json-server`를 설치 후 테스트를 진행하다 로그인,회원가입을 테스트하기 위해 `json-server-auth`를 설치했다.

```bash
yarn add -D json-server-auth
```

- 설치를 진행하고 `auth-db.json`을 생성 후 `package.json`에 `json-server-auth` 미들웨어를 추가했다.

```javascript
// package.json

"scripts": {
  "auth": "json-server-auth --watch auth-db.json --port 8000"
}
```

- `yarn auth` 명령어로 입력하여 실행해봤는데, `express`가 없다는 에러 발생

```bash
Error: Cannot find module 'express'
```

[구글에 검색](https://github.com/jeremyben/json-server-auth/issues/46)하니, 글로벌하게 설치하도록 해야 한다고 했다. 하지만 문제는 계속 발생했고 시도했던 방법은 아래와 같다.

### :two: 시도한 방법

#### :pushpin: 2-1) express 설치

`json-server`을 설치할 때 `express` 가 딸려온다고 했지만, `yarn add express`를 통해 다시 설치해줬다. 하지만 문제는 해결되지 않았다.

#### :pushpin: 2-2) package.json 확인

`package.json`에서 확인했을 때 `json-sever`와 `json-server-auth`가 `dependencies`,`devDependencies` 각각 따로 설치가 되어있어서 혹시 이것이 문제일까 싶어서 `devDependencies`에 같이 있게 수정했다. 하지만 문제는 해결되지 않음 (ㅂㄷㅂㄷ)

#### :pushpin: 2-3) 패키지 재설치

설레는 마음으로 시도해봤지만 어김없이 해결되지 않음

```bash
rm -rf node_modules yarn.lock  # 기존 패키지 삭제
yarn install  # 다시 설치
yarn auth  # 실행
```

#### :pushpin: 2-4) json-server 버전 변경

gpt 형님께 조언을 물었는데, 버전 호환성 문제가 발생할 수 있다고 했다. 당시 `json-server`와 `json-server-auth`의 버전은 아래와 같다.

- `"json-server"`: "^1.0.0-beta.3",
- `"json-server-auth"`: "^2.1.0",

```javascript
"json-server": "^1.0.0-beta.3",
"json-server-auth": "^2.1.0",
```

기존의 `json-server`을 삭제해주고 다시 설치해서 테스트를 해봤다.

```bash
yarn remove json-server
yarn add json-server@^0.17.0 -D # 안정적인 이전 버전이라캄
yarn auth
```

**성공**;

### :three: 해결 방법

이것저것 해봤는데 `json-server`의 버전을 변경하고 정상적으로 노출되는 것을 확인할 수 있었다. 최근에 `tailwind CSS`도 버전 때문에 알던 방법으로 설치가 되지 않았는데, 문제가 생겼을 때 버전도 잘 확인해봐야겠다
