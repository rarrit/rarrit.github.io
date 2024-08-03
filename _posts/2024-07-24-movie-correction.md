---
title: "JavaScript 영화 검색 사이트 제작 및 풀이 과정 (1차)"
date: 2024-07-24
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
author_profile: true
sidebar_main: true
---

## :ledger: JavaScript으로 영화 검색 사이트를 제작해보자!
스파르타 코딩클럽 본 캠프 2주차 - 개인 과제입니다.

![MINKYU MOVIE COLLECTION 01](https://github.com/user-attachments/assets/10654426-d1ae-464b-b8a6-9f637a539d37)
<br/>
![MINKYU MOVIE COLLECTION 02](https://github.com/user-attachments/assets/c1e22b00-3047-4f74-8991-43a26a7a9c57)


### :one: 프로젝트 설명
#### :pushpin: 1-1) 과제 설명
1. Javascript 과정을 마무리하며, JS 문법의 핵심을 적용해 볼 수 있는 영화 검색 사이트를 제작합니다.
2. 영화정보 오픈API인 TMDB(The Movie DB)를 사용합니다.
  - TMDB API문서 https://developer.themoviedb.org/reference/intro/getting-started

#### :pushpin: 1-2) 과재 개요
1. 순수 바닐라 자바스크립트만으로 영화 리스트 조회 및 검색 UI 구현
2. 학습해온 자바스크립트 문법을 최대한 활용. (jQuery No No)
3. 스타일링 작업하며 css와 친해지기

#### :pushpin: 1-3) 요구사항
1. 완성본 예시
  - [https://nabacam-movies.vercel.app/](https://nabacam-movies.vercel.app/)
2. 필수 구현 사항
  - [1] jQuery 라이브러리 사용없이 순수 바닐라 자바스크립트 사용하기
  - [2] TMDB 오픈 API를 이용하여 인기영화 데이터 가져오기
  - [3] 영화정보 카드 리스트 UI 구현
  - [4] 영화 검색 UI 구현
  - [5] 아래 기재된 Javascript 문법 요소를 이용하여 구현
    - const, let, 화살표함수, 배열 메서드, DOM 제어하기
3. 선택 구현 사항
  - [1] CSS: flex, grid 사용하기
  - [2] 웹사이트 랜딩 또는 새로고침 후 검색 입력안에 커서 자동위치시키기
  - [3] 대소문자 관계없이 검색 가능하게 하기
  - [4] 키보드 enter키를 입력해도 검색버튼 클릭한 것과 동일하게 검색 실행시키기
  - [5] 원하는 추가기능 무엇이라도 okay!

### :two: 사용 언어 및 폴더 구조
- HTML5, CSS3, JavaScript, fetch
- 📦TopRatedMovies<br/>
 ┣ 📂assets<br/>
 ┃ ┣ 📂css<br/>
 ┃ ┃ ┗ 📜common.css<br/>
 ┃ ┃ ┗ 📜layout.css<br/>
 ┃ ┃ ┗ 📜main.css<br/>
 ┃ ┃ ┗ 📜slide.css<br/>   
 ┃ ┃ ┗ 📜style.css<br/>    
 ┃ ┗ 📂js<br/>
 ┃ ┃ ┗ 📜api.js<br/>
 ┗ 📜index.html<br/>

### :three: 작성 코드
#### :pushpin: 3-1) `HTML` 코드

  ```html
  <!-- header -->
  <header id="header">
      <div class="inner">
          <h1>RARRIT MOVIE CORRECTION</h1>
          <div class="searchArea">
              <input type="text" placeholder="영화 제목을 검색해 보세요." />
              <button type="button">검색</button>
          </div>
      </div>
  </header>
  <!--// header -->

  <!-- mainBanner -->
  <section id="mainBanner">
      <div class="inner">
          <div class="movieSlideArea">
              <ul id="movieSlideList">
                  <!-- 영화 슬라이드 노출될 영역 -->
              </ul>
          </div>
      </div>
  </section>
  <!--// mainBanner -->

  <!-- mainSection -->
  <section id="mainSection">
      <div class="inner">
          <div class="movieListArea">
              <ul id="movieList">
                  <!-- 영화 리스트 노출될 영역 -->
              </ul>
          </div>
      </div>
  </section>
  <!-- mainSection -->
  ```

#### :pushpin: 3-2) `JavaScript` 코드

  ``` javascript
  const options = {
    method: 'GET',
    headers: {
      accept: 'application/json',
      Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjZmM4NDQ4MmFjMjNjOTk0Yjg0ZTc0ZTE2NzE3YTAyYSIsIm5iZiI6MTcyMTY5NTk0Ni45NDI2NjQsInN1YiI6IjY2OWVmZDkxMDEzOWZlNjM4ZmJmMjliNyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.le2OfU4Q3_drRr_CLeNggdF4WUxZxP3Q6DcPFD57M1g'
    }
  };


  fetch('https://api.themoviedb.org/3/movie/top_rated?language=en-US&page=1', options)
    // json 변환 
    .then(response => response.json())
    // json 데이터 추출
    .then(response => {
      const doc = response; // 데이터를 doc에 할당함
      const movieSlideList = document.getElementById('movieSlideList'); // 영화 슬라이드 리스트 뿌려질 영역   
      const movieList = document.getElementById('movieList'); // 영화 리스트 뿌려질 영역       

      createMovieHtml(doc); // 첫 로딩 시 전체 데이터 노출
      handleListMouseEvent(doc); // 무비 리스트 책갈피 효과 이벤트
    })
    .catch(err => console.error(err));

  // 리스트 생성 함수
  function createMovieHtml(doc) {
    let movieData = doc.results; // movieData에 movie 배열 할당  
    console.log(movieData);
    // ul#movieSlideList 에 뿌려줄 li 생성
    let movieSlideItems = movieData.map(movie =>
      `
        <li class='movieListItem'>
          <div class="tBox">
            <em>MINKYU <span>MOVIE CORRECTION</span></em>
            <strong class="tit">${movie.title}</strong>          
            <p class="info">
              <span class="date">${movie.release_date}</span>  
              <span>
                <em class="average">${Math.ceil(movie.vote_average * 10) / 10}</em>
                <em class="star">${getStarRating(movie.vote_average)}</em>
              </span>           
            </p>          
            <p class="desc">${movie.overview}</p>
            <button type="button">View</button>
          </div>  
          <div class='iBox'>
            <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
            <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
          </div>                
        </li>
      `
    ).join('');

    let movieListItems = movieData.map(movie =>
      `
        <li class='movieListItem' style="width:${100 / movieData.length + '%'}; background:url('https://image.tmdb.org/t/p/w500${movie.poster_path}') no-repeat center center; background-size:cover;">
          <div class="tBox">
            <em>MINKYU <span>MOVIE CORRECTION</span></em>
            <strong class="tit">${movie.title}</strong>          
            <p class="info">
              <span class="date">${movie.release_date}</span>  
              <span>
                <em class="average">${Math.ceil(movie.vote_average * 10) / 10}</em>
                <em class="star">${getStarRating(movie.vote_average)}</em>
              </span>            
            </p>          
            <p class="desc">${movie.overview}</p>
            <button type="button">View</button>
          </div>  
        </li>
      `
    ).join('');


    movieSlideList.innerHTML = movieSlideItems;
    movieList.innerHTML = movieListItems;
  }

  // 메인 배너 하단 책깔피 리스트 마우스 이벤트
  function handleListMouseEvent(doc) {
    if (!doc) return false;
    const movieListItem = document.querySelectorAll(".movieListItem");
    movieListItem.forEach(item => {
      item.addEventListener("mouseover", () => {
        movieListItem.forEach(all => {
          all.classList.remove('curr');
        })
        item.classList.add('curr');
      })
    })
  }

  // 영화 리스트 평점 star로 변환
  function getStarRating(vote_average) {
    let stars = '';
    // console.log(vote_average);
    if (vote_average <= 2) stars = '★☆☆☆☆';
    else if (vote_average <= 4) stars = '★★☆☆☆';
    else if (vote_average <= 6) stars = '★★★☆☆';
    else if (vote_average <= 9.5) stars = '★★★★☆';
    else stars = '★★★★★';
    return stars;
  }
  ```

### :four: 제작 과정
#### :pushpin: 4-1) 기획
- 영화 검색 사이트를 제작하는 것이 과제로 주어지고 가장 먼저 생각났던건 ***넷플릭스***였다.
  대부분의 사용자에게 익숙한 레이아웃으로, 디자인은 영화 포스터가 강렬하니까 최대한 심플하게 생각!
- 기능은 최상단 배너에 슬라이드를 구성하고, 네비게이션을 썸네일로 만들어 적용하려 했으나, 일정을 생각해서 추후 시간이 남을 때 진행하기로 결정
- 하단 섹션 또한 슬라이드로 구현하려 했으나, 지난 7조 미니 프로젝트에서 제작한걸로 재사용하기엔 너무 우려먹는 느낌이어서 다른 인터렉션 요소를 생각했다.

#### :pushpin: 4-2) 디자인
- 디자인은 가장 최상단 배너에 좌측 텍스트, 우측 이미지로 메인 컬러는 레드로 선택했다.
- 하단 섹션은 정말 고민이 많았는데, 일반적으로 리스트나 그리드형태로 뿌려주는게 정석인 느낌인데 첫 과제이기도 하고 나만의 창작물이기에 쉽게 선택하지 못했다. 첫 기획 당시에 일정상 불가능했던 썸네일 네비게이션을 생각하다 문득 책깔피 형태로 뿌려준 다음 마우스를 오버했을 때 해당 영역이 넓어지는 효과가 들어가면 재미있을 것 같아서 확정했다.

#### :pushpin: 4-2) 기능 풀이

1. TMDB 오픈 API를 이용하여 인기영화 데이터 가져오기
2. 전역 변수를 세팅<br/>
  - `const doc = response` : 영화 데이터를 변수 `doc`에 할당
  - `const movieSlideList = ..` : 영화 슬라이드 리스트 뿌려질 영역
  - `const movieList = ..` : 하단 섹션에 리스트 뿌려질 영역 

3. `createMovieHtml(doc)` : 리스트 생성 함수 생성<br/>
- `createMovieHtml(doc)` 함수의 매개변수에 `doc`을 선언
  - 전역으로 설정한 `doc`을 전달받아 리스트를 생성할 예정
- `map`메서드를 사용하여 각 위치에 뿌려줄 리스트를 생성함
- 평점같은 경우 소수점 3자리 까지 있는게 보기 안좋아서 `Math.ceil` 함수를 사용하여 소수점 첫자리만 노출되게 적용

4. `getStarRating(vote_average)` : ★★★★★ 체크하는 함수 생성
- 평점을 숫자로만 표기하니 디자인상 어색해서, 스타를 적용함
- 스타를 적용하는 함수는 `getStarRating(vote_average)`로 배개변수로 전달받은 값을 if문으로 체크하여 별을 채워줌

5. `handleListMouseEvent(doc)` : 하단 섹션 리스트 마우스 오버 이벤트<br/>
- 처음 `doc`을 매개변수로 받아 데이터를 전달받지 않으면 함수 밖으로 나가게 `if문`으로 체크 후 `return false` 해줌
- `const movieListItem` : 첫 로딩 시 데이터가 뿌려지고 난 후 리스트를 찾아야하기에 해당 함수에서 변수를 선언해줌
- `forEach`문을 사용해서 각 리스트에 마우스를 올릴 경우 마우스 올린 요소만 `curr` 클래스가 적용되게 함
- `curr` 클래스에 맞춰 css로 애니메이션 효과를 적용해줌


#### :pushpin: 적용한 코드
- **깃헙 링크**: [TopRatedMovies](https://github.com/rarrit/TIL/tree/main/Project/TopRatedMovies)
- **적용 화면**: [https://rarrit.github.io/TIL/Project/TopRatedMovies/](https://rarrit.github.io/TIL/Project/TopRatedMovies/)

### :fire: 마무리
1차 작업은 전체적인 디자인, 퍼블리싱과 데이터를 뿌려주는 것 까지 완료했다. 2차 구현 예정 사항은 아래에 리스트 형식으로 정리해봤다.<br/>
지난번 7조 미니프로젝트 작업 당시 fetch를 사용하지 않아서 조금 걱정스러웠는데, 처음 이미지 src값에서 헷갈려서 시간을 소요한거 치곤 나머지는 순조로웠다.<br/>
하나 아쉬운건 FireBase을 통해 직접 데이터를 추가해서 작업한 것이 아니여서 자유도가 떨어지는 점인 것 같다.<br/>
그리고 항상 `for`문을 통해 동적으로 요소를 생성했는데, 다른 방법이 없나 궁금해서 검색해보니 `forEach`문과 `map`메서드를 사용한 방법이 있어 공부했다.<br/>
역시 문제를 해결할 수 있는 방법은 정말 다양하다. 내가 모를 뿐.. 열심히 공부 해야겠다!

#### :pushpin: 완료한 사항
1. [1] jQuery 라이브러리 사용없이 순수 바닐라 자바스크립트 사용하기 (진행중)
2. [2] TMDB 오픈 API를 이용하여 인기영화 데이터 가져오기
3. [3] 영화정보 카드 리스트 UI 구현
  - TMDB에서 받아온 데이터를 브라우저 화면에 카드 형태의 데이터로 보여줍니다.
  - 카드에는 title(제목), overview(내용 요약), poster_path(포스터 이미지 경로), vote_average(평점) 이렇게 4가지 정보가 필수로 들어갑니다.
4. [5] 아래 기재된 Javascript 문법 요소를 이용하여 구현
  - const와 let만을 이용한 변수 선언 필수
  - 화살표 함수 : 아래 예시 중 1개 이상 사용
  - DOM 제어하기 : 아래 api 목록 중 2개 이상 사용하기

#### :pushpin: 적용 예정
1. [4] 영화 검색 UI 구현
  - API로 받아온 전체 영화들 중 영화 제목에 input 창에 입력한 문자값이 포함되는 영화들만 화면에 보이도록 합니다. 
  - 입력 후 검색버튼 클릭 시 실행되도록 합니다.
2. [5] 아래 기재된 Javascript 문법 요소를 이용하여 구현
  - 배열 메소드 : 하기 예시 중 2개 이상 사용
3. [선택사항] 웹사이트 랜딩 또는 새로고침 후 검색 입력란에 커서 자동 위치시키기
4. [선택사항] 대소문자 관계없이 검색 가능하게 하기
5. [선택사항] 키보드 enter키를 입력해도 검색버튼 클릭한 것과 동일하게 검색 실행시키기
6. [선택사항] 최상단 배너 슬라이드 적용
7. [선택사항] 검색결과에 따라 표시되는 부분 고민 후 반영
