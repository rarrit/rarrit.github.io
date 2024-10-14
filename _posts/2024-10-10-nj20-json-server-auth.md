---
title: "[Next.js] 인증,인가와 json-server-auth 사용법"
date: 2024-10-10
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - lib
tags:
  - json-server-auth
author_profile: true
sidebar_main: true
---

## :ledger: 인증,인가와 json-server-auth 사용법에 대해 알아보자
`Next.js`에서 인증과 인가는 필수적인 기능 중 하나다. 이 글에서는 인증과 인가의 개념을 살펴보고, 간단하게 사용할 수 있는 `json-server-auth` 라이브러리를 이용한 인증 서버 구현 방법을 알아보자.

### :one: 인증, 인가
Next.js에서 인증을 구현하려면 두 가지 개념을 알고 있어야 한다. **인증(Authentication)**은 사용자의 신원을 확인하는 과정이며, **인가(Authorization)**는 인증된 사용자가 특정 리소스나 기능에 접근할 수 있는 권한을 결정하는 과정이다.

### :pushpin: 1-1) 인증
인증 방식에는 여러 가지가 있으며, 개발자가 구현할 때는 프로젝트의 요구 사항에 맞는 방식을 선택해야 한다.

- `OAuth/OpenID Connect (OIDC)` 
  - 사용자 자격 증명을 공유하지 않고 타사 접근을 활성화한 상태로 하는 인증 / 소셜 미디어 로그인 및 단일 로그인(SSO) 솔루션
- `자격 증명 기반 로그인(이메일 + 비밀번호)`
  - 사용자가 이메일과 비밀번호를 이용한 인증
- `비밀번호 없는/토큰 기반 인증` 
  - 비밀번호 없는 접근을 위해 이메일 링크 또는 SMS 일회용 코드를 사용한 인증

### :pushpin: 1-2) 인가
인가 과정에서는 인증된 사용자가 어떤 자원에 접근할 수 있는지를 결정하게 된다. 특정 페이지나 API 경로에 대해 권한을 확인하고, 허가된 사용자만 접근할 수 있도록 설정할 수 있다. 예를 들어, 관리자인지, 일반 사용자인지에 따라 접근 가능한 페이지를 구분하는 방식으로 구현할 수 있다.

### :two: json-server-auth
인증 서버를 직접 구현하는 것은 복잡할 수 있지만, 간단한 인증 및 인가 기능을 테스트하고 싶다면 `json-server-auth`를 사용할 수 있다. `json-server-auth`는 json-server의 확장 기능으로, 인증과 인가 기능을 쉽게 추가할 수 있는 라이브러리입니다.

#### :pushpin: 2-1) 설치 방법
아래의 명령어를 터미널에 입력한다.

```bash
yarn add -D json-server-auth
```

#### :pushpin: 2-2) 설정 01: 파일 생성 및 데이터 추가
루트 디렉토리에 `auth-db.json` 파일을 생성하고 사용자의 데이터를 추가한다.

```json
// auth-db.json

{
  "users": [
    {
      "email": "user@example.com",
      "password": "$2a$10$eHUzzsY1tbOmyGVqPwVEsuYRQb4LP2Hw/dMXgxp5p8eYE4UgcZMA.",
      "id": 1
    }
  ]
}
```

#### :pushpin: 2-3) 설정 02: json-server를 설정 및 미들웨어 추가
`json-server`를 설정하고 package.json에 `json-server-auth` middleware를 추가한다

```json
// package.json

"scripts": {
  "auth": "json-server-auth --watch auth-db.json --port 8000"
}
```

#### :pushpin: 2-4) 서버 실행
- http://localhost:8000에서 JSON 서버가 실행된다. 
- `json-server-auth`는 users 리소스에 대해 자동으로 인증 및 권한 부여 엔드포인트를 추가함.

```bash
yarn auth
```

### :three: 사용 예시

#### :pushpin: 3-1) 로그인 예시
아래는 `axios`를 이용하여 `json-server-auth` 서버에서 로그인 요청을 보내는 예시이다.

```tsx
import axios from 'axios';

const login = async (email, password) => {
  try {
    const response = await axios.post('http://localhost:8000/login', {
      email,
      password,
    });
    
    if (response.status === 200) {
      const { accessToken, user } = response.data;
      // 토큰 저장
      localStorage.setItem('token', accessToken);
      console.log('로그인 성공:', user);
    }
  } catch (error) {
    console.error('로그인 실패:', error);
  }
};
```

#### :pushpin: 3-2) 인가 예시
인증된 사용자가 접근할 수 있는 페이지를 만들기 위해서는 Next.js에서 서버사이드 렌더링 또는 클라이언트 측에서 토큰 검증을 할 수 있다. 예를 들어, 사용자가 로그인 상태인지 확인하고, 아니라면 로그인 페이지로 리다이렉트할 수 있다.

```tsx
import { useEffect } from 'react';
import { useRouter } from 'next/router';

const ProtectedPage = () => {
  const router = useRouter();

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (!token) {
      router.push('/login');
    }
  }, [router]);

  return (
    <div>
      <h1>보호된 페이지입니다.</h1>
    </div>
  );
};

export default ProtectedPage;
```

### :fire: 마무리
Next.js에서 인증과 인가를 구현하는 기본적인 개념과, 간단한 인증 서버로 `json-server-auth`를 사용하는 방법을 알아보았다. `json-server-auth`는 JSON 데이터를 이용해 손쉽게 인증과 인가 기능을 테스트할 수 있는 도구다.
