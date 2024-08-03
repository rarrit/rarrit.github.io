---
title: "JavaScript 영화 검색 사이트 팀 프로젝트 (2차)"
date: 2024-08-02
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - js
  - api
  - mini
tags:
  - mini project
  - fetch
  - firebase
author_profile: true
sidebar_main: true
---

## :ledger: JavaScript으로 영화 검색 사이트를 제작해보자!
스파르타 코딩클럽 본 캠프 JavaScript 팀 과제 입니다.

### :one: 프로젝트 설명
#### :pushpin: 1-1) 목표
1. Javascript 과정을 마무리하며, 팀원들과 함께 JS 문법의 핵심을 적용해 볼 수 있는 영화 검색 사이트를 제작합니다.
2. 기존에 개인 과제에서 TMDB를 이용하여 수행하신 과제의 심화 버전으로 진행합니다.

#### :pushpin: 1-2) 과재 개요
1. 개인과제에서 작성한 [내배캠 인기영화 콜렉션]을 발전시키는 팀 프로젝트
2. 팀원들의 프로젝트 N개 중 1개를 대표로 선택, 팀 프로젝트로 발전
  - 6조 박소현님의 프로젝트로 진행하기로 결정!

#### :pushpin: 1-3) 필수구현사항
1. TMDB 또는 영화진흥위원회 오픈 API 이용(택 1 또는 중복 사용)
2. 영화정보 상세 페이지 구현
3. 상세 페이지 영화 리뷰 작성 기능 구현
  - 작성자, 리뷰, 확인비밀번호를 입력하도록 구현합니다.
  - 작성한 정보는 브라우저의 localStorage에 적재하도록 합니다.
4. github PR(=Pull Request) 사용한 협업
5. UX를 고려한 validation check
6. Only JavaScript 문법으로 구현

#### :pushpin: 1-4) 기술스텍
- HTML5, CSS3, JavaScript, fetch, FireBase

### :two: Tasks
2차 작업을 진행하며, 즐겨찾기, 마이페이지와 추천페이지의 기능을 구현하려면 로그인,회원가입 기능이 필요하여 추가 및 우선순위로 작업을 진행했다.

- ***목표***
  - 메인 페이지
    - `언어별 데이터 출력 (8/1 완료)`
    - 즐겨찾기(좋아요) 기능
      - 즐겨찾기 클릭 시 마이페이지에 추가
      - 좋아요 갯수 노출
  - `로그인 (8/2 추가 및 완료)`    
  - `회원가입 (8/2 추가 및 완료)`    
  - `로그아웃 (8/2 추가 및 완료)`    
  - 마이 페이지
    - 즐겨찾기(좋아요) 리스트 페이지
    - 평점 리스트 페이지
    - 추천글 리스트 페이지
  - 추천 페이지
    - 추천 글 등록
      - 영화 검색 후 선택
      - 해당 영화의 평점, 글 작성
    - 추천 리스트
      - 모든 유저의 추천 글 리스트 형식으로 출력

### :three: 적용 목표
전날 `fetch`를 사용하여 재사용이 가능한 `언어별 데이터 출력`기능을 만들었고 오늘은 `로그인,회원가입,로그아웃` 기능을 목표로 작업을 진행했다. DB는 `FireBase`를 사용했는데, 강의를 봤던 내용이 기억이 잘 나지않아 다시 강의를 보며 세팅부터 하나씩 진행해보았다. 목표 기능은 아래와 같다.

- 1) FireBase 세팅
  - 1차에서 키워드 정리한 내용으로 collection 세팅
- 2) 회원가입
  - login, password input의 값을 DB에 저장
    - login value 값과 DB에 저장된 값이 같을 경우 "중복된 아이디가 있습니다." 노출
- 3) 로그인
  - login, password input의 값을 작성 후 로그인 버튼을 클릭하면 DB의 값과 비교하여 판별
    - login = true, password = false 일 경우 "비밀번호가 틀립니다." 출력
    - login = false, password = true 일 경우 "아이디가 틀립니다." 출력
    - 둘 다 false일 경우 "아이디와 비밀번호가 틀립니다." 출력
  - 로그인 성공 시 `session storage`를 사용하여, 로그인 상태 여부 확인
    - 로그인 성공 시 기존의 로그인,회원가입 버튼 none, 로그아웃, 마이페이지 버튼 block 처리
