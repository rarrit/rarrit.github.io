---
title: "JavaScript 영화 검색 사이트 제작 및 풀이 과정 (3차)"
date: 2024-07-26
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
2. 1차에서 적용한 사항
  1. [1] jQuery 라이브러리 사용없이 순수 바닐라 자바스크립트 사용하기 (진행중)
  2. [2] TMDB 오픈 API를 이용하여 인기영화 데이터 가져오기
  3. [3] 영화정보 카드 리스트 UI 구현
    - TMDB에서 받아온 데이터를 브라우저 화면에 카드 형태의 데이터로 보여줍니다.
    - 카드에는 title(제목), overview(내용 요약), poster_path(포스터 이미지 경로), vote_average(평점) 이렇게 4가지 정보가 필수로 들어갑니다.
  4. [5] 아래 기재된 Javascript 문법 요소를 이용하여 구현
    - const와 let만을 이용한 변수 선언 필수
    - 화살표 함수 : 아래 예시 중 1개 이상 사용
    - DOM 제어하기 : 아래 api 목록 중 2개 이상 사용하기
3. 2차에서 적용한 사항
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

### :two: 사용 언어 및 폴더 구조
- HTML5, CSS3, JavaScript, fetch
- 📦TopRatedMovies<br/>
 ┣ 📂assets<br/>
 ┃ ┣ 📂css<br/>
 ┃ ┃ ┣ 📜common.css<br/>
 ┃ ┃ ┣ 📜layout.css<br/>
 ┃ ┃ ┣ 📜main.css<br/>
 ┃ ┃ ┣ 📜slide.css(추가)<br/>
 ┃ ┃ ┗ 📜style.css<br/>
 ┃ ┣ 📂images(추가)<br/>
 ┃ ┃ ┗ 📜btn-slide.png<br/>
 ┃ ┣ 📂js<br/>
 ┃ ┃ ┣ 📜api.js<br/>
 ┃ ┃ ┗ 📜slide.js(추가)<br/>
 ┃ ┗ 📂scss<br/>
 ┣ 📜README.md<br/>
 ┗ 📜index.html

### :three: 작성 코드
#### :pushpin: 3-1) `HTML` 코드
1~2차에서 추가된 HTML 코드는 Slide의 dot(navigation), prev,next 버튼이다.

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
      <div id="movieBullet">
        <!-- bullet 노출될 영역 -->
      </div>
      <div id="btnArea">
          <button type="button" id="btnPrev"></button>
          <button type="button" id="btnNext"></button>
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

