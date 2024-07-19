---
title: "JavaScript 슬라이드 제작 및 회고 (3차)"
date: 2024-07-19
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
  - js
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: 제작한 슬라이드에 데이터를 바인딩 후 동적으로 화면에 표현해보자!

### :one: 프로젝트 설명

스파르타 코딩클럽 react 6기 본캠프 첫 번째 미니 프로젝트입니다.<br/>
해당 프로젝트는 같은 조원을 소개하는 웹 사이트를 자유롭게 제작하는 것이며, 저의 롤은 팀원 상세페이지에 들어갈 사진 캐러셀을 구현하는 것입니다.<br/><br/>

지난 글에서는 리팩토링 과정에서 함수 호이스팅이 되지않아 겪었던 오류에 대해 작성해보았습니다.<br/>
이번 글은 FireBase를 통해 데이터를 전달받아 화면에 노출하면서 겪었던 과정을 정리해봤습니다.

### :two: 기능 정의

1. FireBase에서 각 조원의 데이터를 추출하여 슬라이더에 적용
2. 상단 탭(조원) 클릭 시 각 조원에 맞게 데이터 노출 및 슬라이드 초기화
3. 데이터를 전닯받은 후에 슬라이드 실행

### :three: 사용 언어

- HTML5, CSS3, JavaScript, FireBase

### :four: 3차 구현 : 데이터 전달된 시점에 맞춰 적용

#### :pushpin: 4-1) 슬라이드 초기화, 슬라이드 동적으로 표현하기(?)

2차까지 스파게티를 열심히 제조한 후 드디어 각 파트에 작업한 코드를 깃을 통해 합쳤다.<br/>
나는 가장 마지막에 git pull을 받고 나의 코드를 적용해봤는데, 역시 내 슬라이드는 생을 마감해버렸다.. 문제 원인과 해결 과정을 표현하자면<br/><br/>

- 해당 페이지의 조원의 데이터를 바인딩 후 노출했을 때, 슬라이드를 실행 시키는 함수가 화살표 함수여서 호이스팅이 않되어서 해당 함수의 위치를 전역으로 설정한 변수 아래 넣었다.
- 두 번째로는 각 탭을 클릭 시 슬라이드가 그 때 실행이 되어야하는데, 자동으로 롤링`(setInterval)`기능이 계속 유지되고 있던것. 그로 인해 슬라이드를 초기화 하는 함수를 만들어줘야했다.
  - **_ 초기화 함수에서 적용한 내용 _**
    - `slideCurrentIdx = 0;` 슬라이드가 다시 처음으로 시작해야해서 인덱스 값을 0으로 지정
    - `clearInterval(slideMoveTimer);` 자동 롤링 종료
    - `carouselSlide.innerHTML = "";` 메인 롤링되는 슬라이드 영역 없애기
    - `carouselThumb.innerHTML = "";` 썸네일 영역 없애기
    - `carouselSlide.style.transform = "translate3d(0px, 0px, 0px)";` 슬라이드 인덱스 값을 변경했지만, 위치 또한 한번더 첫 슬라이드로 이동
    - `isSlide = false;` isSlide 변수는 false로 만들어줘서 시작 위치 전에 true로 설정 후 if문을 통해 슬라이드 함수를 시작합니다.
- 슬라이드 초기화 함수를 만들어주면 초기화 이후 슬라이드가 실행되는 함수 또한 생성해줘야해서 슬라이드 시작되는 함수를 생성

  - **_ 슬라이드 시작 함수에서 적용한 내용 _**
    - 정말 이 과정이 개인적으로 크게 힘들게 느껴졌는데, 이 파트를 완료해야하는 기간이 되게 적었다.<br/><br/>이제와서 돌아보면 해봤으면 어떨까 하는 마음은 있지만 나로 인해 같은 조원이 피해를 입는게 두려웠던 것 같다. 이 과정이 힘들었던 부분은 모든 함수를 화살표 함수로 선언해버린 탓에 실행 순서를 맞춰야하는데, 그 순서에 맞게 작업하려면 전역으로 선언했던 변수 또한 함수 내로 들어가야했던 부분도 있었고 함수의 선언 순서도 바꿔주면서 엄청난 에러가 노출되었던 것이다.<br/><br/>특히 `carouselThumbSet`(인덱스 값에 맞춰 썸네일 엑티브) 함수가 slideCurrentIdx 값을 받지 못해서 전체 슬라이드가 계속 에러가 노출이 되었고 선언 순서를 `자동롤링(인덱스값 초기 선언 함수)` -> `인덱스 값에 맞춰 엑티브`로 변경 후 전역으로 지정했던 `썸네일 슬라이드들` 을 함수내로 이동시켜 문제를 해결했다.<br/><br/> 요약하면 전달받지 못하는 변수를 받기 위해 화살표 함수의 위치를 순서에 맞게 재변경하고 선언되지 않은 태그들로 인해 노출된 오류를 함수내로 이동시켜 해결했다.