- 4) 로그아웃
  - 로그아웃 버튼 클릭 시 `session storage`의 로그인 상태 여부 false
  - 로그아웃,마이페이지 버튼 none, 로그인,회원가입 버튼 block 처리

### :four: 기능 설명
#### :pushpin: 4-1) 회원가입
- `join.js`
  - `FireBase` 연동 및 인스턴스 초기화 작업을 진행
  - DB에 저장할 값을 객체로 생성 
  - 회원가입 버튼 클릭 시 DB의 `user`의 데이터 값을 `forEach`문으로 확인하여 중복 계정이 있는지 확인
  - 없으면 `if`문을 통해 DB에 저장 및 alert으로 메세지 출력
  - 작성 코드는 아래와 같습니다.

```javascript
// Firebase SDK 라이브러리 가져오기
import { initializeApp } from "url";
import { getFirestore } from "url";
import {
  collection,
  addDoc,
} from "url";
import { getDocs } from "url"

// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "**********",
  authDomain: "*****",
  projectId: "**********",
  storageBucket: "**********",
  messagingSenderId: "*****",
  appId: "**********"
};

// Firebase 인스턴스 초기화
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const btnJoinUp = document.getElementById("join-btn");


btnJoinUp.addEventListener("click", async () => {
  const userJoinId = document.getElementById("join-id").value; // value 값 저장
  const userJoinPw = document.getElementById("join-pw").value;

  // db에 전달할 값 할당
  let doc = {
    user_id: userJoinId,
    user_pw: userJoinPw
  }

  try {
    const userDb = await getDocs(collection(db, "user")); // user data 가져옴
    let userChk; // 중복 계정 있는지 확인용

    // 중복 계정 확인
    userDb.forEach((userDoc) => {
      let userData = userDoc.data();
      // console.log("userData.user_id =>", userData.user_id);
      // console.log("userJoinId =>", userJoinId);
      // console.log("userChk =>", userChk);
      if (userData.user_id === userJoinId) userChk = true; // 중복 계정 없을 경우 true 할당
    });

    // 사용자 추가 또는 중복 메시지 출력
    if (!userChk) {
      await addDoc(collection(db, "user"), doc);
      alert("사용자가 성공적으로 추가되었습니다.");
    } else {
      alert("중복된 아이디가 있습니다.");
    }


  } catch (e) {
    console.log('error =>', e)
  }
})
```

#### :pushpin: 4-2) 로그인, 로그아웃
- `login.js`
  - `FireBase` 연동 및 인스턴스 초기화 작업을 진행
  - 로그인 버튼 클릭
  - 클릭 이벤트 안에 db에 전달할 `doc` 객체 생성
  - 변수 `userDb`를 선언 후 user data 할당
  - `idChk, pwChk` 변수 선언
  - `for of`문으로 중복 계정 확인 및 `idChk,pwChk` 값 재할당 
  - `if`문을 통해 전달 받은 `id,Chk, pwChk` 값으로 판별 후 성공했으면 `session storage` 값 저장, 실패했으면 상황에 맞는 alert 알림 문구 출력
  - 로그아웃 기능은 버튼 클릭 시 `session storage`의 로그인 상태 값을 삭제해주고 알림 문구 출력

