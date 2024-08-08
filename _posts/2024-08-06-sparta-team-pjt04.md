---
title: "JavaScript 영화 검색 사이트 팀 프로젝트 (4차)"
date: 2024-08-06
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
4차 작업은 이전 3차 작업에서 db의 저장한 값과 로그인 되어있는 id를 비교하여, 마이페이지에서 좋아요 선택한 리스트를 출력하는 기능을 완성했다.

- ***목표***
  - 메인 페이지
    - `언어별 데이터 출력 (8/1 완료)`
    - `즐겨찾기(좋아요) 기능 (8/5 완료)`
      - `즐겨찾기 클릭 시 마이페이지에 추가 (8/5 완료)`
      - `좋아요 갯수 노출 (후 순위)`
  - `로그인 (8/2 추가 및 완료)`    
  - `회원가입 (8/2 추가 및 완료)`    
  - `로그아웃 (8/2 추가 및 완료)`    
  - 마이 페이지
    - `즐겨찾기(좋아요) 리스트 페이지 (8/6 완료)`
    - `평점 리스트 페이지 (역할 분장)`
    - `추천글 리스트 페이지 (삭제)`
  - 추천 페이지 (삭제)
    - `추천 글 등록 (삭제)`
      - `영화 검색 후 선택 (삭제)`
      - `해당 영화의 평점, 글 작성 (삭제)`
    - `추천 리스트 (삭제)`
      - `모든 유저의 추천 글 리스트 형식으로 출력 (삭제)`
  - 상세 페이지
    - 좋아요 기능 연동 (적용 예정)

### :three: 적용 목표
- 1) fireBaseConfig.js 생성
  - `fireBase`를 여러 js에서 반복적으로 사용하게 되어, js 파일을 생성 후 export를 사용함
- 2) myPage.js
  - 같은 조원의 준열님께서 myPage.js에 코드를 먼저 작성해주었다. 정리가 잘 되어서 데이터만 잘 가져와서 출력이 목표였다.
    - js파일 최상단에 export한 fireBase에서 필요한 객체인 db, collection, getDocs를 import 했다.
    - `fetchLike()` db에서 데이터를 출력하는 리스트입니다. 이 곳에서 db의 like 컬렉션을 받아와서 현재 해당 db리스트의 user_id값과 현재 로그인한 id값을 비교하여 true일 경우 해당 리스트를 배열로 담아주고 리턴해줌.
    - `renderColumnList()` 함수는 전달받은 데이터를 통해 html 을 출력해주는 함수이며, 리뷰와 좋아요를 같이 사용하고 있기에 type이 like일 경우에만 해당 데이터를 출력하게 적용했다.


### :four: 기능 설명
#### :pushpin: 4-1) fireBaseConfig
- `fireBaseConfig.js`
  - 이전 3차 작업 때 반복적으로 fireBase를 사용한 경우가 잇는데, 이번 4차 작업 때 모듈화를 하기 위해 js 파일을 생성 후 SDK 라이브러리와 fireBase 관련 key값 등등 초기화를 해주었다.
    - 해당 작업을 완료 후 가져다 쓸 값을 export 해주었다. => `export { db, collection, addDoc, getDocs };`

```javascript
// fireBaseConfig.js
// Firebase SDK 라이브러리 가져오기
import { initializeApp } from "url";
import { getFirestore } from "url";
import {
  collection,
  addDoc,
} from "url";
import { getDocs } from "url";

// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "**************",
  authDomain: "**************",
  projectId: "**************",
  storageBucket: "**************",
  messagingSenderId: "**************",
  appId: "**************"
};

// Firebase 인스턴스 초기화
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export { db, collection, addDoc, getDocs };
```

#### :pushpin: 4-2) 좋아요 데이터 리스트 저장
- `fetchLike(userId)` 
  - 해당 함수를 통해 리턴할 배열을 생성해주기 위해 변수를 선언,할당 `likeDataArr = []`
  - `loginSession`변수에 현재 로그인되어있는 세션 값 `userLoginId`를 할당함
  - `likeData`변수에 현재 DB에 저장되어있는 like collection을 할당함
  - `likeData`를 `forEach`문을 통해 순회하며, DB리스트의 저장된 user_id값과 현재 로그인된 값(sessionStorage)가 동일할 경우 `likeDataArr`에 배열로 저장 후 return 해주었다.

```javascript
// mypage.js
async function fetchLike(userId) {
    let likeDataArr = [];
    try {
        const loginSession = sessionStorage.getItem("userLoginId");
        const likeData = await getDocs(collection(db, "like"));

        likeData.forEach(item => {
            const data = item.data();
            if (data.user_id === loginSession) {
                likeDataArr.push(data);
            }
        })
        console.log(likeDataArr);
    } catch (e) {
        console.log("fetchLike Error =>", e);
    }

    return likeDataArr;
}
```

#### :pushpin: 4-3) 좋아요 데이터 리스트 출력 
- `renderColumnList(data, type)`
  - 해당 함수에서 type을 판별하여 html을 생성 후 innerHTML을 통해 화면에 출력하는데, 리뷰와 좋아요를 한 함수에서 사용하게 되어 if문으로 type을 판별 후 내가 전달한 데이터를 바인딩해주었다.


```javascript
// like.js
function renderColumnList(data, type) {
    let htmlContent;
    if (type === "like") {
        htmlContent = `
            <li class="${type} column-container">
                <div class="${type} column-img">
                    <a href="../index.html"><img src="https://image.tmdb.org/t/p/w500/${data.movie_img}"></a>
                </div>
                <div class="${type} column-contents">
                    <h4>${data.movie_title}</h4>
                    <p>${data.movie_over_view}</p>
                    <div class="feed-box">
                        <p>좋아요(3)</p>
                        <p>댓글(323)</p>
                    </div>
                </div>
                <div class="${type} column-date">
                    <p>24분 전</p>
                </div>
            </li>
        `
    } else {
        htmlContent = `
            <li class="${type} column-container">
                <div class="${type} column-img">
                    <a href="../index.html"><img src="${data.backdrop_path}"></a>
                </div>
                <div class="${type} column-contents">
                    <h4>${data.title}</h4>
                    <p>${data.content}</p>
                    <div class="feed-box">
                        <p>좋아요(3)</p>
                        <p>댓글(323)</p>
                    </div>
                </div>
                <div class="${type} column-date">
                    <p>24분 전</p>
                </div>
            </li>
        `
    }

    parentList.innerHTML += htmlContent;
}
```

### :fire: 마무리
3차때 했던 좋아요 작업을 통해 이번 작업은 어느정도 순조로웠다. 4차때 목표였던 `FireBase`를 export, import해서 사용한 것 까지 수행해내서 만족했다. 하나 걱정되는 것이 있는데, 작업을 하며 기존에 작업한 fireBase문법이 기억이 나지 않을 때가 있어 like.js를 반복해서 참고했는데.. 찾아가면서 작업하는 것은 괜찮으나 이후에 코드를 설명할 때 나도 헷갈릴 때가 있다. 이런 부분은 처음 머리속에 설계하고 진행하지 않고 상황에 맞춰 작업하다보니 길을 잃은 것 같은데, 이런 부분을 개선해나갈 수 있으면 좋을 것같다.

#### :pushpin: 추가 적용 예정
- 상세페이지
  - 좋아요 기능 적용

#### :pushpin: 링크
- **깃헙 링크**: [Moview](https://github.com/sHyunis/Moview)
- **적용 화면**: [https://shyunis.github.io/Moview/index.html](https://shyunis.github.io/Moview/index.html)
