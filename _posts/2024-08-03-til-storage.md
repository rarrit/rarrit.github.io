---
title: "API 로컬 스토리지와 세션 스토리지"
date: 2024-08-03
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - js
  - api
  - til
tags:
  - api
  - local storage
  - session storage
author_profile: true
sidebar_main: true
---

## :ledger: Local Storage 와 Session Storage
로컬 스토리지와 세션 스토리지는 웹 스토리지 API의 두 가지 주요 구성 요소로, 클라이언트 측에 데이터를 저장하기 위해 사용된다.

### :one: 로컬 스토리지 (Local Storage)
#### :pushpin: 정의
로컬 스토리지는 웹 브라우저에 데이터를 로컬로 저장하는 방법 중 하나로, 데이터를 영구적으로 저장할 수 있다. <u>사용자가 브라우저를 닫거나 컴퓨터를 재부팅하더라도 데이터가 유지</u>된다.

#### :pushpin: 사용 이유
- ***영구적인 데이터 저장***
  - 데이터를 영구적으로 저장하고, <u>브라우저를 닫아도 데이터가 유지</u>됨
- ***큰 용량***
  - 5MB까지 데이터를 저장할 수 있어 <u>쿠키보다 더 많은 데이터를 저장</u>할 수 있음
- ***사용 예***
  - 사용자 설정, 로그인 상태 유지, 오프라인 데이터 저장 등 <u>영구적인 데이터가 필요</u>한 경우

#### :pushpin: 문법
- `setItem` : 데이터를 저장함
- `getItem` : 데이터를 가져옴
- `removeItem` : 데이터를 삭제함
- `clear` : 모든 데이터를 삭제함

```javascript
// 데이터를 저장
localStorage.setItem('key', 'value');

// 데이터를 가져옴
let data = localStorage.getItem('key');

// 데이터를 삭제
localStorage.removeItem('key');

// 모든 데이터 삭제
localStorage.clear();
```

#### :pushpin: 사용 예시
***사용자 테마 설정 저장***
```javascript
// 사용자 테마를 저장한다.
localStorage.setIem('theme', 'white');

// 페이지 로드 시 적용할 테마를 변수에 할당함
const theme = localStorage.getItem('theme');

// body의 클래스를 주어 테마 설정할 수 있음
if(theme) document.body.className = theme;
```

### :two: 세션 스토리지 (Session Storage)
#### :pushpin: 정의
세션 스토리지는 웹 브라우저의 세션 동안 데이터를 저장하는 방법으로, 브라우저 탭 또는 창을 닫을 때 데이터가 삭제되며, 같은 창이나 탭내에서만 데이터가 유지된다.

#### :pushpin: 사용 이유
- ***임시 데이터 저장***
  - 영구적으로 저장되는 로컬 스토리지와 달리 <u>세션 동안만 데이터를 저장하고, 세션이 끝나면 자동으로 삭제</u>됨
- ***보안***
  - 민감한 데이터가 세션이 끝날 때 자동으로 삭제되므로 <u>보안에 유리</u>함
- ***사용 예***
  - 사용자의 <u>단기적인 입력 데이터나 일시적으로 상태 정보를 저장</u>해야 할 때

#### :pushpin: 문법
- `setItem` : 데이터 저장함
- `getItem` : 데이터 가져옴
- `removeItem` : 데이터 삭제함
- `clear` : 전체 데이터 삭제함

```javascript
sessionStorage.setItem('key', 'value');

// 데이터를 가져옴
let data = sessionStorage.getItem('key');

// 데이터를 삭제
sessionStorage.removeItem('key');

// 모든 데이터 삭제
sessionStorage.clear();
```

#### :pushpin: 사용 예시
***로그인 상태 체크***
```javascript
// 사용자 추가 또는 중복 메시지 출력
if (idChk && pwChk) {
  alert("로그인이 완료되었습니다.");
  sessionStorage.setItem("loginState", "true"); // 로그인 상태 저장
  sessionStorage.setItem("userLoginId", userLoginId); // 로그인 아이디 저장
  window.location.href = "/";
} else if (!idChk && pwChk) {
  alert("아이디를 확인해 주세요.");
} else if (idChk && !pwChk) {
  alert("비밀번호를 확인해 주세요.");
} else {
  alert("아이디와 비밀번호가 틀립니다.")
}
```

### :three: 차이점
1. 저장 기간
  - ***로컬 스토리지(Local Storage)***
    - 데이터가 영구적으로 저장됨
    - 브라우저를 닫고 다시 열어도, 컴퓨터를 재부팅해도 데이터가 남아있음
  - ***세션 스토리지(Session Storage)***
    - 데이터가 세션 동안만 저장됨
    - 브라우저 탭 또는 창을 닫으면 데이터가 삭제됨
2. 범위
  - ***로컬 스토리지(Local Storage)***
    - 같은 도메인의 모든 탭과 창에서 공유됨
  - ***세션 스토리지(Session Storage)***
    - 현재 탭이나 창에서만 접근 가능함
    - 다른 탭이나 창에선 접근할 수 없음
3. 상황에 맞게 쓰는 예시
  - ***로컬 스토리지(Local Storage)***
    - 영구적인 데이터 저장 (사용자가 선택한 웹사이트의 테마 설정)
      - 사용자 설정, 테마 언어 선택 등 사용자가 브라우저를 닫아도 유지되어야 하는 데이터
    - 오프라인 데이터 (오프라인에서 작성된 메모 및 문서)
      - 오프라인에서 작업 후 온라인으로 동기화할 데이터
    - 캐싱 (자주 방문하는 페이지의 데이터 캐싱)
      - 자주 변경되지 않는 데이터를 캐싱하여 성능 향상
  - ***세션 스토리지(Session Storage)***
    - 일시적인 데이터 저장 (쇼핑몰 장바구니, 임시 데이터)
      - 브라우저 탭이나 창이 열려 있는 동안만 필요한 데이터
    - 보안 (로그인 세션 동안 필요한 사용자의 정보)
      - 브라우저를 닫으면 자동으로 삭제되어야 하는 민감한 데이터
    - 다단계 폼 (단계별로 입력된 데이터를 임시로 저장)
      - 페이지 이동 시 유지해야 하는 폼 데이터

4. 비교 표
| 특성 | 로컬 스토리지 | 세션 스토리지 |
| 저장기간 | 영구적 | 세션 동안 |
| 데이터 접근 범위 | 같은 도메인의 모든 탭/창 공유 | 현재 탭/창 |
| 저장 용량 | 약 5MB | 약 5MB |
| 사용 예시 | 사용자 설정, 오프라인 데이터 | 임시 데이터, 보안 데이터|
| 자동 삭제 | 안됨 | 브라우저 탭/창 닫을 때|

### :fire: 마무리
이번 팀 프로젝트를 진행하며 필수 구현 중 ***로컬 스토리지***가 있었는데, 내가 구현할 기능 중 로그인이 있어서 ***세션 스토리지***를 사용하게 되었다. 작업 후 두 스토리지의 차이점이 궁금했고 이번 공부를 통해 알 수 있게 되었다. 각 스토리지를 상황에 맞게 사용할 수 있는 개발자가 되길 바라며 글을 마무리해 본다!
