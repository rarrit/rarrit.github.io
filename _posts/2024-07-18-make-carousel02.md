---
title: "JavaScript 슬라이드 제작 및 회고 (2차)"
date: 2024-07-18
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

## :ledger: JavaScript 슬라이드를 제작했으니 리팩토링을 해보자!

### :one: 프로젝트 설명

스파르타 코딩클럽 react 6기 본캠프 첫 번째 미니 프로젝트입니다.<br/>
해당 프로젝트는 같은 조원을 소개하는 웹 사이트를 자유롭게 제작하는 것이며, 저의 롤은 팀원 상세페이지에 들어갈 사진 캐러셀을 구현하는 것입니다.<br/><br/>

지난 글에서는 처음으로 자바스크립트를 사용하여 슬라이드를 구현해보았습니다.<br/>
첫 구현이다보니 중복으로 사용되는 코드도 있고 조금 더 가독성이 좋고 간결하게 작성할 수 있을 것 같아서 진행하게 되었습니다.

### :two: 기능 정의

1. 슬라이드 자동 롤링 효과 적용
2. 썸네일 영역(작은 이미지)를 클릭하면 해당 index에 맞게 롤링되는 영역(큰 이미지)이 슬라이드 되어야한다.
3. 이전, 다음 버튼 클릭 시 해당 슬라이드가 이동되어야 하며 썸네일 또한 그에 맞게 엑티브가 되어야한다.
4. 슬라이드가 끝에 도달했을 때 loop 되어야 한다.
5. 드래그하여 슬라이드가 이동되어야 한다.

### :three: 사용 언어

- HTML5, CSS3, JavaScript, FireBase

### :four: 작성 코드 (2차 구현 : 리펙토링)

#### :pushpin: 4-1) 함수 처리

분명 기억속엔 깃에 연결해서 커밋하며 수정했는데, 히스토리를 아무리 찾아봐도 이전 코드가 없는걸 보니 로컬에서 함수 처리까지 완료하고 올렸나보다.. 아쉽게도 이전 코드와 현재 코드가 없지만 기억을 살려 작성해보겠다.<br/<br/>

먼저 반복되는 코드들을 함수 처리해서 재사용 가능하고 유지보수가 쉽게 처리했다.<br/>
가장 먼저 슬라이드가 롤링될 때 현재 인덱스 값에 맞춰 썸네일 엑티브하는 코드를 함수로 만들었다.<br/><br/>

```javascript
// 함수 처리한 코드
const carouselThumbSet = () => {
  carouselThumbItems.forEach((thumb) => thumb.classList.remove("curr"));
  carouselThumbItems[slideCurrentIdx].classList.add("curr");
};
```

기존에는 함수 안의 코드를 슬라이드 이전,다음,드래그 등 모든 이벤트에 넣어줬었다.<br/>
그렇다보니 수정을 진행할 경우 모든 코드를 수정했어야 했고, 유지보수가 쉽게 함수화 처리를 해주었다.<br/>

### :five: 함수 처리 과정에서의 오류 (feat. 화살표 함수, 함수 호이스팅)

`function 함수명(){}`으로 사용했으나 화살표 함수를 알게 된 후 최신 언어라는 생각에 무작정 사용했는데 돌아보며 생각해보면 스스로도 조금 어이없는게 화살표 함수에 대해 이해하고 쓰지 않았다는 점이다.<br/><br/>
함수선언식과 화살표함수에서 필자가 크게 고통을 받았던 건 `함수 호이스팅`이었는데,
화살표 함수는 선언 후 사용이 가능한데, 그냥 무지성으로 함수로 만들고 실행 순서에 맞춰 붙이다보니, 호이스팅이 되지 않아서 넘겨줘야 할 값들을 찾지 못해 모든 기능에서 에러가 노출되었던 것..!<br/><br/>
나는 당연히 호이스팅이 된다고 생각하고 그 문제가 아닐거라 생각해서 모든 함수에 콘솔을 찍어 어떤 순서로 실행되는지 테스트하다가 알게 되었고 구글링해보고 확신을 가졌다. <br/><br/>

### :six: 알게된 부분

이 과정에서 함수가 호이스팅이 되지 않아 변수 또한 전역에서 함수안으로 넣게되고, 또 다른 스파게티를 만든 경험을 갖게 되었는데, 앞으로는 함수 표현식(화살표 함수)을 사용할 때와 함수 선언식을 사용할 때를 확실히 이해하고 작성해 봐야겠다.<br/>

- [함수 선언식,표현식 차이점 참고 블로그](https://idealstring.tistory.com/4)
- [화살표 함수 호이스팅 참고 블로그](https://www.padosum.dev/wiki/JavaScript-Arrow-Function-Hoisting/)

### :fire: 마무리

이번 미니프로젝트를 진행하면서 누군가에게는 당연할 수 있던 부분이지만 이제라도 알게되어 너무 좋았다.<br/> 함수 호이스팅 관련 수정은 프로젝트 완료 기간에 맞춰야해서 진행하지 못했지만 주말에 한번 도전해볼 예정이다.<br/><br/>

또한, 화살표 함수, 함수 선언식과 함수 표현식의 차이점, 호이스팅에 대해 더 자세히 공부해보고 기록해봐야겠다!<br/><br/>

내일은 오늘보다 더 성장해 있길 기대하며, 글을 마무리해본다. 끝!

#### :pushpin: 적용한 코드

- **깃헙 링크**: [https://github.com/rarrit/TIL/tree/main/Project/carouselSlide](https://github.com/rarrit/TIL/tree/main/Project/carouselSlide)
- **화면 링크**: [https://rarrit.github.io/TIL/Project/carouselSlide/](https://rarrit.github.io/TIL/Project/carouselSlide/)

#### :pushpin: 다음글 목표

1. 제작 3차 과정은 FireBase를 통해 데이터를 전달받았을 때의 오류 해결과정을 작성해보겠습니다!
