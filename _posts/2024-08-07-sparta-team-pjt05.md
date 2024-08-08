---
title: "JavaScript 영화 검색 사이트 팀 프로젝트 (5차)"
date: 2024-08-07
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
4차 작업은 이전 3차 작업에서 db의 저장한 값과 로그인 되어있는 id를 비교하여, 마이페이지에서 좋아요 선택한 리스트를 출력하는 기능을 완성했다.

- ***목표***
  - 메인 페이지
    - `언어별 데이터 출력 (8/1 완료)`
    - `즐겨찾기(좋아요) 기능 (8/5 완료)`
      - `즐겨찾기 클릭 시 마이페이지에 추가 (8/5 완료)`
  - `로그인 (8/2 추가 및 완료)`    
  - `회원가입 (8/2 추가 및 완료)`    
  - `로그아웃 (8/2 추가 및 완료)`    
  - 마이 페이지
    - `즐겨찾기(좋아요) 리스트 페이지 (8/6 완료)`
      - `저장된 시간 반영하여 몇분전,몇시간전 기능 표시 (8/7 완료)`

### :three: 적용 목표
- 1) 마이페이지의 좋아요 리스트 저장 시간 반영
- 2) 전체 페이지 css 수정 

### :four: 기능 설명
#### :pushpin: 4-1) 좋아요 클릭 시 DB의 저장 및 업데이트
- `getTime()`
  - 리스트에서 좋아요 버튼을 클릭 시 해당 시간을 마이페이지의 리스트에 노출시키기 위해 `Date`객체를 사용했다.
    - `year`: getFullYear()를 사용하여 연도를 할당함
    - `month`: getMonth()를 사용하여 월을 할당함
      - 여기서 월은 0부터 11까지의 값으로 반환되기 때문에, 1을 더해 사람들에게 익숙한 1부터 12까지의 월로 만듬
    - `day`: getDate()를 사용하여 일을 할당함
    - `hours`: getHours()를 사용하여 시를 할당함
    - `minutes`: getMinutes()를 사용하여 분을 할당함
    - `seconds`: getSeconds()를 사용하여 초를 할당함
    - `formattedDate`: 템플릿 리터럴을 사용하여 월,일,시,분,초 순서로 배치 후 중간에 구분기호 -,:를 넣어 포맷팅된 날짜와 시간 문자열을 만든 후 리턴함

- `handleLikeAdd()`
  - 기존에 좋아요를 클릭 시 DB에 저장한 함수에 `movieLikeTime` 변수에 현재 시간을 저장한 함수를 할당했다.
  - 이후 현재 시간의 값이 DB에 있을 경우 없을 경우를 판별하여 값을 저장 및 업데이트를 해주었다.
    - 값이 없을 경우 `const docs` 즉, DB에 저장할 변수에  `movie_like_time: movieLikeTime,`를 담아주어 `addDoc`을 통해 DB에 저장
    - 값이 있을 경우 `await updateDoc(docRef, { like: newLikeState, movie_like_time: movieLikeTime });` 통해 현재 시간을 업데이트 해주었다.
```javascript
// like.js

// 현재 시간 구하는 함수
function getTime() {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = currentDate.getMonth() + 1;
  const day = currentDate.getDate();
  const hours = currentDate.getHours();
  const minutes = currentDate.getMinutes();
  const seconds = currentDate.getSeconds();
  const formattedDate = `
    ${year}
    - ${String(month).padStart(2, '0')}
    - ${String(day).padStart(2, '0')} 
    ${String(hours).padStart(2, '0')}
    : ${String(minutes).padStart(2, '0')} 
    : ${String(seconds).padStart(2, '0')}
  `;
  return formattedDate;
}

// DB에 저장하는 함수
async function handleLikeAdd({ id, img, title, overview }) {
  const loginId = sessionStorage.getItem("userLoginId");
  const movieLikeTime = getTime();

  try {
    const likeQuery = query(
      collection(db, "like"),
      where("user_id", "==", loginId),
      where("movie_id", "==", id)
    );

    const likeQueryStart = await getDocs(likeQuery);

    if (likeQueryStart.empty) {
      const newDoc = {
        user_id: loginId,
        movie_id: id,
        movie_img: img,
        movie_title: title,
        movie_over_view: overview,
        movie_like_time: movieLikeTime,
        like: true
      };
      await addDoc(collection(db, "like"), newDoc);
      return;
    }

    likeQueryStart.forEach(async (likeDoc) => {
      const likeData = likeDoc.data();
      const newLikeState = !likeData.like;

      const docRef = doc(db, "like", likeDoc.id);
      await updateDoc(docRef, { like: newLikeState, movie_like_time: movieLikeTime });

    });
  } catch (e) {
    console.error("handleLikeAdd error =>", e);
  }
}
```

