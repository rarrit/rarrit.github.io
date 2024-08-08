---
title: "JavaScript 영화 검색 사이트 팀 프로젝트 (3차)"
date: 2024-08-05
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

![sparta-team-pct-01](https://github.com/user-attachments/assets/02cfdbcb-3c6c-44dc-b8ce-70dd16e2d163)
<br/>
![sparta-team-pct-02](https://github.com/user-attachments/assets/3af1d375-c11c-4d58-9bc4-ede8607a7ce0)

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
3차 작업을 진행하며, 즐겨찾기(좋아요) 기능을 적용했다. fetch를 사용해 출력한 뮤비 리스트에서 즐겨찾기를 클릭 하면 현재 로그인 아이디(세션 스토리지)와 클릭한 리스트의 id,img,title,overview 값을 db에 저장하고 마이페이지에서 출력하는 기능이다.

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
    - `즐겨찾기(좋아요) 리스트 페이지 (8/5 완료)`
    - `평점 리스트 페이지 (역할 분장)`
    - `추천글 리스트 페이지 (삭제)`
  - 추천 페이지 (삭제)
    - `추천 글 등록 (삭제)`
      - `영화 검색 후 선택 (삭제)`
      - `해당 영화의 평점, 글 작성 (삭제)`
    - `추천 리스트 (삭제)`
      - `모든 유저의 추천 글 리스트 형식으로 출력 (삭제)`

### :three: 적용 목표
- 1) FireBase 세팅
  - collection like 추가
    - `user_id` : 현재 로그인한 유저 아이디 값
    - `movie_id` : 클릭한 리스트 movie 아이디 값    
    - `movie_img` : 클릭한 리스트 movie 이미지 src 값
    - `movie_title` : 클릭한 리스트 movie 타이틀디 값
    - `movie_over_view` : 클릭한 리스트 movie 짧은글 값
- 2) 리스트 좋아요 
  - fetch를 사용해 출력한 리스트에 `movie-like`를 추가함
  - 기존 리스트 전체를 클릭하면 상세페이지로 이동되어 클릭 시 movie-like일 경우 따로 기능 적용
    - document.body.addEventListener("click"), if(e.target.classList.contains("movie-like"))
  - [1] 좋아요 클릭 시 현재 세션 스토리지 값(로그인 ID)를 가져와서 if문으로 로그인 판별
    - 로그인되었을 경우: `getMovieLike()` 함수를 통해 해당 클릭한 movie id 값을 전달
    - 비로그인일 경우: `alert("회원만 가능합니다.")` 출력
  - [2] `getMovieLike()`함수에서 전달받은 매개변수 movie id값과 `fetch`를 통해 전달받은 movie data 를 `if`문으로 판별하여 클릭한 무비 리스트를 가져옴.
    - 이후 해당 리스트의 아이디,이미지,타이틀,짧은 글을 변수로 담아 `handleLikeAdd()`의 매개변수에 할당함
  - [3] `handleLikeAdd()`의 js와 다른 js 중 type이 module로 되어있어서 함수가 전달이 않되었음
    - <u>script가 module로 되어있으면 다른 js끼리 서로 참조가 안됌</u>
      - `window.handleLikeAdd = handleLikeAdd`; 를 사용해서 전역으로 함수를 설정했으나, <u>이 방법은 옳지 않음.</u> 가능하면 전체적으로 수정이 필요했으나, 일정이 촉박하여 편법을 사용함...
  - [4] `handleLikeAdd`함수에서 query를 사용하여, db리스트에 있는 로그인 아이디,무비 아이디와 매개변수로 전달받은 로그인 아이디,무비 아이디 값을 비교 해당 쿼리를 `likeQueryStart`변수를 통해 db에 문서가 있는지 판별함
    - if문을 통해 db에 저장된 값이 없을 경우 db에 저장함
    - if문을 통해 db에 저장된 값이 있을 경우 like(현재 좋아요 상태 true,false) 값을 반전시켜주고 db를 업데이트해줌
  - [5] 마지막으로 첫 로드시에 좋아요 표시가 되어있을 수 있게 `movieLikeChk()`함수를 실행시켜 줌


