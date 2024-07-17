---
title: "JavaScript 숫자 기억하기 게임 제작 및 풀이 과정"
date: 2024-07-12
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

## :ledger: JavaScript으로 숫자 기억하기 게임을 만들어 보자!

### :one: 요구 사항

1. 숫자를 기억하고 맞추는 숫자 기억 게임을 만든다.
2. 시작 버튼 클릭 시 1000 ~ 9999의 숫자가 무작위로 나타난다. 이때. **5초 뒤** 숫자가 다시 사라진다.
3. 사용자가 숫자를 입력하고, 제출 버튼을 통해 정답 유무를 확인합니다

- **_정답_** : "정답입니다!" 노출
- **_오답_** : "오답입니다. 정답은 [정답숫자] 입니다." 노출

4. (선택 사항) CSS로 간단한 스타일을 적용한다.
5. (선택 사항) 사용자 경험을 향상시키기 위한 기능을 추가한다. (e.g. 남은 시간초 노출, 텍스트 입력 시 안내 문구 노출, 점수 표시 등)

### :two: 사용 언어 및 폴더 구조

- HTML5, CSS3, JavaScript
- 📦rememberNumberGame<br/>
  ┣ 📂assets<br/>
  ┃ ┣ 📂css<br/>
  ┃ ┃ ┗ 📜rememberNumber.css<br/>
  ┃ ┗ 📂js<br/>
  ┃ ┃ ┗ 📜rememberNumber.js<br/>
  ┗ 📜rememberNumber.html<br/>

### :three: 작성 코드

- HTML 파일

  ```html
  <div class="container">
    <div class="gameBox">
      <h1>숫자 기억 게임</h1>
      <div id="rememberNumBox" style="display: none">
        <span id="numberVal">4075</span>
        <p><em id="countNum">5</em>초 남았어요!</p>
      </div>
      <div id="msgBox" style="display: none;"></div>
      <div class="inputBox">
        <input
          type="text"
          id="inputArea"
          placeholder="숫자를 입력하세요."
          disabled
        />
      </div>
      <div class="btnBox">
        <button type="button" id="btnStart">시작</button>
        <button type="button" id="btnAnswer">제출</button>
      </div>
    </div>
  </div>
  ```

- JS 파일

  ```javascript
  document.addEventListener("DOMContentLoaded", () => {
    const startBtn = document.getElementById("btnStart"); // 실행 버튼
    const answerBtn = document.getElementById("btnAnswer"); // 제출 버튼
    const inputArea = document.getElementById("inputArea"); // 인풋 영역
    const rememberNumBox = document.getElementById("rememberNumBox"); // 랜덤 숫자 할당할 영역
    const rememberNumVal = document.getElementById("numberVal"); // 랜덤 숫자 노출할 영역
    const msgBox = document.getElementById("msgBox"); // 메시지 박스 영역
    const countNum = document.getElementById("countNum"); // 카운트 영역

    let rememberNumber = 0; // 기억할 숫자 초기값
    let delayCount = 4; // 카운트 다운 초 (0~4: 5초)
    let intervalTimer; // setInterval 초기화

    /******************************************************************************
     * 숫자 기억 게임 시작
     * ****************************************************************************
     **/
    const handleGameStart = () => {
      // 다시하기를 클릭 시 초기화해준다.
      handleResetGame();

      // 1000 ~ 9999의 랜덤으로 소숫점 제거하여 변수 randomNumber에 담아줌
      const randomNumber = Math.trunc(Math.random() * (9999 - 1000 + 1) + 1000);

      startBtn.innerText = "다시하기"; // 시작하기 -> 다시하기 텍스트 변경
      rememberNumBox.style.display = "block"; // 기억할 숫자 박스 영역 노출

      rememberNumber = randomNumber; // 기억해야할 숫자에 랜덤으로 선언한 값 할당
      rememberNumVal.innerText = randomNumber; // 노출해야할 텍스트에 랜덤으로 선언한 값 할당

      // 카운트 다운
      handleSetTimer();
    };

    /******************************************************************************
     * 카운트 다운, 시간 설정
     * ****************************************************************************
     **/
    const handleSetTimer = () => {
      intervalTimer = setInterval(() => {
        // console.log(count); 카운트 확인

        // 카운트 1씩 감소
        delayCount--;

        if (delayCount === 0) {
          // 0초가 되면 기억박스 제거, 함수 종료
          rememberNumBox.style.display = "none";
          inputArea.disabled = false;
          clearInterval(intervalTimer);
        } else {
          // 0초가 아닐 경우 카운트 적용
          countNum.innerText = delayCount;
        }
      }, 1000);
    };

    /******************************************************************************
     * 제출하기 버튼
     * ****************************************************************************
     **/
    const handleAnswer = () => {
      // 기억할 숫자가 0(시작버튼 클릭 안했을 시에)이면 실행되지 않게 함
      if (rememberNumber === 0) {
        alert("시작버튼을 눌러주세요!");
        return false;
      }

      let answerNumber = inputArea.value; // 인풋의 값을 받아옴
      answerNumber = Math.floor(answerNumber); // 인풋의 문자열 값을 숫자로 변환

      // 정답일 때 노출할 텍스트
      let trueText = `
      <p class="textGreen">한번에 맞췄다니 대단한걸요!</p>
    `;
      // 오답일 때 노출할 텍스트
      let falseText = `
      <p class="textRed">아쉽네요!</p>
      <span>정답은 <em id="msgNum">${rememberNumber}</em>입니다!</span>
    `;

      // console.log(rememberNumber, answerNumber); 확인

      // 메시지 박스 노출
      msgBox.style.display = "block";

      if (rememberNumber === answerNumber) {
        // 정답
        msgBox.innerHTML = trueText;
      } else {
        // 오답
        msgBox.innerHTML = falseText;
      }

      inputArea.disabled = true; // 인풋 비활성화
    };

    /******************************************************************************
     * 초기화 작업
     * ****************************************************************************
     **/
    const handleResetGame = () => {
      inputArea.disabled = true; // 인풋 비활성화
      inputArea.value = ""; // 작성 초기화
      msgBox.style.display = "none"; // 메시지 박스 제거
      rememberNumBox.style.display = "none"; // 기억 박스 제거
      delayCount = 5; // 카운트 초기화
      countNum.innerText = delayCount; // 카운트 초기화
      if (intervalTimer) clearInterval(intervalTimer); // 실행 중인 타이머가 있으면 중지
    };

    // 시작 버튼 클릭시 handleGameStart 실행
    startBtn.addEventListener("click", handleGameStart);
    answerBtn.addEventListener("click", handleAnswer);
  });
  ```