```javascript
btnLogin.addEventListener("click", async () => {
  const userLoginId = document.getElementById("login-id").value.trim(); // value 값 저장
  const userLoginPw = document.getElementById("login-pw").value.trim();

  // db에 전달할 값 할당
  let doc = {
    user_id: userLoginId,
    user_pw: userLoginPw
  }

  try {
    const userDb = await getDocs(collection(db, "user")); // user data 가져옴
    let idChk = false, pwChk = false;

    // 중복 계정 확인
    for (const userDoc of userDb.docs) {
      let userData = userDoc.data();
      console.log("| user ID => ", userData.user_id, "| user PW => ", userData.user_pw, "| userLoginId =>", userLoginId, "| userLoginPw =>", userLoginPw);
      if (userData.user_id === userLoginId && userData.user_pw === userLoginPw) {
        console.log("둘다 맞아");
        idChk = true;
        pwChk = true;
        break;
      } else if (userData.user_id === userLoginId && userData.user_pw !== userLoginPw) {
        console.log("비밀번호가 틀려");
        idChk = true;
        pwChk = false;
        break;
      } else if (userData.user_id !== userLoginId && userData.user_pw === userLoginPw) {
        console.log("아이디가 틀려");
        idChk = false;
        pwChk = true;
      }
    }

    console.log(idChk, pwChk);

    // 사용자 추가 또는 중복 메시지 출력
    if (idChk && pwChk) {
      alert("로그인이 완료되었습니다.");
      sessionStorage.setItem("loginState", "true"); // 로그인 상태 저장
      sessionStorage.setItem("userLoginId", userLoginId); // 로그인 아이디 저장
      window.location.href = "/Moview/index.html";
    } else if (!idChk && pwChk) {
      alert("아이디를 확인해 주세요.");
    } else if (idChk && !pwChk) {
      alert("비밀번호를 확인해 주세요.");
    } else {
      alert("아이디와 비밀번호가 틀립니다.")
    }


  } catch (e) {
    console.log('error =>', e)
  }
})


btnLogout.addEventListener("click", async () => {
  await sessionStorage.removeItem("loginState");
  alert("로그아웃 되었습니다.");
  window.location.href = "/Moview/index.html";
})
```

#### :pushpin: 4-3) 로그인 판별 전역 js
- `common.js`
  - `session storage`값을 판별하여, DOM 조작 설정
    - 로그인했을 경우 로그인,회원가입 버튼 삭제 및 로그아웃,마이페이지 버튼 추가
    - 로그아웃일 경우 로그인,회원가입 버튼 노출 및 로그아웃,마이페이지 버튼 삭제

```javascript
document.addEventListener("DOMContentLoaded", () => {
  handleLoginChk(); //로드 후 로그인 판별
});

async function handleLoginChk() {
  const session = await sessionStorage.getItem("loginState");
  const headerBtnLogin = document.getElementById("header-btn-login");
  const headerBtnMypage = document.getElementById("header-btn-mypage");
  const headerBtnLogout = document.getElementById("header-btn-logout");
  const headerBtnSignup = document.getElementById("header-btn-signup");
  console.log(session);
  if (session === "true") {
    headerBtnLogin.style.display = "none";
    headerBtnSignup.style.display = "none";
    headerBtnMypage.style.display = "block";
    headerBtnLogout.style.display = "block";
  } else {
    headerBtnLogin.style.display = "block";
    headerBtnSignup.style.display = "block";
    headerBtnMypage.style.display = "none";
    headerBtnLogout.style.display = "none";
  }
}
```


### :fire: 마무리
로그인,회원가입,로그아웃 기능을 만들어봤다 처음에 `FireBase`를 설정하는 것 부터 어려움이 컸는데, 이전에 봤던 강의를 참고하며 진행하느라 시간이 너무 많이 소요되었다. 그리고 로그인,회원가입 기능은 `FireBase`의 유용한 라이브러리가 있었지만 이번 팀 과제 또한 `JavaScript`로 구현하기 위해 어떤 방식으로 데이터를 주고 받고 해야할 지 고민이 정말 많았다. 특히 회원가입에서 `forEach`문을 사용하여 값을 전달했던것을 로그인 페이지에서 똑같이 `forEach`문으로 진행하다가 `idChk, pwChk`의 값이 마지막 순회의 값을 전달받아 원하는 값이 출력잉 않되어서 결국 구글링 후 수정해주었다.<br/><br/>
처음 만든 기능이라 미흡한 부분이 정말 많고 고려하지 못한 것도 많지만 개인적으로 한번도 안해본 것과 한번이라도 해본것에 의미를 두고 긍정적인 생각으로 멘탈을 관리해보려한다..!

#### :pushpin: 추가 적용 예정
- 즐겨찾기(좋아요)
  - 즐겨찾기 클릭 시 마이페이지에 추가
  - 리스트 좋아요 갯수 노출
- 마이페이지
  - 리스트(3가지) 별 데이터 값 출력