- 초기화,실행 함수를 생성 후 슬라이드를 데이터 바인딩하여 동적으로 화면에 노출시키는 기능을 적용하기 위해 `const createSlides = (imgs) => {}`를 생성했다. `(imgs)`는 DB에 저장된 각 조의 데이터를 담은 함수를 받기 위한 매개변수이다.
- 마지막으로는 `데이터를 전달 받기 전 초기화` -> `받은 후 슬라이드 실행`으로 적용해서 문제를 해결했다.
- 아래는 스파게티 만들기 유망주의 코드이다.

#### :pushpin: 4-2) 적용 코드

```javascript
// 슬라이드 자동 롤링
const carouselSlideMove = () => {
  slideMoveTimer = setInterval(() => {
    const carouselSlideLength = document.querySelectorAll(
      ".carousel-slide-item"
    ).length; // 슬라이드 갯수 확인
    slideCurrentIdx = (slideCurrentIdx + 1) % carouselSlideLength; // 현재 인덱스값 구하기
    carouselSlide.style.transform = `translate3d(-${
      slideCurrentIdx * slideWid
    }px, 0px, 0px)`; // 인덱스 값 x 슬라이드 넓이 = 현재 슬라이드 위치

    carouselThumbSet(); // 인덱스 값에 맞춰 썸네일의 순서도 액티브
  }, slideMoveDelay);
};

// 현재 인덱스 값에 맞춰 썸네일 엑티브
const carouselThumbSet = () => {
  const carouselThumbItems = document.querySelectorAll(".carousel-thumb-item");
  carouselThumbItems.forEach((thumb) => thumb.classList.remove("curr"));
  carouselThumbItems[slideCurrentIdx].classList.add("curr");
};

// 슬라이드 뒤로가기
const carouselPrevMove = () => {
  clearInterval(slideMoveTimer); // 롤링 초기화
  slideCurrentIdx == 0 ? (slideCurrentIdx = 4) : slideCurrentIdx--; // 슬라이드 맨 앞이면 마지막으로 이동
  carouselSlide.style.transform = `translate3d(-${
    slideCurrentIdx * slideWid
  }px, 0px, 0px)`;

  carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
  carouselSlideMove(); // 다시 롤링 시작
};

// 슬라이드 앞으로가기
const carouselNextMove = () => {
  clearInterval(slideMoveTimer); // 롤링 초기화
  slideCurrentIdx == 4 ? (slideCurrentIdx = 0) : slideCurrentIdx++; // 슬라이드 마지막이면 맨 앞으로 이동
  carouselSlide.style.transform = `translate3d(-${
    slideCurrentIdx * slideWid
  }px, 0px, 0px)`;

  carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
  carouselSlideMove(); // 다시 롤링 시작
};

// 슬라이드 시작
const handleSlideStart = () => {
  // 슬라이드 리스트 초기 넓이 세팅
  slideWid = carouselSlide.offsetWidth; // 시작될 때 현재 슬라이드 영역의 넓이 저장
  carouselSlideItems.forEach(
    (item, idx) => (item.style.width = slideWid + "px")
  ); // 각 슬라이드마다 넓이 지정

  carouselSlideMove(); // 슬라이드 시작

  // 슬라이드 썸네일 클릭 기능
  document.querySelectorAll(".carousel-thumb-item").forEach((item, idx) => {
    // 썸네일 클릭 이벤트
    item.addEventListener("click", () => {
      clearInterval(slideMoveTimer); // 롤링 초기화
      slideCurrentIdx = idx; // 인덱스값을 클릭한 인덱스 값과 동일하게 변경해줌

      //클릭한 요소에 맞춰 슬라이드 이동 : carouselSlide의 transform3d의 값을 클릭한 요소의 -(인덱스*슬라이드) 넓이로 적용
      carouselSlide.style.transform = `translate3d(-${
        slideCurrentIdx * slideWid
      }px, 0px, 0px)`;

      carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
      carouselSlideMove(); // 다시 롤링 시작
    });
  });

  carouselBtnPrev.addEventListener("click", carouselPrevMove); // 이전 버튼 클릭
  carouselBtnNext.addEventListener("click", carouselNextMove); // 다음 버튼 클릭
};

let isDragChk = false; // [drag] 초기화
let downClientX; // [drag] 마우스 클릭했을 때 포인터 위치
let upClientX; // [drag] 마우스 땟을 때 포인터 위치

// 마우스 클릭했을 때 이벤트
carouselSlide.addEventListener("mousedown", (e) => {
  isDragChk = true;
  downClientX = e.clientX; // 처음 클릭할 때 클릭 지점 체크
});

// 마우스 클릭에서 땟을 때 이벤트
carouselSlide.addEventListener("mouseup", (e) => {
  // 이미지 들어가면 실행이 안되가지고 css 에서 img에 user-drag: none; 넣어줘야함
  isDragChk = false;
  upClientX = e.clientX; // 클릭후 땟을 때 클릭 지점 체크

  // 첫클릭 > 클릭땟을때 ? 오른쪽 이동 : 왼쪽 이동;
  downClientX > upClientX ? carouselNextMove() : carouselPrevMove();
  carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
});

// 슬라이드 초기화
const slideReset = () => {
  slideCurrentIdx = 0;
  clearInterval(slideMoveTimer);
  carouselSlide.innerHTML = "";
  carouselThumb.innerHTML = "";
  carouselSlide.style.transform = `translate3d(0px, 0px, 0px)`;
  isSlide = false;
};

// 슬라이드 생성
const createSlides = (imgs) => {
  let slideBigItems = "";
  let slideThumbItems = "";

  for (let i = 0; i < imgs.length; i++) {
    slideBigItems += `
                <div class="carousel-slide-item">
                    <img src="${imgs[i]}" />
                </div>
            `;
    slideThumbItems += `
                <div class="carousel-thumb-item ${i === 0 ? "curr" : ""}">
                    <img src="${imgs[i]}" />
                </div>
            `;
  }
  carouselSlide.innerHTML = slideBigItems;
  carouselThumb.innerHTML = slideThumbItems;
};

/////////////////// 탭 클릭 시 //////////////////////////
const tabs = document.querySelectorAll(".tab-column");
tabs[valName].classList.add("tab-active");
tabs.forEach((target) => {
  target.addEventListener("click", async function (image) {
    slideReset();

    const temp = document.querySelector(".temp");
    temp.remove();
    document.querySelector(".coment-cont").remove();

    docs.forEach((doc) => {
      let row = doc.data();
      let docId = doc.id;

      if (row["index"] == target.id) {
        let index,
          likes,
          city = createDescrption(row);
        createComment(target.id);
        createLikeBtn(index, likes, docId);
        getWeather(city);
      }
    });
    isSlide = true;
    if (isSlide) handleSlideStart();

    document.querySelectorAll(".tab-column").forEach((targets) => {
      targets.classList.remove("tab-active");
    });
    target.classList.add("tab-active");
  });
});
```

