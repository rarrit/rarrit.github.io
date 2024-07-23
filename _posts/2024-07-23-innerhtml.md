---
title: "JavaScript 반복문을 사용하여 HTML 추가하는 방법"
date: 2024-07-23
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - js
  - til
tags:
  - innerHTML
  - for
  - forEach
  - map
author_profile: true
sidebar_main: true
---

## :ledger: JavaScript 반복문을 사용하여 동적으로 요소 생성하기

스파르타 코딩클럭 리엑트 6기 1주차, 2주차 과제를 진행하며 API를 전달받아 리스트를 생성하는 작업이 필수였다.<br/>
개인적으로 많이 사용해보지 않았고 for반복문 말고 다른 방법이 있는지 찾아보고 정리해봤다.

### :one: `for` 문을 사용한 방법

#### :pushpin: `createElement`, `AppendChild` 사용한 방법

먼저 이 방법을 가장 많이 사용했었고 사람들에게 가장 익숙한 방법이라 생각한다.

```javascript
// 배열 저장
const items = ["Item 1", "Item 2", "Item 3"];
// 뿌려지는 리스트를 담아줄 요소 선택
const box = document.getElementById("box");
// 배열만큼 반복하여 요소에 리스트를 담아줌
for (let i = 0; i < items.length; i++) {
  // 반복문이 순회할 때마다 뿌려줄 요소 생성함
  const item = document.createElement("li");
  // 생성된 요소에 값을 넣음
  item.innerHTML = items[i];
  // box에 담아줌
  box.appendChild(item);
}
```

#### :Pushpin: `백틱`을 사용한 방법

백틱을 사용하면 가독성도 좋고 데이터를 담아주기 편해서 이 방법을 더 선호했다.<br/>
마침 개인 과제에서 사용했던 코드로 작성해봤다.

```javascript
// 함수로 생성함 (백틱 사용과는 무관)
function createMovieHtml(doc) {
  // 매개변수로 전달받음
  let movieData = doc.results; // 매개변수로 전달받은 값을 movieData에 할당함
  let movieListItems = ""; // 뿌려줄 요소 초기화

  for (let i = 0; i <= movieData.length; i++) {
    movieListItems += `
      <li class='movieListItem'>
        <img src='https://image.tmdb.org/t/p/w500${movieData[i].poster_path}' />
        <strong>${movieData[i].title}</strong>
        <p>${movieData[i].overview}</p>
        <span>${movieData[i].vote_average}</span> // 이렇게 value 값이 많을 경우 백틱을 사용하기가 가독성이 좋다.
      </li>
    `;
  }
  movieList.innerHTML = movieListItems;
}
```

### :two: `forEach` 메서드를 사용한 방법

#### :pushpin: `createElement`, `AppendChild` 사용한 방법

백틱을 사용하지 않으면 필요한 element 요소를 추가해줘야하고, 값도 할당해야 하기에 보여줘야 할 데이터가 많아질수록 유지보수하기 어려운 것 같다.

```javascript
function createMovieHtml(doc) {
  let movieData = doc.results; // 매개변수로 전달받은 값을 movieData에 할당함
  const movieList = document.getElementById("movieList"); // 영화 리스트를 담을 요소 선택

  // 기존의 콘텐츠를 비우기 (옵션)
  movieList.innerHTML = "";

  // forEach를 사용하여 movieData 배열의 각 요소를 순회함
  movieData.forEach((movie) => {
    // <li> 요소 생성
    const listItem = document.createElement("li");
    listItem.className = "movieListItem"; // 클래스 추가

    // <img> 요소 생성 및 속성 설정
    const img = document.createElement("img");
    img.src = "https://image.tmdb.org/t/p/w500" + movie.poster_path;
    img.alt = movie.title; // 접근성을 위한 alt 속성 추가

    // <strong> 요소 생성 및 텍스트 설정
    const strong = document.createElement("strong");
    strong.textContent = movie.title;

    // <p> 요소 생성 및 텍스트 설정
    const p = document.createElement("p");
    p.textContent = movie.overview;

    // <span> 요소 생성 및 텍스트 설정
    const span = document.createElement("span");
    span.textContent = movie.vote_average;

    // 모든 요소를 <li> 요소에 추가
    listItem.appendChild(img);
    listItem.appendChild(strong);
    listItem.appendChild(p);
    listItem.appendChild(span);

    // <li> 요소를 movieList에 추가
    movieList.appendChild(listItem);
  });
}
```

#### :pushpin: `백틱`을 사용한 방법

확실히 백틱이 가독성및 유지보수가 용이한 것 같고 for문과 비교하자면 식이 줄어들어 조금 더 간결한 것 같다.

```javascript
function createMovieHtml(doc) {
  let movieData = doc.results; // 매개변수로 전달받은 값을 movieData에 할당함
  let movieListItems = ""; // 뿌려줄 요소 초기화

  // forEach를 사용하여 movieData 배열의 각 요소를 순회함
  movieData.forEach((movie) => {
    movieListItems += `
      <li class='movieListItem'>
        <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
        <strong>${movie.title}</strong>
        <p>${movie.overview}</p>
        <span>${movie.vote_average}</span>
      </li>
    `;
  });

  const movieList = document.getElementById("movieList");
  movieList.innerHTML = movieListItems;
}
```

### :three: `map` 메서드를 사용한 방법

`map` 메서드를 사용하여 배열의 각 요소를 HTML 문자열로 변환한 후, 한번에 `innerHTML`로 추가하는데, `join('')`을 사용하지 않으면 배열로 보여지나 값은 잘 들어가진다. 허나 찾아보니 문자열로 변환을 하는게 좋다고 하는데, 문자열로 변환하면 또 보안에서 취약할 수 있다하고 어떻게 사용하는게 가장 좋은건지 사실 어렵게 느껴진다.

```javascript
function createMovieHtml(doc) {
  let movieData = doc.results; // movieData에 movie 배열 할당
  let movieListItems = movieData
    .map(
      (movie) =>
        `
      <li class='movieListItem'>
        <img src='https://image.tmdb.org/t/p/w500${movie.poster_path}' />
        <strong>${movie.title}</strong>
        <p>${movie.overview}</p>
        <span>${movie.vote_average}</span>
      </li>
    `
    )
    .join("");
  movieList.innerHTML = movieListItems;
}
```

### :fire: 마무리

항상 `for` 문으로 사용해서 동적으로 요소를 생성했는데, `forEach`와 `map`을 통해 적용하는 방법을 배워봤다.<br/>
이번 내용을 찾아보면서 `DocumentFragment`와 `insertAdjacentHTML` 를 사용한 방법도 있었는데, 일단 가장 익숙한 방법부터 정리해봤으며, 추후 성능면에서도 효율적인 방법이 무엇인지 정리해봐야겠다고 생각했다.<br/><br/>
방법은 늘 항상 많으며, 단지 나만 몰랐을 뿐...! 공부하는 사람들 모두 파이팅이다..!
