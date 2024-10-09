---
title: "[Next.js] Route Handler와 Server Action을 알아보자"
date: 2024-10-08
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
tags:
  - next js
  - route handler
  - server action
author_profile: true
sidebar_main: true
---

## :ledger: Next.js의 Route Handler와 Server Action에 대해 알아보자!
이번 Next.js에서 가장 헷갈렸던 Route Handler와 Server Action에 대해 알아보고 차이점은 무엇인지 정리해보자.

### :one: Route Handler란 무엇일까?
`Route Handler`는 이름 그대로 경로(Route)에 대한 요청을 처리하는 "핸들러"를 의미한다. 웹사이트에서 어떤 주소로 이동하거나 그 주소로 정보를 보내는 요청이 발생할 때 서버가 그 요청을 어떻게 처리할지 정의하는 부분이라 이해했다.

#### :pushpin: 1-1) Route Handler가 필요한 이유
- 사용자가 웹사이트에서 특정 페이지를 방문할 때(예: `/rotation`) 서버가 그 정보를 어떻게 응답할지 결정해야함.
- 서버에 정보를 전달하는 경우 (예: 회원가입, 로그인 등등) 어떻게 데이터를 처리할 지 필요함

정리하면 웹사이트에서 누군가 클릭하거나 버튼을 누를 때 어떤 일이 발생해야 할지 설정하는게 Route Handler라고 보면 된다.

#### :pushpin: 1-2) Route Handler 사용법
- `app/api/경로/route.ts` 생성 (예: api/rotation/route.ts)
  - api 디렉토리 내부에 route.ts 파일을 만나면 기본적으로 Next.js는 router handler로 인식함
- 사용 방법은 아래와 같다.

```tsx
// GET, POST, PUT, DELETE, PATCH
export async function GET(request: Request) {
  console.log("GET /api/test");
}

export async function POST(request: Request) {
  console.log("POST /api/test");
}

export async function PUT(request: Request) {
  console.log("PUT /api/test");
}

export async function DELETE(request: Request) {
  console.log("DELETE /api/test");
}

export async function PATCH(request: Request) {
  console.log("PATCH /api/test");
}
```

#### :pushpin: 1-3) 실제 사용 예시
이번 Next.js 프로젝트에서 사용한 코드는 아래와 같다.

- `getChampionRotation` 함수는 두 가지 데이터를 결합함
  - 전체 챔피언 리스트 (`getChampions`를 통해 가져옴)
  - Riot API에서 가져온 로테이션 중인 챔피언들의 ID 리스트 (`freeChampionIds`)
- `freeChampionIds`에 포함된 챔피언 ID에 해당하는 챔피언들을 필터링하여 클라이언트에서 사용할 수 있도록 해줌.

```tsx
// api/rotation/route.ts
import { RotationType } from "@/app/types/ChampionRotation";
import { RIOT_API_KEY } from "../apiKey";

export async function GET(){
  const res = await fetch(`https://kr.api.riotgames.com/lol/platform/v3/champion-rotations`, {
    headers: {
      'Content-Type': 'application/json',
      'X-Riot-Token': RIOT_API_KEY as string
    }
  });
  const data: RotationType = await res.json();    
  
  return Response.json({ data });
}

// utils/riotApi.ts
import { getChampions } from './serverApi';
import { ChampionInfo } from '../types/Champion';

export const getChampionRotation = async () => {
  const champions = await getChampions();
  const response = await fetch('/api/rotation');
  const result = await response.json();
  
  const freeChampionIds = result.data.freeChampionIds;
  const rotation: ChampionInfo[] = Object.values(champions).filter((champion) =>
    freeChampionIds.includes(Number(champion.key))
  );

  return rotation;
};
```

### :two: Server Action이란 무엇일까?
`Server Action`은 말 그대로 서버에서 수행하는 동작(액션)으로 이해하면 된다. 웹 브라우저는 보통 화면에 정보를 표시하거나, 버튼을 눌러서 데이터를 보낼 때 사용되는데, Server Action은 서버가 이런 요청을 직접 처리해줌

#### :pushpin: 2-1) Server Action의 필요성
- 사용자가 웹사이트에서 어떤 정보를 입력하거나(예:댓글 작성), 특정 동작을 요청할 때(예:게시글 삭제) 그 요청은 서버에서 처리된다. 이럴 경우 서버가 어떻게 반응할지를 정의하는 것이 `Server Action`이다.
- 서버는 데이터베이스에 정보를 저장하거나, 특정 조건에 따라 데이터를 변경하는 등의 작업을 함

#### :pushpin: 2-2) Server Action 사용법
- 서버에서만 실행되는 함수로, 클라이언트가 서버에 데이터를 보내거나 요청할 때 사용됨
- 코드에 `"use server"` 지시어가 사용됨

```tsx
"use server" // 서버 액션 정의

export async function 함수명(){
  // ...
}
```

### :three: Route Handler와 Server Action의 역할 및 장점

#### :pushpin: 3-1) Route Handler
  - `역할`
    - API 엔드포인트를 정의하고 HTTP 요청(GET, POST, PUT, DELETE, PATCH 등) 처리하는데 사용됨
    - 클라이언트와 서버 간의 데이터 교환을 용이하게 하며, RESTful API를 구현하는데 유용함
  - `사용법`
    - app/api/[route]/route.ts 파일에서 HTTP 메서드에 함수를 정의함
  - `장점`
    - API 요청을 간단하게 처리할 수 있으며, 별도의 서버 없이 RESTful API를 구성할 수 있음
    - 클라이언트에서 직접 호출이 가능하며, API 응답을 반환함

#### :pushpin: 3-2) Server Action
  - `역할`
    - 클라이언트에서 발생한 이벤트(예: 폼제출)를 처리하고, 서버에서 비즈니스 로직을 수행하는데 사용됨
    - 클라이언트와 서버 간의 데이터 전송을 간소화하고, 서버에서 직접 로직을 실행할 수 있게함
  - `사용법`
    - 클라이언트 컴포넌트 내에서 `async` 함수로 정의되며, `"use server"` 지시어를 사용하여 서버에서 실행함
  - `장점`
    - API 엔드포인트를 별도로 만들지 않고도 데이터를 서버에서 직접 처리할 수 있어 코드가 단순해짐
    - 서버 액션은 클라이언트에서 접근할 수 없는 서버 환경에서 실행되므로 보안적인 장점이 있음

#### :four: Route Handler와 Server Action 차이점 요약 표

| ------- | ------------------ | ----------------- |
| **기능** | **Route Handler**	| **Server Action** | 
| `목적` | API 요청을 처리 | 클라이언트 이벤트를 처리 | 
| `실행` | 위치	서버에서 실행 | 서버에서 실행 | 
| `사용법` | HTTP 메서드별로 | 분리된 함수 정의	클라이언트 컴포넌트 내에서 정의 | 
| `보안` | 클라이언트에서 호출 가능	| 클라이언트 접근 불가 | 
| `장점` | RESTful API 구현 용이 | 서버 로직 간소화 및 보안 강화 | 

### :fire: 마무리
Next.js의 `Server Action`과 `Route Handler`에 대해 알아보았다. `Server Action`은 보안이 중요한 민감한 데이터 처리에 적합하고, `Route Handler`는 다양한 요청을 쉽게 처리할 수 있는 유연성을 제공한다. 이 두가지를 이해하고 함께 유연하게 사용할 수 있도록 해야겠다.