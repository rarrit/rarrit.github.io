---
title: "JavaScript 슬라이드 제작 및 회고 (1차)"
date: 2024-07-17
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

## :ledger: JavaScript으로 슬라이더 기능을 만들어보자!

### :one: 프로젝트 설명

스파르타 코딩클럽 react 6기 본캠프 첫 번째 미니 프로젝트입니다.<br/>
해당 프로젝트는 같은 조원을 소개하는 웹 사이트를 자유롭게 제작하는 것이며, 저의 롤은 팀원 상세페이지에 들어갈 사진 캐러셀을 구현하는 것입니다.<br/>

### :two: 기능 정의

1. 슬라이드 자동 롤링 효과 적용
2. 썸네일 영역(작은 이미지)를 클릭하면 해당 index에 맞게 롤링되는 영역(큰 이미지)이 슬라이드 되어야한다.
3. 이전, 다음 버튼 클릭 시 해당 슬라이드가 이동되어야 하며 썸네일 또한 그에 맞게 엑티브가 되어야한다.
4. 슬라이드가 끝에 도달했을 때 loop 되어야 한다.
5. 드래그하여 슬라이드가 이동되어야 한다.

### :three: 사용 언어

- HTML5, CSS3, JavaScript, FireBase

### :four: 작성 코드 (1차 구현)

- JS 파일

  ```javascript
  document.addEventListener("DOMContentLoaded", function () {
    const carouselWrap = document.getElementById("carousel-wrap"); // 전체 감싸고있는 슬라이드 영역
    const carouselSlide = document.getElementById("carousel-slide"); // 큰 이미지 슬라이드 (자동)
    const carouselSlideItems = document.querySelectorAll(
      ".carousel-slide-item"
    ); // 큰 이미지 슬라이드 아이템
    const carouselSlideLength = carouselSlideItems.length;
    const carouselThumb = document.getElementById("carousel-thumb");
    const carouselThumbItems = document.querySelectorAll(
      ".carousel-thumb-item"
    );
    const carouselBtnArea = document.getElementById("carousel-btn-area");
    const carouselBtnPrev = document.getElementById("btn-prev");
    const carouselBtnNext = document.getElementById("btn-next");

    let slideWid; // 슬라이드 넓이 값
    let slideAllWid; // 슬라이드 요소 전체 넓이 값
    let slideCurrentIdx = 0; // 슬라이드 현재 인덱스 값
    let slideMoveDelay = 0; // 슬라이드 돌아가는 속도
    let slideMoveTimer; // interval 초기화

    // [#01] 슬라이드 리스트 초기 넓이 세팅
    slideWid = carouselSlide.offsetWidth;
    carouselSlideItems.forEach((item, idx) => {
      // console.log(item);
      item.style.width = slideWid + "px";
    });

    // [#02] 슬라이드 자동 롤링
    const carouselSlideMove = () => {
      // console.log(slideMoveDelay);
      slideMoveDelay = 3000;
      slideMoveTimer = setInterval(() => {
        slideCurrentIdx = (slideCurrentIdx + 1) % carouselSlideItems.length; // 현재 인덱스값 구하기
        carouselSlide.style.transform = `translate3d(-${
          slideCurrentIdx * slideWid
        }px, 0px, 0px)`;
        // 활성화된 썸네일 업데이트
        carouselThumbItems.forEach((thumb, idx) => {
          thumb.classList.toggle("curr", idx === slideCurrentIdx);
        });
        console.log(slideCurrentIdx);
      }, slideMoveDelay);
    };
    carouselSlideMove();

    // [#03] 슬라이드 썸네일 클릭 기능
    carouselThumbItems.forEach((item, idx) => {
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

    // [#04] 현재 인덱스 값에 맞춰 썸네일 엑티브
    const carouselThumbSet = () => {
      carouselThumbItems.forEach((thumb) => thumb.classList.remove("curr"));
      carouselThumbItems[slideCurrentIdx].classList.add("curr");
    };

    // [#05] 슬라이드 뒤로가기
    const carouselPrevMove = () => {
      clearInterval(slideMoveTimer); // 롤링 초기화
      slideCurrentIdx == 0 ? (slideCurrentIdx = 4) : slideCurrentIdx--; // 슬라이드 맨 앞이면 마지막으로 이동
      carouselSlide.style.transform = `translate3d(-${
        slideCurrentIdx * slideWid
      }px, 0px, 0px)`;

      carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
      carouselSlideMove(); // 다시 롤링 시작
    };

    // [#06] 슬라이드 앞으로가기
    const carouselNextMove = () => {
      clearInterval(slideMoveTimer); // 롤링 초기화
      slideCurrentIdx == 4 ? (slideCurrentIdx = 0) : slideCurrentIdx++; // 슬라이드 마지막이면 맨 앞으로 이동
      carouselSlide.style.transform = `translate3d(-${
        slideCurrentIdx * slideWid
      }px, 0px, 0px)`;

      carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
      carouselSlideMove(); // 다시 롤링 시작
    };

    // [#07] 이전 버튼 다음 버튼 클릭
    carouselBtnPrev.addEventListener("click", carouselPrevMove); // 이전 버튼 클릭
    carouselBtnNext.addEventListener("click", carouselNextMove); // 다음 버튼 클릭

    let isDragChk = false; // [drag] 초기화
    let downClientX; // [drag] 마우스 클릭했을 때 포인터 위치
    let upClientX; // [drag] 마우스 땟을 때 포인터 위치

    // [#08] 마우스 클릭했을 때 이벤트
    carouselSlide.addEventListener("mousedown", (e) => {
      isDragChk = true;
      downClientX = e.clientX; // 처음 클릭할 때 클릭 지점 체크
    });

    // [#09] 마우스 클릭에서 땟을 때 이벤트
    carouselSlide.addEventListener("mouseup", (e) => {
      // 이미지 들어가면 실행이 안되가지고 css 에서 img에 user-drag: none; 넣어줘야함
      isDragChk = false;
      upClientX = e.clientX; // 클릭후 땟을 때 클릭 지점 체크

      // 첫클릭 > 클릭땟을때 ? 오른쪽 이동 : 왼쪽 이동;
      downClientX > upClientX ? carouselNextMove() : carouselPrevMove();
      carouselThumbSet(); // 현재 인덱스 값에 맞춰 썸네일 엑티브
    });

    // 마우스 이동 이벤트
    carouselSlide.addEventListener("mousemove", (e) => {
      if (isDragChk) {
        //console.log(e);
      }
    });

    // [#10] 마우스 들어올 때 이벤트
    carouselSlide.addEventListener("mouseover", (e) => {
      isDragChk = false;
      clearInterval(slideMoveTimer); // 자동 슬라이드 종료
    });

    // [#11] 마우스 나갔을 때 이벤트
    carouselSlide.addEventListener("mouseleave", (e) => {
      isDragChk = false;
      carouselSlideMove(); // 자동 슬라이드 실행
    });
  });
  // 마우스 이벤트 종류 참고: https://hianna.tistory.com/492
  ```

