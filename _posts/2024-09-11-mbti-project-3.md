---
title: "[3차] 팩트폭행 MBTI 테스트 프로젝트 - 리팩토링(API,상태 관리)"
date: 2024-09-11
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

- 3차에선 리팩토링을 진행했습니다.
- 깃에서 `refactor/general` 브랜치를 통해 확인 가능

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
1. 메인 페이지 (1차에서 완료)
  - 약간의 어그로성 문구와 `테스트하기` 버튼을 통해 로그인 페이지로 이동합니다.
2. 회원가입 페이지
  - 닉네임, 아이디, 비밀번호 입력 후 회원가입 버튼을 클릭하면 "회원가입이 완료되었습니다." 문구와 함께 `로그인` 페이지로 이동합니다.
3. 로그인 페이지
  - 아이디와, 비밀번호를 입력 후 로그인 버튼을 클릭하면 "로그인이 완료되었습니다." 문구와 함께 `메인` 페이지로 이동합니다.
4. 프로필 페이지
  - 아이디, 이전 닉네임을 확인할 수 있으며, 변경 닉네임에 값을 입력 후 닉네임을 변경 버튼을 클릭하면 변경된 닉네임으로 확인이 가능합니다.
5. 테스트 페이지
  - 12개의 문항에 대해 체크 후 저장 버튼을 클릭하면, MBTI 테스트 결과가 화면에 출력됩니다.
6. 결과보기 페이지
  - MBTI 테스트 결과 리스트가 있는 페이지입니다.
    - 게시물은 본인이 쓴 게시물만 공개/비공개, 삭제 처리가 가능합니다.
      - 비공개 선택 시 해당 글은 결과보기 페이지에서 본인만 확인이 가능합니다.

## :five: 작업 목록

### :pushpin: 5-1) 효율적인 API 관리
- `queryKeys.js`

```jsx
// constants/queryKeys.js
export const QUERY_KEYS = {
  MBTI: "mbti",
  REGISTER: "register",
  LOGIN: "login",
  USER: "user",
  PROFILE: "profile"
}
```
---

```jsx
// api/...API.js
// API 요청 함수 정리되어 있음
```

---

- `mutations.jsx`

```jsx
// hooks/mutations.jsx
export const useDeleteMbti = () => {  
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: deleteTestResult,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.MBTI]);
    }
  })
}

export const useVisibilityMbti = () => {
  const queryClient = useQueryClient();
  return useMutation({
    // mutationFn : 인자를 1개만 받을 수 있음
    mutationFn: updateTestResultVisibility,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.MBTI]);
    }
  })
}

export const useRegisterUser = () => {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: handleUserRegister,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.REGISTER]);
    }
  })
}

export const useLoginUser = () => {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: handleUserLogin,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.LOGIN]);
    }
  })
}

export const useGetProfile = () => {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: getUserProfile,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.USER]);
    }
  })
}

export const useUpdateProfile = () => {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: updateProfile,
    onSuccess: () => {
      queryClient.invalidateQueries([QUERY_KEYS.PROFILE]);
    }
  })
}
```

---

- `queries.jsx`

```jsx
// hooks/queries.jsx
import { getUserProfile } from "@/api/authAPI";
import { getTestResults } from "@/api/mbtiAPI";
import { QUERY_KEYS } from "@/constants/queryKeys";
import { useQuery } from "@tanstack/react-query";

export const useMbti = () => useQuery({
  queryKey: [QUERY_KEYS.MBTI],
  queryFn: getTestResults,
})

export const useUser = () => useQuery({
  queryKey: [QUERY_KEYS.USER],
  queryFn: getUserProfile
})

```

---

- `Login.jsx`