#### :pushpin: 4-2) 저장한 시간과 현재 시간을 판별하여 리스트에 노출
- `handleTimeCalculate(time)` 
  - 해당 함수의 매개변수 `time`은 DB의 리스트에 저장되어있는 `movie_like_time`값을 전달받는다.
  - 해당 함수에서 `getDate` 변수로 매개변수를 할당하고 `nowDate`변수로 현재 시간을 할당한다.
  - `CalculateDate`: 변수에서 현재 시간과 DB에 저장된 리스트의 시간을 빼주었다.
    - `calcSeconds`: 시간을 빼준 값을 초로 변환
    - `calcMinutes`: 시간을 빼준 값을 분으로 변환
    - `calcHours`: 시간을 빼준 값을 시간으로 변환
    - `calcDays`: 시간을 빼준 값을 일로 변환
  - 이후 `if`문을 통해 시간이 얼마나 지났는지 return해주었다.
  - 해당 함수는 HTML 리스트를 랜더하는 `renderColumnList`함수에서 `<p>${handleTimeCalculate(data.movie_like_time)}</p>` 실행되어 리스트가 생성될 때 함수처리 후 시간이 노출된다.

```javascript
// mypage.js
// 현재 시간과 DB의 저장된 리스트의 시간을 계산하는 함수
function handleTimeCalculate(time) {
  const getDate = new Date(time);
  const nowDate = new Date();

  const CalculateDate = nowDate - getDate;
  const calcSeconds = Math.floor(CalculateDate / 1000);
  const calcMinutes = Math.floor(calcSeconds / 60);
  const calcHours = Math.floor(calcMinutes / 60);
  const calcDays = Math.floor(calcHours / 24);

  if (calcSeconds < 60) {
    return "방금";
  } else if (calcMinutes < 60) {
    return `${calcMinutes}분 전`;
  } else if (calcHours < 24) {
    return `${calcHours}시간 전`;
  }
  return `${calcDays}일 전`;
}

// 화면에 리스트를 출력하기 위해 HTML을 생성하는 함수
function renderColumnList(data, type) {
  let htmlContent = `
            <li class="${type} column-container">
                <div class="${type} column-img">
                    <a href="../view/detail.html?id=${
                      data.movie_id
                    }"><img src="https://image.tmdb.org/t/p/w500/${data.movie_img}"></a>
                </div>
                <div class="${type} column-contents">
                    <h4>${data.movie_title}</h4>
                    <p>${data.movie_over_view}</p>
                    <div class="feed-box">
                        <p>좋아요(${data.likeCount})</p>
                    </div>
                </div>
                <div class="${type} column-date">
                    <p>${handleTimeCalculate(data.movie_like_time)}</p>
                </div>
            </li>
        `;

  parentList.innerHTML += htmlContent;
}
```

### :fire: 마무리
5차 작업 중 저장한 시간을 리스트에 출력함에 있어서 `Date`객체를 알게 되었는데 처음에 어떻게 사용해야하는 지 몰라서 블로그를 많이 참고해서 적용했다. (선 복붙 -> 테스트 -> 이해 -> 적용) 시간을 저장함에 있어서 `FireBase`에서 조금 헷갈렸는데, 이전에 작성했던 주석을 보며 적용하니 그래도 쉽게 적용할 수 있었다. 5차 작업을 끝으로 최종 마무리 후 발표하는 시간도 가졌는데 일주일간 모두의 노력이 결실을 맺은 순간이 아닐까라 생각한다. 이번 프로젝트를 통해 기능을 만들면서 `fetch`, `firebase`와 `javascript`를 집중적으로 배울 수 있는 시간이었으며, 조금 친해졌다고 생각하며 글을 마무리한다. 

#### :pushpin: 다음글
- JavaScript 영화 검색 사이트 팀 프로젝트 회고

#### :pushpin: 링크
- **깃헙 링크**: [Moview](https://github.com/sHyunis/Moview)
- **적용 화면**: [https://shyunis.github.io/Moview/index.html](https://shyunis.github.io/Moview/index.html)

#### :pushpin: JavaScript 영화 검색 사이트 팀 프로젝트 글 모음
https://rarrit.github.io/til/js/api/mini/sparta-team-pjt04/)