### :five: 풀이 과정 (1차)

1. [#01] 슬라이드 리스트 넓이 값 적용<br/>

- 각 조원의 상세페이지에서 FireBase로 이미지 데이터를 전달받아 슬라이드에 적용해야 합니다.<br/>
  이미지의 크기는 같을 수 없으며, 슬라이드 영역 넓이에 자동으로 맞추어 주기 위해 각 슬라이드마다 넓이를 지정해줬습니다.

2. [#02] 슬라이드 자동 롤링<br/>

- 슬라이드가 자동 롤링 되어야 할 때 가장 먼저 생각한 것은 지난번 `숫자 기억하기 게임`을 제작했을 때 사용했던 `setInterval` 이었습니다.<br/>
  처음에는 슬라이드 영역에서 `transform: translate3d`의 값을 `slideWid(슬라이드 영역)` 만큼 - 해주면 된다고 쉽게 생각해서 적용했으나, 이후 썸네일을 클릭할 때나 드래그할 때, 현재 슬라이드의 index 값이 반드시 필요하단 걸 느낀 후 과감히 변경<br/>

3. [#03] 슬라이드 썸네일 기능 적용<br/>

- 슬라이드에 썸네일 기능을 적용하기 위해 `carouselThmbItems` 변수에서 각각 클릭 시 해당 idx 값과 slide 넓이를 곱한 후 슬라이드의 transform3d에 값을 전달하여 슬라이드가 움직이도록 적용했다.

4. [#04] 현재 인덱스에 맞춰 엑티브 기능 적용<br/>

- 슬라이드가 자동으로 롤링될 때나 인덱스 값이 변경될 때 그에 맞춰서 썸네일도 엑티브 해주기 위해 함수를 생성했다.

5. [#05, #06, #07] 슬라이더 이전,다음 으로 이동하는 기능<br/>

- 먼저 공통적으로 버튼을 클릭할 경우 `clearInterval(slideMoveTimer)` 자동 롤링을 중지한다. <br/>
  이후 이전 버튼을 예시로 `slideCurrentIdx == 0 ? (slideCurrentIdx = 4) : slideCurrentIdx--` 삼항 연산자를 사용해서 슬라이드가 첫 번째일 경우 끝의 슬라이더로 이동되며, 첫 번째가 아닐 경우 인덱스값을 1씩 감소시켜 이동시켰다.<br/>
  다음 버튼 또한 같은 방법으로 적용했다.

6. [#08] 마우스를 처음 클릭했을 때 이벤트<br/>

- 처음 드래그 기능을 제작할 때 당연히 `addEventListener("drag")` 인 줄 알았다.<br/>
  2시간(?)동안 삽질 후 구글링을 통해 알아봤는데, 처음 본 블로그에서 `mousedown`, `mouseup`, `mousemove`를 사용하는 걸 알고 다시 테스트를 진행했다.<br/>
  그리고 드래그 하는 기능을 적용하기위해 생각했을 때, 아래와 같이 진행하기 위해 `slideWid/2 = 슬라이드 중앙` 값에서 클릭한 영역(`downClientX = e.clientX`)이 우측, 좌측을 구해서 진행했는데 바보같이 mousedown 안에 두 변수를 선언해버려서 엄청나게 삽질을 했다..
  1,2번 같이 생각했으나 **움직인 후 클릭을 뗀다**에서 mouseup 에서 처리했어야 했는데 말이다! 하하하<br/>
  - 오른쪽으로 이동하려면 보통 슬라이드의 오른쪽 영역을 클릭 후 왼쪽으로 마우스를 움직인 후 클릭을 땐다.
  - 왼쪽으로 이동하려면 슬라이드의 왼쪽 영역을 클릭 후 오른쪽으로 마우스를 움직인 후 클릭을 땐다.

7. [#09] 마우스를 클릭 후 땟을 때 이벤트<br/>

- [#08]에서 삽질 후 `slideWid/2 = 슬라이드 중앙` 또한 쓸모 없음을 깨닫고 간단하게 첫클릭이 땐클릭보다 높으면 다음으로 이동 아니면 이전으로 이동(`downClientX > upClientX ? carouselNextMove() : carouselPrevMove()`)으로 적용했다.<br/>

8. [#10, #11] 마우스 들어올 때 이벤트, 마우스 나갈 때 이벤트

- 슬라이드 영역에 들어올 때 슬라이드가 멈추고 나갈 때 다시 실행하면 된다고 너무 간단하다고 생각을 했었는데, 이후 혼자 테스트하다가 마우스를 엄청 빠르게 들어갓다 나갓다 할 때 에러가 생겼다.<br/>
  마우스 나갈 때에 슬라이드를 실행하고 들어올 때 중지했으니 당연히 오류가 안생겼을 거라 생각했지만 놀랍게도 함수들이 실행되는 시간에 지연이 생겨서 에러가 뒤늦게 발견되는데.. (자동 롤링 3초마다 적용했는데, 마우스 나갈때 횟수만큼 실행됨..) <br/>
  이 부분에 대해서는 수정 방법을 찾아봐야할 것 같다.

### :fire: 마무리

이렇게 자동 슬라이드, 썸네일과 슬라이드 엑티브 연동, 좌우 버튼 기능, 드래그 기능을 적용한 슬라이드를 제작해봤다.<br/>
기능을 만들 때 일단 만들어보고 리팩토링해야지! 하고 만들었는데, 막상 만드는 것 자체도 쉽지 않았고 생각대로 되지 않아서 어려움이 많았다.<br/>
현재는 3차까지 완료한 상태이고 제작과정 1차를 작성하는 입장에서 당시에는 정말 뿌듯하고 재밌었는데, 리팩토링 과정과 firebase를 통해 데이터를 전달받아서 동적으로 화면에 표현하기까지 진행하는데 정말 다양한 오류들과 어려움이 있었다.<br/>
개인적으로 슬라이드를 구현할 때 `swiper`와 `slick` 등 라이브러리를 사용하며 직접 만드는 것을 쉽게 생각했는데 생각보다 고려해야될 부분도 많고 절대 쉽지 않았다고 느꼈..<br/>
사실 3차까지 작업하면서 깔끔하게 코드를 짯냐고 물어보면 당연히 `아니..ㅎㅎ`라고 할 거지만 만들면서 배운것도 많고 자바스크립트를 공부한다면 꼭 한번쯤 제작해보라고 추천하고 싶다.

#### :pushpin: 다음글 목표
1. 제작 2차 과정은 코드리팩토링을 진행한 경험을 작성해 볼 예정입니다!


#### :pushpin: 링크
- **깃헙 링크**: [https://github.com/rarrit/TIL/tree/main/Project/carouselSlide](https://github.com/rarrit/TIL/tree/main/Project/carouselSlide)
- **화면 링크**: [https://rarrit.github.io/TIL/Project/carouselSlide/](https://rarrit.github.io/TIL/Project/carouselSlide/)


#### :pushpin: [Mini Project] 슬라이드 제작 및 회고 글
- [JavaScript 슬라이드 제작 및 회고 (1차)](https://rarrit.github.io/mini/til/js/make-carousel01/)
- [JavaScript 슬라이드 제작 및 회고 (2차)](https://rarrit.github.io/mini/til/js/make-carousel02/)
- [JavaScript 슬라이드 제작 및 회고 (3차)](https://rarrit.github.io/mini/til/js/make-carousel03/)