#### :pushpin: 3-2) `JavaScript` api.js 코드

  ``` javascript
  /*
  1. 첫 로딩 시 API KEY,URL 값을 전달받아 리스트를 화면에 출력한다.
  2. 검색영역에 값을 입력후 검색한다. ==>> input의 keyup 이벤트, e.keyCode == 13(엔터), button의 click이벤트
    2-1. 검색 할 때 값을 받아야함 ==>> input.value
    2-2. 검색한 값이 json으로 전달받은 데이터가 있는지 없는지 판별해야한다. ==>> [?]그렇다면 그 데이터는 어떻게 받을건가?    
      2-2-1. 있을 경우 해당 데이터를 화면에 출력한다. ==>> handleMovieRender 함수 사용
        2-2-1-1. 만약 데이터 값이 1개면 아래의 리스트는 삭제 후 최상단에만 노출한다. ==>> handleMovieRender 사용 및 DOM 조작
      2-2-2. 없을 경우 "검색결과가 없습니다." 경고창 노출 후 새로고침하여 전체 리스트를 다시 출력한다. ==>> window.reload 및 fetchMovies 실행
  */

  // API KEY 값 저장 : 변경되면 않되는 값
  const API_KEY = 'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjZmM4NDQ4MmFjMjNjOTk0Yjg0ZTc0ZTE2NzE3YTAyYSIsIm5iZiI6MTcyMTY5NTk0Ni45NDI2NjQsInN1YiI6IjY2OWVmZDkxMDEzOWZlNjM4ZmJmMjliNyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.le2OfU4Q3_drRr_CLeNggdF4WUxZxP3Q6DcPFD57M1g';
  // API URL 값 저장 : 변경되면 않되는 값
  const API_URL = 'https://api.themoviedb.org/3/movie/top_rated?language=en-US&page=1';

  const options = {
    method: 'GET',
    headers: {
      accept: 'application/json',
      Authorization: `Bearer ${API_KEY}`
    }
  };

  // DOM이 로드되면 실행
  document.addEventListener('DOMContentLoaded', () => {
    fetchMovies();
    handleMovieSearch();
  });

  /*
  ** [fetchMovies] 함수 
  ** 처음 함수로 생성한 이유는 DOM로드되면 실행되게 끔 했는데, 이후 검색기능을 고민하면서 김준현 튜터님께 물어봤다.
  ** 답변 내용으로는 검색 함수에서도 fetch를 다시 불러 올 수 있지만, 현재 생성된 fetchMovies 함수의 파라미터에
  ** 값을 추가하여 검색함수가 실행될 때 input의 value값을 전달받아 fetchMovies에서 로직을 구현할 수 있는것!
  ** 안물어봤으면 또 스파게티를 만들었을 것 같다.
  */
  function fetchMovies(searchVal) {
    fetch(API_URL, options)
      .then(response => response.json()) // json 변환 
      .then(response => { // json 데이터 추출
        const movies = response.results; // 데이터를 doc에 할당함

        // 검색 값이 있는지 없는지 판별하여 리스트를 출력함
        if (searchVal === '' || searchVal === null || searchVal === undefined) {
          slideReset();
          document.getElementById('btnArea').style.display = 'block';
          handleMovieRender(movies); // 첫 로딩 시 전체 데이터 노출
          handleListMouseEvent(); // 무비 리스트 책갈피 효과 이벤트                        
          handleSlideStart(movies);
        } else {
          // [filter - 실행순서]
          // 1. movie.title.toLowerCase() : 대소문자를 구분하지 않고 검색어를 비교하기위해 타이틀을 소문자로 변경해줌
          // 2. .includes(searchVal.toLowerCase()) : 그리고 소문자로 변경한 값에 검색한 글자가 포함되어 있는지 판별함
          // 3. 즉 소문자로 변경한 검색어가 소문자로 변경한 movie.title에 있는지 확인해서 참이면 배열을 반환해줌
          // includes ==>> 해당 메서드는 문자열이 포함되어 있는지를 확인해줌
          // toLowerCase ==>> 문자열을 소문자로 변환, 반대는 toUpperCase        
          const filterMovies = movies.filter(movie => movie.title.toLowerCase().includes(searchVal.toLowerCase()));
          if (filterMovies.length > 0) { // 검색어가 포함된 movie 데이터가 있는지 확인
            handleMovieRender(filterMovies); // 해당 데이터 값으로 동적으로 화면에 출력
            document.getElementById('btnArea').style.display = 'block';
            handleListMouseEvent(); // 무비 리스트 책갈피 효과 이벤트 

            if (filterMovies.length === 1) { // 영화가 한 개일 때
              slideReset();
              document.getElementById('movieList').innerHTML = '';
              document.getElementById('movieBullet').innerHTML = '';
              document.getElementById('btnArea').style.display = 'none';
            } else {
              slideReset();
              handleSlideStart(filterMovies);
            }
          } else {
            alert('검색한 영화가 없습니다.');
            window.location.reload;
          }
        }
      })
    // .catch(err => console.error(err));
  }

  /*
  ** [handleMovieSearch] 함수
  ** 글을 작성 후 엔터 하는 기능과 버튼을 클릭한 기능을 적용
  ** 여기서 작성된 input의 value값을 handleMovieFindRender함수의 파라미터로 값을 넘겨 준다.
  */
  function handleMovieSearch() {
    const searchInput = document.getElementById('searchInp');
    const searchBtn = document.getElementById('searchBtn');

    searchInput.addEventListener('keyup', (e) => {
      if (e.keyCode === 13) {
        // 엔터했을 때
        e.preventDefault();
        handleMovieFindRender(searchInput.value);
      }
    })

    searchBtn.addEventListener('click', () => {
      handleMovieFindRender(searchInput.value);
    })
  }

  /*
  ** [handleMovieFindRender] 함수
  ** handleMovieSearch 함수에서 전달받은 input value를 fetchMovies로 전달 함
  */
  function handleMovieFindRender(inputVal) {
    fetchMovies(inputVal); // 값 전달
  }

  /*
  ** [handleMovieRender] 함수
  ** 기존 createMovieHtml 로 fetch내에서 바로 동적으로 요소를 생성해서 화면에 노출시켰는데, 검색기능이 생기면서
  ** 추가된 함수이다. 이유는 첫 랜더 후 검색할 때 마다 동적으로 요소를 화면에 보여줘야하는데, 초기화 기능이 없어서 변경하게되었다.
  */
  function handleMovieRender(movies) {
    const movieSlideList = document.getElementById('movieSlideList'); // 영화 슬라이드 리스트 뿌려질 영역   
    const movieList = document.getElementById('movieList'); // 영화 리스트 뿌려질 영역       

    // 리스트 초기화
    movieSlideList.appendChild.remove;
    movieList.appendChild.remove;

    // 리스트 생성
    movieSlideList.innerHTML = movies.map(createMovieSlideHtml).join('');
    movieList.innerHTML = movies.map((movie, index, array) => createMovieHtml(movie, array.length)).join('');
  }

  /*
  ** [createMovieSlideHtml] 함수
  ** handleMovieRender 함수를 생성하며 분리해줬다.
  ** 처음 한개의 함수내에서 두개의 HTML을 전달하는 것이 복잡해질 것 같아서 이후 슬라이드를 적용할 때, 크게 느껴질 것 같음
  */
  function createMovieSlideHtml(movie) {
    return `
      <li class='movieListSlideItem'>
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
          <button type="button" onclick="alert('이 영화의 id 값은 => ${movie.id} 입니다.')">View</button>
        </div>  
        <div class='iBox'>
          <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
          <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
        </div>                
      </li>
    `
  }

  /*
  ** [createMovieHtml] 함수
  ** handleMovieRender 함수를 생성하며 분리해줬다.
  ** 처음 한개의 함수내에서 두개의 HTML을 전달하는 것이 복잡해질 것 같아서 이후 슬라이드를 적용할 때, 크게 느껴질 것 같음
  */
  function createMovieHtml(movie, totalMovies) {
    // console.log(movie.length); 길이가 안나옴; render 함수에서 기존 원본 배열을 받아서 파라미터로 length 전달해줌
    return `
      <li class='movieListItem' style="width:${100 / totalMovies + '%'}; background:url('https://image.tmdb.org/t/p/w500${movie.poster_path}') no-repeat center center; background-size:cover;">
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
          <button type="button" onclick="alert('이 영화의 id 값은 => ${movie.id} 입니다.')">View</button>
        </div>  
      </li>
    `
  }

  // [handleListMouseEvent] 함수 : 두 번째 리스트 마우스 오버 이벤트 함수이다.
  function handleListMouseEvent() {
    const movieListItem = document.querySelectorAll('.movieListItem');
    movieListItem.forEach(item => {
      item.addEventListener("mouseover", () => {
        movieListItem.forEach(all => all.classList.remove('curr'));
        item.classList.add('curr');
      })
      item.addEventListener("mouseout", () => {
        movieListItem.forEach(all => all.classList.remove('curr'));
      })
    })
  }

  // [getStarRating] 함수 : 평점 체크 및 변환 함수
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

#### :pushpin: 3-3) `JavaScript` slide.js 코드

  ```javascript
  let slideWid = 100; // 슬라이드 넓이 값
  let slideCurrentIdx = 0; // 슬라이드 현재 인덱스 값
  let slideMoveDelay = 3000; // 슬라이드 돌아가는 속도
  let slideMoveTimer; // interval 초기화
  let isDragChk = false; // [drag] 초기화
  let downClientX; // [drag] 마우스 클릭했을 때 포인터 위치
  let upClientX; // [drag] 마우스 땟을 때 포인터 위치      
  let isSlide;

  // 초기화에서 사용할 수 있게 함수도 초기화해준다.
  let carouselPrevMove;
  let carouselNextMove;
  let carouselMouseDown;
  let carouselMouseUp;

  function handleSlideStart(movies) {
    if (!movies || movies.length < 2) {
      console.log("응 2개 미만")
    } else {
      console.log("응 3개 이상")
    }
    const carouselSlide = document.getElementById("movieSlideList");
    const carouselSlideItems = document.querySelectorAll("#movieSlideList > li");
    const carouselBullet = document.getElementById("movieBullet");
    const carouselBulletItems = document.querySelectorAll("#movieBullet > span");
    const carouselBtnPrev = document.getElementById("btnPrev");
    const carouselBtnNext = document.getElementById("btnNext");

    // 슬라이드 리스트 초기 넓이 세팅
    slideWid = carouselSlide.offsetWidth;
    carouselSlideItems.forEach(item => item.style.width = `${slideWid}px`);

    // 슬라이드 생성
    function createSlides(movies) {
      let bullets = movies.map((item, index) => `
        <span class='bulletItem ${index === 0 ? "curr" : ""}'></span>
      `).join('');

      carouselBullet.innerHTML = bullets;
    };

    // #02. 슬라이드가 시작되는 함수, 가독성이 좋게 함수명 변경함
    function startSlideMove() {
      slideMoveTimer = setInterval(() => {
        slideCurrentIdx = (slideCurrentIdx + 1) % carouselSlideItems.length;
        updateSlideTransform();
        updateThumbActive();
      }, slideMoveDelay);
    }

    // #03. 슬라이드 인덱스에 따라 슬라이드의 위치가 이동되어야 하기 때문에 함수로 생성함
    function updateSlideTransform() {
      carouselSlide.style.transform = `translate3d(-${slideCurrentIdx * slideWid}px, 0px, 0px)`;
    }

    // #04. 슬라이드 썸네일 엑티브 또한 슬라이드의 인덱스에 맞춰 엑티브가 되어야하기 때문에 함수로 생성함
    function updateThumbActive() {
      document.querySelectorAll("#movieBullet > span").forEach((thumb, idx) => thumb.classList.toggle("curr", idx === slideCurrentIdx));
    }

    // #05. 슬라이드의 롤링되는 기능을 멈추는것 또한 자주 사용되어 함수로 생성함
    function stopSlideMove() {
      clearInterval(slideMoveTimer);
    }

    // #06. 슬라이드 왼쪽으로 이동 함수
    carouselPrevMove = function () {
      stopSlideMove(); // 자동 슬라이드 종료
      slideCurrentIdx = (slideCurrentIdx === 0) ? carouselSlideItems.length - 1 : slideCurrentIdx - 1;
      updateSlideTransform(); // 슬라이드 인덱스에 맞춰 이동
      updateThumbActive(); // 슬라이드 인덱스에 맞춰 썸네일 엑티브
      startSlideMove(); // 자동 슬라이드 시작
    }

    // #06. 슬라이드 오른쪽으로 이동 함수
    carouselNextMove = function () {
      stopSlideMove();
      slideCurrentIdx = (slideCurrentIdx === carouselSlideItems.length - 1) ? 0 : slideCurrentIdx + 1;
      updateSlideTransform();
      updateThumbActive();
      startSlideMove();
    }

    // #07. 슬라이드 마우스 클릭했을 때 함수
    carouselMouseDown = function (e) {
      isDragChk = true;
      downClientX = e.clientX;
    }

    // #08. 슬라이드 마우스 클릭하고 놓았을 때 함수
    carouselMouseUp = function (e) {
      isDragChk = false;
      upClientX = e.clientX;
      downClientX > upClientX ? carouselNextMove() : carouselPrevMove();
      updateThumbActive();
    }

    // #07. 슬라이드 썸네일 클릭 함수
    function handleThumbnailClick(idx) {
      return function () {
        stopSlideMove();
        slideCurrentIdx = idx;
        updateSlideTransform();
        updateThumbActive();
        startSlideMove();
      };
    }

    carouselBulletItems.forEach((item, idx) => {
      item.addEventListener("click", handleThumbnailClick(idx));
    });

    createSlides(movies);
    startSlideMove();
    carouselBtnPrev.addEventListener("click", carouselPrevMove);
    carouselBtnNext.addEventListener("click", carouselNextMove);
    carouselSlide.addEventListener("mousedown", carouselMouseDown);
    carouselSlide.addEventListener("mouseup", carouselMouseUp);
  }
  // 슬라이드 초기화
  function slideReset() {
    console.log('초기화');

    isSlide = false;
    slideCurrentIdx = 0;
    clearInterval(slideMoveTimer);

    const carouselSlide = document.getElementById("movieSlideList");
    const carouselBtnPrev = document.getElementById("btnPrev");
    const carouselBtnNext = document.getElementById("btnNext");

    document.getElementById("movieBullet").innerHTML = '';
    carouselSlide.style.transform = `translate3d(0px, 0px, 0px)`;

    if (carouselBtnPrev) carouselBtnPrev.removeEventListener("click", carouselPrevMove);
    if (carouselBtnNext) carouselBtnNext.removeEventListener("click", carouselNextMove);
    if (carouselSlide) {
      carouselSlide.removeEventListener("mousedown", carouselMouseDown);
      carouselSlide.removeEventListener("mouseup", carouselMouseUp);
    }
  }
  ```

### :four: 제작 과정
#### :pushpin: 4-1) 변경 및 추가 내용
3차 제작을 진행하며, 이전에 만들었던 슬라이드를 적용해보려고 노렸을 했다. 변경 및 추가 내용은 아래와 같다.
1. 먼저 `api.js` 에서는 검색한 값을 `fiter`메서드를 통해 if문으로 데이터가 1개일 때와 2개 이상일 때를 따로 분리해서 기능을 추가해줬다.
2. 그리고 `slide.js`에서 기존에 만들었던 슬라이드 함수를 가져와서 사용했는데, 정~말 어려웠다. 왜냐하면 슬라이드 함수는 데이터 값을 고려하지 않고 만들어서 초기화 함수도 없었고 함수내에 있는 함수가 전역으로 사용되지 못해서 값을 넘겨야할 함수는 전역으로 선언 후 `handleSlideStart`함수에서 함수 표현식으로 작성했다. 이 과정이 글로 쓰니 짧았지만 문제를 해결하는 방법을 몰랐어서 `movies`의 매개변수를 전달받아 함수내에서 처리해보고 `fetch`내에서 if문으로 처리했는데 그 과정에서 너무 오랜 시간이 걸렸다.
3. [2]번 과정을 조금 더 자세하게 생각해보면, 첫 로딩 시 실행되는 `handleSlideStart`의 함수 내에서 마우스 이벤트 관련 함수들이 있어서, fetch를 통해 데이터를 다시 전달받고 슬라이드를 초기화 해줘도 이벤트는 실행이 되었었다. 그렇기에 슬라이드가 1개여도 마우스 드래그, 클릭 기능이 실행되어 전역으로 설정 후 `slideReset` 함수에서 `removeEventListener`를 사용해 이벤트를 중지해주었다.

### :fire: 마무리
이번 슬라이드 적용까지 개인적으로 목표했던 기능은 전부 적용했으며, 그 과정에서 개선해야될 부분과 소감을 작성해보았다.

#### :pushpin: 개선할점
- 아직 데이터를 전달받는 걸 고려해서 기능을 만들어야함
  - 1차 완료 후 검색 기능을 만들 때 코드가 정말 많이 변경이 되었다. 검색어 value값을 fetch 함수 매개변수로 전달받아 처리하는 과정도 그렇고, 기존에 만들었던 함수에서 처리된 값을 어떻게 전달하는지 고민하는 과정 등 처음 기능을 설계하는 과정이 얼마나 중요한지 많이 느꼈다.

- 문제 해결과정에 있어서 차근차근 해결해 나가지 못했던 점
  - 슬라이드 함수를 가져와서 붙여보았을 때, 데이터 전달받는걸 고려하지 못하고 만들었다보니 문제가 정말 많았다. 특히 초기화 후 이벤트 리스너가 실행되는 부분에서 이 이벤트가 왜 실행이 되는지를 찾아야했는데, if문을 통해 실행을 못하게 하는 방법을 더 찾았었다. 문제가 생길 경우 그 문제를 정확히 알아보고 해결하는 방법을 길러야 겠다.

#### :pushpin: 소감
- 기능 개발을 하기 전 충분히 생각하고 설계한 다음 진행해야겠다. (일단 시작 후 고쳐란 마인드는 앞으로 자제해야될 것..)
- 문제 해결에 있어서 문제에 대해 정확히 파악하는 습관을 들이자. (그 문제를 피하고 다른 방법으로 문제를 덮는것도 자제..!)

#### :pushpin: 추가 적용 예정
- [선택사항] 최상단 배너 슬라이드 적용

#### :pushpin: 링크
- **깃헙 링크**: [TopRatedMovies](https://github.com/rarrit/TIL/tree/main/Project/TopRatedMovies)
- **적용 화면**: [https://rarrit.github.io/TIL/Project/TopRatedMovies/](https://rarrit.github.io/TIL/Project/TopRatedMovies/)

#### :pushpin: JavaScript 영화 검색 사이트 제작 및 풀이 과정 글
- [JavaScript 영화 검색 사이트 제작 및 풀이 과정 (1차)](https://rarrit.github.io/til/js/api/mini/movie-correction01/)
- [JavaScript 영화 검색 사이트 제작 및 풀이 과정 (2차)](https://rarrit.github.io/til/js/api/mini/movie-correction02/)
- [JavaScript 영화 검색 사이트 제작 및 풀이 과정 (3차)](https://rarrit.github.io/til/js/api/mini/movie-correction03/)