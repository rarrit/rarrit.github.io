---
title: "[1차] 팩트폭행 MBTI 테스트 프로젝트 - 초기 세팅 및 회원 기능"
date: 2024-09-09
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - mini
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: REACT MBTI TEST PROJECT

![1  mbti-test-main](https://github.com/user-attachments/assets/8d85374b-7b5e-4db3-a19a-35bd9fb1f9f5)

## :rocket: 베포 링크

- vercel
  - [https://fact-mbti-test.vercel.app/](https://fact-mbti-test.vercel.app/)
- github
  - [https://github.com/rarrit/react-mbti-test](https://github.com/rarrit/react-mbti-test)

## :one: 프로젝트 설명

팩트로 폭행하는 MBTI TEST 입니다. F는 상처받을 수 있으니 테스트 진행 시 주의 요망

## :two: STACK

![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white) ![Context-API](https://img.shields.io/badge/Context--Api-000000?style=for-the-badge&logo=react) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens) ![React Query](https://img.shields.io/badge/-React%20Query-FF4154?style=for-the-badge&logo=react%20query&logoColor=white) ![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

## :three: 프로젝트 구조

- 1차에선 프로젝트 초기 세팅 및 로그인,회원가입,프로필 수정 기능을 적용했다.
- 깃에서 `feat/user-auth` 브랜치를 통해 확인 가능

<details>
<summary>전체 프로젝트 구조</summary>
📦src<br/>
 ┣ 📂api<br/>
 ┃ ┣ 📜authAPI.js<br/>
 ┃ ┗ 📜mbtiAPI.js<br/>
 ┣ 📂assets<br/>
 ┃ ┣ 📂img<br/>
 ┃ ┃ ┣ 📂mbti<br/>
 ┃ ┗ 📜react.svg<br/>
 ┣ 📂components<br/>
 ┃ ┣ 📂footer<br/>
 ┃ ┣ 📂header<br/>
 ┃ ┣ 📜GlobalStyle.jsx<br/>
 ┃ ┣ 📜Layout.jsx<br/>
 ┃ ┣ 📜ProtectedRoute.jsx<br/>
 ┃ ┣ 📜TestForm.jsx<br/>
 ┃ ┣ 📜TestResultItem.jsx<br/>
 ┃ ┗ 📜TestResultList.jsx<br/>
 ┣ 📂constants<br/>
 ┃ ┗ 📜queryKeys.js<br/>
 ┣ 📂context<br/>
 ┃ ┗ 📜AuthContext.jsx<br/>
 ┣ 📂data<br/>
 ┃ ┗ 📜questions.js<br/>
 ┣ 📂hooks<br/>
 ┃ ┣ 📜mutations.jsx<br/>
 ┃ ┗ 📜queries.jsx<br/>
 ┣ 📂instance<br/>
 ┃ ┗ 📜baseInstance.js<br/>
 ┣ 📂pages<br/>
 ┃ ┣ 📜Join.jsx<br/>
 ┃ ┣ 📜Login.jsx<br/>
 ┃ ┣ 📜Main.jsx<br/>
 ┃ ┣ 📜Mypage.jsx<br/>
 ┃ ┣ 📜TestPage.jsx<br/>
 ┃ ┗ 📜TestResultPage.jsx<br/>
 ┣ 📂shared<br/>
 ┃ ┗ 📜Router.jsx<br/>
 ┣ 📂utils<br/>
 ┃ ┣ 📜dateResult.js<br/>
 ┃ ┣ 📜mbtiCalculator.js<br/>
 ┃ ┣ 📜mbtiImg.js<br/>
 ┃ ┗ 📜mbtiResult.js<br/>
 ┣ 📂zustand<br/>
 ┃ ┗ 📜userStore.js<br/>
 ┣ 📜App.css<br/>
 ┣ 📜App.jsx<br/>
 ┣ 📜index.css<br/>
 ┗ 📜main.jsx<br/>
</details>

## :four: 프로젝트 세팅 및 기능 설명
### :pushpin: 4-1) 설치 패키지

1. Json-server
2. Tanstack query
3. zustand
4. Axios
5. Tailwind
6. styled-component
7. react-router-dom
8. the-new-css-reset
9. lucide-react

### :pushpin: 4-2) 주요 기능 소개
1. **메인 페이지**
  - 약간의 어그로성 문구와 `테스트하기` 버튼을 통해 로그인 페이지로 이동합니다.
2. **회원가입 페이지**
  - 닉네임, 아이디, 비밀번호 입력 후 회원가입 버튼을 클릭하면 "회원가입이 완료되었습니다." 문구와 함께 `로그인` 페이지로 이동합니다.
3. **로그인 페이지**
  - 아이디와, 비밀번호를 입력 후 로그인 버튼을 클릭하면 "로그인이 완료되었습니다." 문구와 함께 `메인` 페이지로 이동합니다.
4. **프로필 페이지**
  - 아이디, 이전 닉네임을 확인할 수 있으며, 변경 닉네임에 값을 입력 후 닉네임을 변경 버튼을 클릭하면 변경된 닉네임으로 확인이 가능합니다.
5. **테스트 페이지**
  - 12개의 문항에 대해 체크 후 저장 버튼을 클릭하면, MBTI 테스트 결과가 화면에 출력됩니다.
6. **결과보기 페이지**
  - MBTI 테스트 결과 리스트가 있는 페이지입니다.
    - 게시물은 본인이 쓴 게시물만 공개/비공개, 삭제 처리가 가능합니다.
      - 비공개 선택 시 해당 글은 결과보기 페이지에서 본인만 확인이 가능합니다.

## :five: 작업 목록
### :pushpin: 5-1) 레이아웃 및 라우터 설정

- **레이아웃**
  - Layout.jsx 컴포넌트에서 Header,Footer 및 초기 레이아웃을 설정해줌

```jsx
  const Layout = ({ children }) => {
    return (
      <StContainer>
        <Header />
        <StContents>
          { children }
        </StContents>
        <Footer />
      </StContainer>
    )
  }
  const StContainer = styled.div``
  const StContents = styled.div``
  export default Layout
```

- **라우터 설정 (Protected Route)**
  - 이번 프로젝트에서 회원 여부에 따라 접근 가능한 페이지를 설정하기 위해 `Protected Route`설정을 해주었다.
  - context API를 사용하여 `isLogin` 값으로 라우터 처리 및 메뉴도 로그인 여부에 따라 노출되게 설정했다.

```jsx
  import { AuthContext } from '@/context/AuthContext';
  import { useContext } from 'react';
  import { Navigate } from 'react-router-dom';

  const ProtectedRoute = ({ children, redirectIsLogin, redirectNotLogin}) => {
    const { isLogin } = useContext(AuthContext);

    // 로그인된 상태에서 접근할 수 없는 페이지 처리
    if (isLogin && redirectIsLogin) {    
      alert("이미 로그인되어 있습니다.");
      return <Navigate to={redirectIsLogin} />;
    }

    // 로그인된 상태에서 접근할 수 없는 페이지 처리
    if (!isLogin && redirectNotLogin) {    
      alert("로그인이 필요한 페이지입니다.");
      return <Navigate to={redirectNotLogin} />;
    }

    // 로그인되지 않은 상태에서 접근할 수 없는 페이지 처리
    return children
  };

  export default ProtectedRoute;
```


### :pushpin: 5-2) 로그인
로그인 페이지에서 아이디, 비밀번호를 입력 후 버튼을 클릭했을 때 로그인 처리가 되어야 한다.

![2  mbti-test-login](https://github.com/user-attachments/assets/dcf2d29e-35ca-43cc-acb6-19cd961ac115)


```jsx
const [id, setId] = useState("");
const [password, setPassword] = useState("");
const navigate = useNavigate();
const { login } = useContext(AuthContext);

const handleLogin = async (e) => {
  e.preventDefault();
  try{
    const loginData = { id, password }
    const response = await handleUserLogin(loginData);
    if(response.success){        
      alert("로그인되었습니다. 메인 페이지로 이동합니다.")
      login(response.accessToken);
      navigate("/");
    }
  }catch(e){
    console.log("Login Error =>", e);
    alert("로그인이 실패했습니다.")
  }
}
```

1. **상태관리**
  - `useState`를 사용해서 is,password를 입력받아 저장한다.
2. **AuthContext**
  - context 의 login 상태 관리 함수를 가져온다.
  - login(token)
    - ```jsx
      const login = (token) => {
        localStorage.setItem("accessToken", token);
        setLogin(true);
      }
      ```
3. **useNavigate**
  - 라우터 훅을 사용하여 로그인 후 페이징 처리
4. **handleLogin**
  - 로그인 처리 함수이며, 사용자가 폼을 제출하면 실행된다.
  - loginData(사용자가 입력한 아이디,비밀번호를 객체로 생성)
  - API 호출
    - `handleUserLogin(loginData)` 함수를 통해 서버에 로그인 요청을 보내고 성공 시 서버에서 받은 `accessToken`을 반환함
  - 해당 토큰으로 `login(token)` 실행 시 로컬 스토리지에 토큰 값이 저장되고 현재 로그인상태(`setLogin(true)`)가 변경됨

### :pushpin: 5-3) 회원가입
회원가입 페이지에서 아이디, 비밀번호. 닉네임를 입력 후 버튼을 클릭했을 때 입력한 데이터가 저장(회원가입) 되어야 한다.

![3  main-test-join](https://github.com/user-attachments/assets/0ba69d86-15fd-47b7-b134-a414e07cc298)

```jsx
const [id, setId] = useState("");
const [password, setPassword] = useState("");
const [nickname, setNickname] = useState("");
const navigate = useNavigate();

const handleJoin = async (e) => {
  e.preventDefault();
  try{
    const joinData = {
      id,
      password,
      nickname
    }
    const response = await handleUserRegister(joinData);
    if(response.success) {
      alert("회원가입이 완료되었습니다. 로그인 페이지로 이동합니다.")
      navigate("/login");
    }else{
      alert(response.message || "회원가입이 실패했습니다.");
    }
  }catch(e){
    console.log("Join Error =>", e.response || e);
    alert("회원가입을 실패했습니다.")
  }
}
```

1. **상태관리**
  - `useState`를 사용해서 is,password,nickname를 입력받아 저장한다.
2. **useNavigate**
  - 라우터 훅을 사용하여 로그인 후 페이징 처리
4. **handleJoin**
  - 회원가입 처리 함수이며, 사용자가 폼을 제출하면 실행된다.
  - JoinData(사용자가 입력한 아이디,비밀번호,닉네임를 객체로 생성)
  - API 호출
    - `handleUserRegister(JoginData)` 함수를 통해 서버에 로그인 요청을 보냄
      - 성공 시 알럿 실행 및 로그인 페이지로 이동
      - 실패 시 에러 메세지 출력 


### :pushpin: 5-4) 마이페이지
회원가입 페이지에서 아이디, 비밀번호. 닉네임를 입력 후 버튼을 클릭했을 때 입력한 데이터가 저장(회원가입) 되어야 한다.

![4  mbti-test-mypage](https://github.com/user-attachments/assets/e728fb08-59f0-47cc-abc1-00c89f4c0ea1)

```jsx
const [id, setId] = useState("");
const [password, setPassword] = useState("");
const [nickname, setNickname] = useState("");
const navigate = useNavigate();

const handleJoin = async (e) => {
  e.preventDefault();
  try{
    const joinData = {
      id,
      password,
      nickname
    }
    const response = await handleUserRegister(joinData);
    if(response.success) {
      alert("회원가입이 완료되었습니다. 로그인 페이지로 이동합니다.")
      navigate("/login");
    }else{
      alert(response.message || "회원가입이 실패했습니다.");
    }
  }catch(e){
    console.log("Join Error =>", e.response || e);
    alert("회원가입을 실패했습니다.")
  }
}
```

1. **상태관리**
  - useState를 사용하여 사용자가 입력한 아이디, 비밀번호, 닉네임을 각각 id, password, nickname 상태에 저장합니다. 이 상태들은 input 필드의 값과 연결되어 있으며, 사용자가 입력할 때마다 상태가 업데이트됩니다.
2. **useNavigate**
  - 라우터의 훅인 `useNavigate` 로그인이 되어 있지 않은 사용자를 로그인 페이지로 리다이렉트한다.
3. **useEffect**
  - 컴포넌트가 처음 렌더링될 때, 로그인이 되어 있는지 확인하고, 로그인이 안 되어 있으면 로그인 페이지로 이동한다. 로그인이 되어 있으면 서버로부터 사용자 정보를 가져와 상태에 저장한다.
4. **닉네임 변경 처리**
  - 사용자가 새 닉네임을 입력하고 제출하면 `handleNicknameChange` 함수가 실행된다. 닉네임 변경 요청을 서버에 보내고, 성공 시 사용자 정보 상태를 업데이트하며 알림을 띄운다. 실패 시 오류 메시지를 출력한다.

## :fire: 회고
로그인,회원가입,마이페이지까지 개인과제 가이드에 작성된 내용이다. 처음에 복붙을 진행하며 기분이 조금.. 그랬지만 가이드 해준 코드가 아마 내가 안보고 만든 것 보다 더 효율적으로 작성했을 거라 생각해서 코드를 이해하는 방향으로 진행했다. 이후 Tanstack Query의 커스텀 훅으로 변경하고 사용할 예정이다.

### :pushpin: Keep - 현재 만족하고 있는 부분
가이드로 제공한 코드를 보며 이해가 안되는 부분이 없었다는 것

### :pushpin: Problem - 불편하게 느끼는 부분
복붙 후 이해하기 보다는 직접 만들어보는 게 더 좋았을 거란 걸 알지만 시간에 쫒기며 편함을 택한 나에게 용서를..

### :pushpin: Try - problem에 대한 해결책, 당장 실행 가능한 것
프로젝트가 끝나면 항상 해설영상이 있기에 이번 개인과제 제출 후 안보고 작성하고 해설 영상을 통해 문제점을 찾아볼것이다.

### :pushpin: 관련 글
- [[1차] 팩트폭행 MBTI 테스트 프로젝트 - 초기 세팅 및 회원 기능](https://rarrit.github.io/react/mini/mbti-project-1/)
- [[2차] 팩트폭행 MBTI 테스트 프로젝트 - MBTI 테스트 기능 적용](https://rarrit.github.io/react/mini/mbti-project-2/)
- [[3차] 팩트폭행 MBTI 테스트 프로젝트 - 리팩토링(API,상태 관리)](https://rarrit.github.io/react/mini/mbti-project-3/)