---
title: "JavaScript 영화 검색 사이트 제작 및 풀이 과정 (2차)"
date: 2024-07-25
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - js
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
 ┃ ┃ ┣ 📜slide.css<br/>
 ┃ ┃ ┗ 📜style.css<br/>
 ┃ ┣ 📂js<br/>
 ┃ ┃ ┗ 📜api.js<br/>
 ┃ ┗ 📂scss<br/>
 ┣ 📜README.md<br/>
 ┗ 📜index.html

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
          handleMovieRender(movies); // 첫 로딩 시 전체 데이터 노출
          handleListMouseEvent(); // 무비 리스트 책갈피 효과 이벤트        
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
            handleListMouseEvent(); // 무비 리스트 책갈피 효과 이벤트 
            if (filterMovies.length === 1) { // 영화가 한 개일 때
              document.getElementById('movieList').appendChild.remove;
            }
          } else {
            alert('검색한 영화가 없습니다.');
            window.location.reload;
          }
        }
      })
      .catch(err => console.error(err));
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

### :four: 제작 과정
#### :pushpin: 4-1) 변경 및 추가 내용

1. API KEY,URL 값을 `const` 로 전역 설정해주었다.
2. 1차에서 화면에 출력 후 검색 기능을 적용하던 중 검색한 내용에 맞춰 필터링 후 출력해야 하기에 `fetchMovie` 함수를 생성 후 매개변수로 `inputVal(검색한 값)` 을 전달받아 값이 없을 경우 전체 데이터를 노출시키고 값이 있을경우 `filter`메서드를 사용해서 값이 들어가 있는 데이터를 배열로 담아서 `handleMovieRender`함수의 매개변수에 전달하여 화면에 출력해주었다.
3. `handleMovieRender`함수도 이번 2차에서 생성된 함수인데, 기존에 `createMovieHtml` 함수 내에서 처리할 수 있었지만, 추후 슬라이드 기능까지 고려를 했던터라 좀 더 유지보수가 쉽게 따로 분리를 해주었다.
4. `hanldeMovieFindRender`함수는 input의 value값을 어떻게 전달해줄 까 고민하다 이벤트내에서 매개변수로 `inputVal`을 전달하기 위해 생성했다.  


#### :pushpin: 적용한 코드
- **깃헙 링크**: [TopRatedMovies](https://github.com/rarrit/TIL/tree/main/Project/TopRatedMovies)
- **적용 화면**: [https://rarrit.github.io/TIL/Project/TopRatedMovies/](https://rarrit.github.io/TIL/Project/TopRatedMovies/)

### :fire: 마무리
1차에서 기획,디자인,퍼블을 진행 후 화면에 출력하면서 코드가 단순했지만, 2차를 진행하면서 검색 기능이 추가되어 대부분의 핵심 코드가 변경되었다.<br/><br/>
진행하면서 막혔던 부분이 어떻게 input의 value값을 전달하고 그 함수는 movie 데이터를 전달받는지, 그렇다면 전달 후에 어떻게 검색한 데이터를 가져와서 화면에 출력하는지 산넘어 산이었다. 고민이 길어지던 중 튜터님께 질문을 하게 되었고, 튜터님이 친절하게 알려주었지만 자리로 돌아왔을 때 사실 기억이 많이 않났엇다. 그러나 추천해줬던 부분이 기능을 만들기 전에 순서에 대해 작성을 한번 해보고 진행해보라하셨는데 정말 크게 도움이 되었다.<br/><br/>
스파게티 마스터로서 은퇴를 위해 앞으로는 기능을 만들기 전에 충분히 고민해보고 진행해봐야겠다.

#### :pushpin: 추가 적용 예정
- [선택사항] 최상단 배너 슬라이드 적용