```

### :four: 풀이 과정
1. 변수 선언<br/>
처음에 전역으로 선언할 변수들과 함수 내에서 선언할 변수를 적용하면서 애를 많이 먹었다.<br/>
예를들면 ***기억할 숫자 초기값*** 같은 경우 요구사항에서 __시작 버튼 클릭 시__ 라는 단어가 뇌리에 깊게 박혀서 `handleGameStart(시작하기 함수)`함수에서 변수를 선언했고, `handleAnswer(제출하기 함수)`에서 if문의 조건에 적용할 때 그 값을 받아와야했어서 작업을 진행면서 전역으로 설정했다.

2. `handleGameStart` 게임 시작 함수<br/>
그나마 구현에 있어서 가장 쉬웟던 부분이다.<br/>
요구사항에서 몇가지 추가한 기능이 있는데, 요구 사항에 맞춰 작업 후 시작하기 버튼을 한번 눌렀을때는 문제가 없었지만 __여러번 눌럿을 때 문제__가 생겼던 것이었다.<br/>
문제 해결을 위해 ***시작하기 -> 다시하기*** 텍스트 추가와 `handleResetGame(초기화 함수)`를 적용해주었다.

- 시작하기 버튼 클릭 후 계속 클릭이 가능함. (클릭한 만큼 함수가 실행되어, 카운트 다운이 먹통이되버림)
- 무작위 숫자가 노출될 때, 입력이 가능함. (이것은 게임이고 기억할 숫자가 노출될 때 정답을 작성하는 것은 반칙임)
- 시작하기 버튼 클릭 후 다시하기가 있어야함. (게임은 한 번만 하면 안됨 여러번 해야함 ㅎㅎ..)


3. `handleSetTimer` 카운트 다운, 시간 설정 함수<br/>
처음 카운트 다운 작업을 할 때, 변수 `delayCount(카운트 시간)`와 `intervalTimer`을 함수안에서 선언을 했었다가 전역으로 변경했엇다.<br/>
카운트 시간의 경우 추 후 유지보수 할 때 함수까지 찾아가야 하는게 귀찮아서 전역으로 설정했고 `intervalTimer`는 게임 시작 함수 실행 시에 초기화 해주기 위해 전역으로 설정했다.

4. `handleAnswer` 제출하기 함수<br/>
제출하기 함수내에서도 추가해준 부분이 있다.
- 함수 조건에 맞춰 비활성화: 기억할 숫자가 0(시작버튼 클릭 안했을 시에)이면 함수가 실행되지 않게 적용 후 alert으로 "시작버튼을 눌러주세요!" 문구 추가함
- 인풋 비활성화 추가: 제출하기 버튼 클릭 후 if문을 통해 정답,오답의 멘트가 나오는데 다시 제출하는 것을 방지하기위해 input을 disabled 해주엇다. (오답일 때 정답을 알려주는데, 그걸 보고 정답을 작성 후 제출하니 "한번에 맞췄네요!" 라는 문구가 어색했다.)

5. `handleResetGame` 초기화 함수<br/>
게임 시작 함수를 만들 때 모든 어색했던 부분을 없애기 위해 추가해준 함수이다.<br/>
아래와 같이 초기화 기능들을 작성 후 다시하기(`hanldeGameStart`) 함수가 실행하니 모든 어색한 부분이 없어졌다고 생각한다.

- 인풋 비활성화
- 인풋 value 초기화
- 메시지 박스 제거 (다시하기 게임을 위한 초기화 작업)
- 기억 박스 제거 (다시하기 게임을 위한 초기화 작업)
- 카운트 초기화
- 실행중인 타이머가 있으면 중지

#### :pushpin: 적용한 코드
**깃헙 링크**: [숫자 기억하기 게임](https://github.com/rarrit/TIL/tree/main/Project/rememberNumberGame)

### :fire: 마무리
요구사항에 맞춰서 1차로 완성했을 때는 게임을 딱 한번만 한다는 조건일 경우 문제가 없었다.<br/>
그러나 여러번 클릭 했을 때 "??? 뭐지" 하면서 수정하다 보니 여러번 할 수 있는 게임이 완성된 것 같다.<br/>
처음 요구 사항을 보며 나름 정리한 후 작업을 시작했는데 불구하고 시간이 많이 소요된 것 같아 조금 아쉽지만 앞으로도 꾸준히 고민하고 문제를 경험하다 보면 속도도 따라올 것이라고 생각하기에 열심히 만든 나를 칭찬하며 마무리해본다!


```