### :four: 기능 설명
#### :pushpin: 4-1) 클릭 이벤트
- `like.js - click event`
  - 리스트 클릭 시 상세로 이동되어 `movie-like`인지 확인
    - 현재 로그인 여부(`세션 스토리지`) 값 확인, 클릭한 무비 리스트의 id 값 변수`getId`에 할당
  - `if`문으로 세션 값 확인(로그인 여부)
    - 로그인일 경우 
      - `getMovieLike(getId)`함수의 매개변수에 id값 전달
      - 클릭한 `movie-like`의 class중 `curr(엑티브)`가 되어있으면 remove, 아니면 add해줌
    - 로그인 아닐 경우
      - alert으로 "회원만 가능합니다." 출력

```javascript
// like.js
document.addEventListener("DOMContentLoaded", () => {
  document.body.addEventListener("click", async (e) => {
    if (e.target.classList.contains("movie-like")) {
      try {
        const sessionChk = await sessionStorage.getItem("loginState");
        const getId = e.target.closest(".movie-card").querySelector(".card-id").innerText;        
        if (sessionChk) {
          getMovieLike(getId);
          if (e.target.classList.contains("curr")) {
            e.target.classList.remove("curr");
          } else {
            e.target.classList.add("curr");
          }
        } else {
          alert("회원만 가능합니다.");
        }
      } catch (e) {
        console.log("좋아요 에러 =>", e);
      }
    }
  })
})
```

#### :pushpin: 4-2) 무비 리스트 중 클릭한 무비 데이터 저장
- `getMovieLike(movieId)` 
  - url 변수를 통해 현재 언어의 api 값을 전달 받음 (이 값은 language.js의 hanldeMovieRender 함수를 통해 세션값을 저장함)
  - 매개변수로 전달받은 아이디 값을 `getMovieId` 변수에 할당함
  - `if`문을 사용하여 `getMovieId`값을 판별
    - 있을 경우: `fetch`를 사용하여, 전체 무비 리스트와 전달받은 id을 판별하기 위해 `filter`메서드를 사용함
      - 이후 true인 값이 있으면, 해당 값으로 id,title,img,overview를 각 변수에 할당해주고 `handleLikeAdd`의 매개변수에 할당함
    - 없을 경우: `catch(e)` 에러 메세지 console.log 출력

```javascript
// api.js
async function getMovieLike(movieId) {
  const url = getUrl("us");
  let getMovieId = movieId;
  if (getMovieId) {
    try {
      const res = await fetch(url);
      const data = await res.json();
      const movies = data.results;
      const targetMovie = movies.filter(item => item.id === Number(getMovieId))[0];
      const targetId = targetMovie.id;
      const targetImg = targetMovie.backdrop_path;
      const targetTitle = targetMovie.title;
      const targetOverView = targetMovie.overview;
      console.log(targetId, targetTitle, targetOverView, targetImg);
      handleLikeAdd(targetId, targetImg, targetTitle, targetOverView);
    } catch (e) {
      console.log("getMovieLike Error =>", e);
    }
  }
}
```

#### :pushpin: 4-3) handleLikeAdd 함수 전역으로 설정
- like.js 와 api.js에서 스크립트를 html에서 Import할 때 <u>type이 module로 되어있어서 선언한 함수가 참조가 안되는 이슈가 있었음.</u>
  - 이와 같은 경우 모듈화를 해줘야하는데, 시간상으로 여유가 없어서 `window.handleLikeAdd = handleLikeAdd;`문법으로 함수를 전역으로 설정해줌. 그렇지만 이와같은 방법은 좋은 방식이 아니므로, 가능하다면 다른 해결방법을 찾는게 좋음


```javascript
// like.js
window.handleLikeAdd = handleLikeAdd;
```


