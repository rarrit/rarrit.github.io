---
title: "JavaScript 영화 검색 사이트 팀 프로젝트 (1차)"
date: 2024-08-01
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
지난 1차 팀과제와 개인 과제에서 개인적으로 `FireBase`와 `fetch`를 많이 사용해보지 못해 아쉬움이 있었고 이번 팀 과제를 통해 최대한 사용해 보기를 원해 아래와 같이 기능을 적용하기로 했다.

- ***목표***
  - 메인 페이지
    - 언어별 데이터 출력
    - 즐겨찾기(좋아요) 기능
      - 즐겨찾기 클릭 시 마이페이지에 추가
      - 좋아요 갯수 노출
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

### :three: 초기 진행
- 먼저 기능을 정리하고 싶었지만, 와이어 프레임 제출 기한이 몇 시간 남지 않아서 간단하게 하고싶은 기능 상의 후 와이어 프레임을 제출
- 내가 맡은 역할은 기능 중점이여서, FireBase생성 후 기능을 먼저 정리해봤다.
- 이후가 가장 고통스러웠는데 DB를 세팅해본 적이 없어서 암담했다.. 답답한 마음에 아무것도 하지 못했는데, 일단 모든 기능을 정리해보고 DB에 저장되어야 할 요소들을 정리 후 다이어그램 형식으로 페이지마다 기능을 정리해봤다. (이미지는 아래에 올렸습니다.)
- 이후에 DB를 세팅해야되는데, 너무 횡설수설해서 시간이 많이 지나 먼저 적용이 가능할 것 같은 ***언어별 데이터 출력***를 진행했다.
![diagram img](https://github.com/user-attachments/assets/c3b3dd4f-c468-4186-8e70-a9657f91ba16)


### :four: 작업 내용
#### :pushpin: 4-1) 목표
***언어별 데이터 출력*** 기능을 작업하면서 정말 고민이 많았다. `fetch` 문법도 사실 익숙하지 않아서 이전에 정리한 내용을 몇번이고 다시 보게 되었고, 비동기 처리(`async/await`)도 적용하기 까지 쉽지 않았던 것..!<br/>
이후에는 산 넘어 산이었는데, 목표는 언어별 출력되는 기능을 원할 때 어디서든 사용할 수 있도록 적용하고 싶었기에 정말 많이 고민했고 내용은 아래와 같다.

- 1) 언어 변경 버튼을 클릭한다.
- 2) 언어가 무엇인지 판별하기 위해 함수의 매개변수에 값을 넣어서 실행한다.
- 3) 실행한 함수는 `fetch`를 사용하여, 전달받은 매개변수의 값을 if문으로 판별 후 데이터를 할당한다.
- 4) 화면에 랜더링 될 리스트 함수의 매개변수에 데이터를 넣는다.
- 5) 랜더링 될 함수는 전달받은 매개 변수를 통해 HTML을 생성한다.
- 6) 리스트를 출력한다.

이렇게 작성햇을 때는 순서대로 진행하면 쉬울 거라 생각했는데, 5,6 에서 정말 많이 헷갈렸다. 위의 실행 순서는 `클릭 -> 언어 판별 및 데이터 할당 -> HTML 생성 -> 리스트 출력`이었는데, 완성된 코드의 순서는 `클릭 -> 언어 판별 및 데이터 할당 -> 리스트 출력 -> HTML 생성`으로, 초기에 생각한 순서를 깨기까지가 엄청 많은 시간이 걸렸다.

#### :pushpin: 4-2) 적용 코드
- `api.js`
  - 먼저 공용으로 사용하기 위해 api.js를 생성 후 전역으로 설정
  - `changeMovieLang` : 언어에 따라 리스트 출력 함수
    - 매개변수 `language` 는 클릭 이벤트에 실행되는 함수에서 `kr,en` 값을 받아온다.
    - 이후 if문을 통해 언어를 판별해서 변수 `url`에 할당한다.
    - `fetch`의 매개변수에 `url`을 할당 후 `handleMovieRender`함수를 실행
    - `handleMovieRender` 함수는 아래 `language.js`에 작성했습니다.