### :five: 알게된 부분

현재는 미니 프로젝트가 최종적으로 완료해서 발표까지 한 시점이라 느껴지는 부분이 많은 것 같다.<br/>
요번 프로젝트를 진행하면서 나의 가장 첫 번째 문제점은 잘못된 지식으로 코드를 난사한 것이고, 두 번째 문제점은 데이터를 전달 받은 뒤에 실행되게 고려하지 않고 만들었던 점이다.<br/>

#### :pushpin: 5-1) 앞으로 개선해야할 사항

- 함수 선언식, 함수 표현식과 호이스팅에 대해 공부해보자.
- 데이터가 처리되는 시점과 화면에 로드되는 시점을 고려해서 재사용이 쉽게 기능을 만들어보자.

### :fire: 마무리

미니 프로젝트를 통해 개인적으로 얻어간 것이 너무나도 많은 것 같다.<br/>
기획부터 와이어프레임 제작, 코드 컨벤션 진행, github rules 정하기, 마지막으로 가장 중요한 깃으로 협업하기!<br> <br>  
깃으로 협업을 해본적이 없는데, 조원 분들과 브랜치를 만들어서 각자 작업하고 PR요청하고 서로 문제가 있는지 같이 확인하고 merge하고 다시 pull받는 이 과정들에서 문제가 크게 없어서 그런지 정말 재밌게 느껴졌고, 서로 문제에 대해 같이 고민하고 해결하고 모든 과정이 나에겐 큰 경험인 것 같다.<br/><br/>
또한, 기능 제작부터 리팩토링, 동적으로 슬라이드 표현하는 과정에서 경험한 오류들이 나의 성장에 있어서 좋은 양분들이라 생각! 주말에 다시한번 슬라이드를 제작해보고 리팩토링을 해보는 시간을 가지며 조금 더 성장하길 바라는 나를 상상해보며 글을 마친다.

#### :pushpin: 프로젝트 "뭐야?" 링크

- 완성 프로젝트 url: [https://dev-rjw.github.io/LuckyVicky/](https://dev-rjw.github.io/LuckyVicky/)
- 프로젝트 소개: [https://github.com/dev-rjw/LuckyVicky](https://github.com/dev-rjw/LuckyVicky)