#### :pushpin: 4-4) DB 저장
  - 세션스토리지에 저장된 회원 아이디 값을 loginId에 할당
  - 매개변수로 해당 뮤비의 아이디값을 받아오고 movieId에 할당
  - query 함수를 사용하여 db의 like컬렉션을 확인함
    - where()절을 사용하여 like의 user_id,movie_id와 현재 로그인한 loginId, 클리한 movieId 비교함
  - likeQueryStart에 getDocs함수를 사용하여 쿼리를 실행, likeQuery의 where조건에 맞는 문서를 가져옴
  - likeQueryStart(가져온 문서)가 비어있는지 확인함
    - 값이있다면, newLikeState 변수에 현재 like의 반대(true면 false)를 할당
      - docRef 변수에 likeDoc, 즉 like DB의 고유 id값을 할당함
      - 고유 id값의 like를 업데이트 해줌
      - 값이없다면, user_id,movie_id,like값을 전달할 newDoc객체에 저장함
        - 이후 새로 추가될 데이터를 db에 저장함


```javascript
// like.js
async function handleLikeAdd(id, likeImg, likeTitle, likeOverView) {
  const loginId = sessionStorage.getItem("userLoginId");
  const movieId = id;
  const movieImg = likeImg;
  const movieTitle = likeTitle;
  const movieOverView = likeOverView;

  try {
    const likeQuery = query(
      collection(db, "like"),
      where("user_id", "==", loginId),
      where("movie_id", "==", movieId)
    );

    const likeQueryStart = await getDocs(likeQuery);

    if (!likeQueryStart.empty) {
      likeQueryStart.forEach(async (likeDoc) => {
        const likeData = likeDoc.data();
        const newLikeState = !likeData.like;

        const docRef = doc(db, "like", likeDoc.id);
        await updateDoc(docRef, { like: newLikeState });

      });
    } else {
      const newDoc = {
        user_id: loginId,
        movie_id: movieId,
        movie_img: movieImg,
        movie_title: movieTitle,
        movie_over_view: movieOverView,
        like: true
      };
      await addDoc(collection(db, "like"), newDoc);
    }
  } catch (e) {
    console.log("handleLikeAdd error =>", e);
  }
}

```


### :fire: 마무리
처음에 좋아요 기능에 대해 생각했을 때, 쉽게 생각했었던 내 자신을 후회하며 머리를 계속 박으면서 적용했다..<br/>
처음 구상한 생각은 아래와 같았다.<br/>
1. 클릭하면 해당 movie값을 가져와야지!
2. movie값과 현재 로그인한 세션을 db에 저장해야지!
3. 로그인했을 때, 나의 로그인 여부(세션)와 movie값을 비교해서 좋아요 표시된 내용을 체크해야지!

실제로 적용하면서 script module 설정으로 인한 js가 참조가 않되는 이슈와 세션값을 통해 movie값을 가져올 때의 처리 과정. db에 저장하는 과정 이 모든 과정에서 console.log를 찍어 확인할 정도로 시행착오가 정말 많았다. 가장 고통스러웠던 것은 `fireBase`의 query와 where을 통해 조건에 맞는 문서를 가져오는 것이었는데, 처음에는 JavaScript로 처리하다 시간이 너무 소요되어 결국 구글링을 통해 진행하였다. 이후 마이페이지와 상세페이지에서의 기능은 조금 더 편하게 적용할 수 있을 것 같지만 개발 기능을 적용하기 전의 기획이 얼마나 중요한지 느끼게 되었고 JavaScript의 모듈화의 필요성을 정말 크게 느끼게 되었다. (현재.. fetch와 firebase가 각 js마다 호출되고있음.. 반복반복) 프로젝트 기간이 얼마 남지않아서 중간에 수정하게 되면 각 조원들께 폐를 끼칠 것 같아서 가장 가까운 마이페이지를 적용할 때 export,import를 사용하여 firebase를 적용해봐야겠다.

#### :pushpin: 추가 적용 예정
- 마이페이지
  - 좋아요 리스트 출력
- 상세페이지
  - 좋아요 기능 적용

#### :pushpin: 링크
- **깃헙 링크**: [Moview](https://github.com/sHyunis/Moview)
- **적용 화면**: [https://shyunis.github.io/Moview/index.html](https://shyunis.github.io/Moview/index.html)