```javascript
const options = {
  method: "GET",
  headers: {
    accept: "application/json",
    Authorization: "Bearer fbf16579bff5b8c3f6664841d9dd0613",
  },
};

const API_KEY = "fbf16579bff5b8c3f6664841d9dd0613";
const URL = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=en-US&page=1`;
const BASE_URL = `https://api.themoviedb.org/3/movie`;
const LANG_KR = `https://api.themoviedb.org/3/discover/movie?api_key=${API_KEY}&language=ko-KR&with_origin_country=KR&with_genres=16&without_genres=10749}&page=1`;
const LANG_EN = `https://api.themoviedb.org/3/discover/movie?api_key=${API_KEY}&language=en-US&with_origin_country=US&with_genres=16&without_genres=10749}&page=1`;


/*
 * 언어 변경 API 함수 입니다.
 * 언어가 추가될 경우 if문 추가, language.js 이벤트 리스너 추가하시면 됩니다.
*/
async function changeMovieLang(language) {
  let url;
  if (language === 'kr') url = LANG_KR;
  else if (language === 'en') url = LANG_EN;
  try {
    const res = await fetch(url);
    const data = await res.json();
    handleMovieRender(data.results);
  } catch (e) {
    console.log("language api error =>", e);
  }
}
```

- `language.js`
  - `handleMovieRender` 함수는 전달받은 매개변수를 통해 화면에 출력한다.
  - 전달받은 매개변수(데이터)를 `map`메서드를 사용하여 `handleMovieCreate` 함수를 실행한다.

```javascript
/**
 * 언어별 출력하는 스크립트 입니다.
 * 사용 페이지에서 id값을 맞춰 html을 추가
 * 만약 id가 변경되어야 한다면, 이벤트리스너만 추가해주시면 됩니다.
 */
const btnLanguageKr = document.getElementById("btn-lang-kr");
const btnLanguageEn = document.getElementById("btn-lang-en");

// 리스트 생성
function handleMovieCreate(movie) {
  return `
    <div class="movie-card">
      <div class = "card-img">
        <img src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class = "movie-content">
      <h3>${movie.title}</h3>
      <p>${movie.overview}</p>
      <span>Rating: ${movie.vote_average}</span>
      </div>    
    </div>
  `
}

// 리스트 출력
function handleMovieRender(movies) {
  console.log(movies);
  const movieContainer = document.getElementById("movie-container");
  movieContainer.innerHTML = "";
  movieContainer.innerHTML = movies.map(handleMovieCreate).join('');
}

// 클릭 이벤트
btnLanguageKr.addEventListener("click", () => changeMovieLang('kr'));
btnLanguageEn.addEventListener("click", () => changeMovieLang('en'));
```


### :fire: 마무리
유지보수가 쉽고 재사용이 가능한 코드를 생각하는 것이 정말 어렵다. 개인적으로 이런 부분이 강했으면 좋겠는데, 실행 순서를 작성하고 코드를 짜봤는데, 생각처럼 쉽지 않았다는.. 작은 기능이지만 만들어봤고 좋은 코드일지는 모른다. 앞으로 계속 생각하고 만들면서 성장할 수 있기를 바란다.

#### :pushpin: 개선할점
- 이전에 일단 기능이 돌아가기만 하면 된다고 생각하고 코드를 만들었는데, 이후 수정하는 시간이 정말 오래걸렸던 기억이 있다. 이번엔 충분히 생각하고 기능을 만들었지만 내가 생각한 것이 맞다고 판단되어 다른 방법을 찾지 않고 고집부리느라 시간이 정말 많이 소요되었다. 개선할점은 나의 생각이 무조건 맞다고 고집부리지말고 유연하게 생각할 수 있도록 노력해봐야겠다.

#### :pushpin: 추가 적용 예정
- fireBase 세팅
- 즐겨찾기(좋아요) 기능
  - 즐겨찾기 클릭 시 마이페이지에 추가
  - 리스트 좋아요 갯수 노출

#### :pushpin: 링크
- **깃헙 링크**: [Moview](https://github.com/sHyunis/Moview)
- **적용 화면**: [https://shyunis.github.io/Moview/index.html](https://shyunis.github.io/Moview/index.html)