---
title: "axios response 객체에 대해 알아보자"
date: 2024-09-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - api
tags:
  - axios
author_profile: true
sidebar_main: true
---

## :ledger: Axios의 Response 객체에 대해 알아보자
Axios로 요청을 보내면, 응답 객체(`response`)에는 여러 속성들이 포함되어 있다. 각 속성의 역할을 알아보자.

![axios response](https://github.com/user-attachments/assets/e8c9aeab-6876-4c20-a9e8-b5f823ae0a91)

### :one: config

![axios response config](https://github.com/user-attachments/assets/6cb065a7-602d-4f18-83e6-c66c8c1e57e4)

- 요청을 보낼 때 사용된 설정 객체이다. 
- 요청한 URL, HTTP 메서드, 헤더, 전송된 데이터 등이 포함된다.
- 주로 자주 사용하는 속성은 url, method, headers, timeout임
- transformRequest/Response는 고급 기능으로 유용하다함 하지만 아직 잘 모르겠음
- adapter 같은 설정은 프론트엔드 개발에서 중요도가 낮다고함

1. `url`
  - 요청을 보낼 대상 URL임
2. `method`
  - HTTP 메서드 (GET, POST, PUT, PATCH, DELETE)
  - 중요함 각 메서드에 대해 무조건 이해하고 있어야함 모르면 못 씀
3. `headers`
  - 서버로 보낼 HTTP 헤더
  - 인증 토큰이나 콘텐츠 타입을 설정할 때 자주 사용됨
4. `timeout`
  - 요청이 타임아웃되기까지 시간을 설정한다.
  - 기본적으로 타임아웃이 없지만, 네트워크 문제를 대비해 설정하는 것이 좋다함
5. `transformRequest & transformResponse`
  - 요청이나 응답을 변환할 때 사용됨
  - 데이터를 전송하기 전에 가공하거나 응답을 원하는 형태로 변환할 때 유용함
  - 기본 설정도 상관없지만 커스텀마이징할 때 알아두면 좋다함
6. `adapter`
  - 요청 방식(`xhr`, `http`, `fetch`)을 지정함
  - 브라우저 환경에서는 기본설정 그대로 사용하면 되어서 신경 쓸 필요는 거이 없음

### :two: data
- 서버로부터 응답받은 실제 데이터이다.
- API에서 요청한 정보가 이 속성에 담겨 있음

```json
[
  {"id": 1, "title" : "axios response"},
  {"id": 2, "title" : "axios study"}
]
```

### :three: headers
- 서버가 응답으로 보낸 HTTP 헤더 정보이다.
- 여기에는 콘텐츠 타입, 응답 시간, 쿠키 등의 정보가 포함됨

```json
{
  "content-type": "application/json",
  "date": "Sat, 14 Sep 2024 12:34:56 GMT"
}
```

### :four: request
- request 객체는 브라우저가 서버로 보내는 HTTP요청을 관리하는 `XMLHttpRequest`객체다.
- 프론트엔드 개발자가 request 객체에서 주로 신경 써야 하는 것은 status, response, timeout 정도라 함
- 파일 업로드를 다루는 경우엔 upload 속성도 중요할 수 있음

1. `status`
  - 서버로부터 받은 HTTP 상태코드임
2. `statusText`
  - 상태 코드에 대한 설명임
3. `response`, `responseText`
  - 서버로부터 받은 실제 응답 데이터임
  - 보통 JSON이나 텍스트 형태로 응담함 
4. `readyState`
  - 요청의 상태를 나타냄
5. `onerror, ontimeout, onabort`
  - onerror: 요청이 실패했을 때 
  - ontimeout: 타임아웃되었을 때
  - onabort: 중단되었을 때
6. `timeout`
  - 요청이 타임아웃되기까지 시간(밀리초)
  - 응답이 너무 오래 걸릴 때 타임아웃을 설정하여 처리 가능

### :five: status, statusText
- HTTP 상태 코드와 상태 코드에 대한 설명임
- [HTTP 상태코드 - RARRIT](https://rarrit.github.io/api/development/what-is-http/)


### :fire: 마무리
console.log로 response를 찍어보며 궁금한 내용들을 정리해봣다. 모든 속성을 깊게 알 필요는 없으며 주로 사용되는 핵심 내용만 이해하고 있어도 좋을 거같다. 

#### :pushpin: 관련 글