```jsx
// Login.jsx

// 수정 전
const handleLogin = async (e) => {
  e.preventDefault();
  try{
    const loginData = { id, password }
    const response = await handleUserLogin(loginData);
    if(response.success){        
      alert("로그인되었습니다. 메인 페이지로 이동합니다.");
      login(response.accessToken);        
      navigate("/");
    }
  }catch(e){
    console.log("Login Error =>", e);
    alert("로그인이 실패했습니다.")
  }
}

// 수정 후
const { mutate: loginUser, isError } = useLoginUser();
if(isError) return <div>에러발생 에러발생.</div>

const handleLogin = (e) => {
  e.preventDefault();
  const loadingData = { id, password };
  loginUser(loadingData, {
    onSuccess: (response) => {      
      alert(`로그인에 성공했습니다. 메인 페이지로 이동합니다.`);
      login(response.accessToken);        
      navigate("/");
    },
    onError: (error) => {
      alert(error.response?.data?.message || "로그인에 실패했습니다.")
    }
  })
}
```

- `Join.jsx`

```jsx
// Join.jsx

// [수정 전]
const handleJoin = async (e) => {
  e.preventDefault();
  try{
    // 작성한 값
    const joinData = {id, password, nickname}
    // 해당 함수를 통해 데이터를 db에 전송함
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

// [수정 후]
const handleJoin = (e) => {
  e.preventDefault();
  const joinData = {id, password, nickname}
  registerUser(joinData, {
    onSuccess: (response) => {
      alert(`${response.message}, 로그인 페이지로 이동합니다.`)
      navigate("/login");
    },
    onError: (error) => {
      alert(error.response?.data?.message || "회원가입에 실패했습니다.");
    }
  })
}
```

## :joystick: Trouble Shooting
작성중


## :fire: 회고
이번 리팩토링을 진행하며 개선된 점은 아래와 같다.

1. queryKey 
  - 데이터를 효율적으로 관리하고 쉽게 재사용 할 수 있게됌 
2. API 인스턴스 설정 파일
  - 별도 파일로 관리하며, API 호출의 일관성을 유지하고 한 곳에서 수정이 가능해서 용이함
3. API 요청 함수
  - 해당 함수를 분리하여 코드의 재사용성을 높이고, API 호출 로직을 명확하게 처리하게됌
4. 커스텀 훅
  - 복잡한 로직을 각각의 컴포넌트에서 분리하고 상태관리와 API 요청을 이전보다 간결하게 처리함

리팩토링을 통해 유저 API와 MBTI API 관련 로직을 명확히 분리하여 각 기능에 맞는 커스텀 훅을 생성하였고, 이로 인해 복잡한 로직을 간결하게 유지할 수 있었다. 중복 코드를 줄이고, 향후 유지보수에도 유리할 것이라 생각하고 특히 팀 프로젝트에서 이러한 구조는 유용할 것 같다고 생각함.

### :pushpin: Keep - 현재 만족하고 있는 부분
- 중복 코드를 제거함
- 커스텀 훅을 사용해서 복잡한 로직을 간결하게 처리해봄
- 코드의 일관성을 유지하고 유지보수가 좀 더 편리하게 수정해봄

### :pushpin: Problem - 불편하게 느끼는 부분
- 문법적으로는 익숙해졌지만 동작 방식에 대해 완벽하게 이해하지 못하는 것 같음
- 컴포넌트에서는 간결해진 것 같은데, 폴더가 많이 복잡한 것 같음

### :pushpin: Try - problem에 대한 해결책, 당장 실행 가능한 것
- 리팩토링을 통해 효율적으로 관리할 수 있게 된 것 같지만 나의 코드를 처음 보는 사람들 입장에서 조금 더 쉽게 이해할 수 있도록 커스텀 훅과 각 기능별로 문서화를 하면 어떨까란 생각을 해봄

### :pushpin: 관련 글
- [[1차] 팩트폭행 MBTI 테스트 프로젝트 - 초기 세팅 및 회원 기능](https://rarrit.github.io/react/mini/mbti-project-1/)
- [[2차] 팩트폭행 MBTI 테스트 프로젝트 - MBTI 테스트 기능 적용](https://rarrit.github.io/react/mini/mbti-project-2/)
- [[3차] 팩트폭행 MBTI 테스트 프로젝트 - 리팩토링(API,상태 관리)](https://rarrit.github.io/react/mini/mbti-project-3/)