---
title: "[mjt] 복잡한 코드에서 디버깅을 통해 실행 단계를 확인하자"
date: 2024-12-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - debugging
author_profile: true
sidebar_main: true
---

## :ledger: 복잡한 코드는 디버깅을 통해 안전하게 작성하기

`디버깅(debugging`은 스크립트 내 에러를 검출해 제거하는 일련의 과정을 의미한다. 모던 브라우저와 호스트 환경 대부분은 개발자 도구 안에 UI 형태로 디버깅 툴을 구비해 놓으며, 해당 툴을 사용하면 디버깅이 훨씬 쉬워지고 실행 단계마다 어떤 일이 일어나는지를 코드 단위로 추적이 가능하다.

### :one: 소스(Sources) 패널

![debugging-sources-image](https://github.com/user-attachments/assets/e48a2832-0bdd-4535-b18b-fa347d521a10)

`소스(Sources) 패널`은 크게 세 개의 영역으로 구성된다.

1. **_파일 탐색 영역_**: 페이지를 구성하는 데 쓰인 모든 리소스를 트리 형태로 보여준다.
2. **_코드 에디터 영역_**: 리소스 영역에서 선택한 파일의 소스 코드를 보여준다.
3. **_자바스크립트 디버깅 영역_**: 디버깅에 관련된 기능을 제공한다.

### :two: 중단점(breakpoint)

`중단점(breakpoint)`은 자바스크립트의 실행이 중단되는 코드 내 지점을 의미한다. 중단점을 이용하면 <u>실행이 중지된 시점에 변수가 어떤 값을 담고 있는지</u> 알 수 있으며 실행이 중지된 시점을 기준으로 명령어를 실행할 수 있다.

![debugging-breakpoint-image](https://github.com/user-attachments/assets/acadfee5-c8e3-48ed-8556-6df903b4757e)

위의 이미지와 같이, 소스 패널에서 코드의 라인을 클릭하면 우측에서 중단점을 확인할 수 있으며, 코드가 실행될 때 어떤 값을 담고 있는지 확인할 수 있다.

#### :pushpin: 2-1) 조건부 중단점

줄 번호에 커서를 옮긴 후 마우스 오른쪽 버튼을 클릭하면 `조건부 중단점`을 설정할 수 있다. `Add conditional breakpoint`를 클릭했을 때 뜨는 작은 창에 표현식을 입력하면, 표현식이 **참인 경우**에만 실행을 중지시킬 수 있다. 조건부 중단점을 설정하면 <u>변수에 특정 값이 할당될 때나 함수의 매개 변수에 특정 값이 들어올 때만 실행을 중단</u>시킬 수 있어 디버깅 시 유용하게 활용할 수 있다.

### :three: debugger 명령어

`debugger` 명령어를 사용하면 브라우저를 켜 개발자 도구를 열고 소스 코드 영역을 띄워 중단점을 설정할 필요가 없어진다. 에디터를 떠나지 않고 중단점을 설정할 수 있기에 편리하다.

```javascript
function hello(name) {
  let phrase = `Hello, ${name}!`;

  debugger; // <-- 여기서 실행이 멈춥니다.

  say(phrase);
}
```

### :fire: 회고

디버깅을 많이 사용한 기억은 없다. 하지만 같이 프로젝트를 진행했을 때 문제가 어느 지점에서 발생하는지 찾을 때 유용했던 기억이 있고, 개발자 도구보다는 `debugger` 명령어를 사용한 것이 조금 더 편리했다.

#### :pushpin: 참고 문서

- [ko.javascript.info - Chrome으로 디버깅하기](https://ko.javascript.info/debugging-chrome)
