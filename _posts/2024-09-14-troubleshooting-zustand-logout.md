---
title: "로그인 실행 시 상태값 변환으로 리렌더링되어 publicRoute의 alert이 출력되는 현상"
date: 2024-09-14
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:  
  - troubleshooting
tags:
  - troubleshooting
author_profile: true
sidebar_main: true
---

## :ledger: Zustand를 사용해서 로그인 처리중 겪은 에러입니다.

### :one: 개요

![zustand-double-alert](https://github.com/user-attachments/assets/23ae6d76-1ff4-473e-8c98-5d47281c9403)

`Zustand`를 사용해서 로그인 처리했을 때 "로그인이 되었습니다. 메인페이지로 이동합니다." 이후 "현재 로그인되어있습니다."라는 문구가 출력되어 문제를 해결하려했다. 적용된 코드는 아래와 같다.

```jsx
// zustand
const useAuthStore = create(
  persist(
    (set) => ({
      accessToken: "",
      avatar: "",
      nickname: "",
      success: false,
      userId: "",
      isLoggedIn: false,
    
      setAccessToken: ( token ) => set({ accessToken: token }),
      setAvatar: ( avatar ) => set({ avatar }),
      setNickname: ( nickname ) => set({ nickname }),
      setSuccess: ( success ) => set({ success }),
      setUserId: ( userId ) => set({ userId }),
      setIsLoggedIn: ( isLoggedIn ) => set({ isLoggedIn }),
      // 로그아웃 처리 
      logout: () => set({
        accessToken: null,
        nickname: null,
        userId: null,
        isLoggedIn: false
      })
    }),
    {
      name: "auth-storage",
      getStorage: () => localStorage,
    }
  )
)

// Login.jsx
import AuthForm from "@/components/auth/AuthForm"
import { handleUserLogin, handleUserProfile } from "@/core/api/authAPI"
import useAuthStore from "@/core/store/authStore"
import { useNavigate } from "react-router-dom"
import styled from "styled-components"

const Login = () => {
  const navigate = useNavigate();
  const {setAccessToken, setNickname, setUserId } = useAuthStore();

  const handleLogin = async (formData) => {
    try{
      const loginData = await handleUserLogin(formData);
      setAccessToken(loginData.accessToken);
      setNickname(loginData.nickname);
      setUserId(loginData.userId);    
      setIsLoggedIn(true);
      alert("로그인 되었습니다. 메인으로 이동합니다.");
      navigate("/");
    }catch(e){
      console.log("에러가 발생했습니다.", e);
    }
  }

  return (
    <StLoginArea>
      <AuthForm mode={"login"} onSubmit={handleLogin}/>
    </StLoginArea>
  )
}

const StLoginArea = styled.div`
  
`

export default Login
```

### :two: 분석 및 처리
처음 코드를 분석했을 때 로그아웃 후 `PublicRoute.jsx`에서 alert이 출력된다는건 상태값 변경으로 리렌더링이 된다고 생각했다. 그렇다면, `publicRoute.jsx`에서 로그인 처리 후 리렌더링이 되지 않아야되는데, 어떻게 처리를 해야할까 고민을 많이했다. 코드는 아래와 같다.

```jsx
import useAuthStore from '@/core/store/authStore';
import React from 'react'
import { Navigate } from 'react-router-dom'

const PublicRoute = ({ children }) => {
  const { isLoggedIn } = useAuthStore();
  if(isLoggedIn) {
    alert("현재 로그인되어있습니다.");
    return <Navigate to="/"/>
  }
  return children;
}

export default PublicRoute
```

#### :pushpin: 2-1) 시도해봄 1
처음 해당 코드에서 if문으로 useLocation을 사용해서 해당 url값에서 로그인 처리가 될 때 alert이 노출되지 않게 처리했지만, 만약 사용자가 로그인된 상태에서 url로 /login 페이지에 이동하려 했을 때 alert이 출력되지 않으면 그것 또한 오류라고 판단했다. 그렇다면 결국 publicRoute에서 처리하는것은 아니라고 생각하고 다른 방법을 찾아봤다.


#### :pushpin: 2-2) 시도해봄 2
그렇다면 일단 메인페이지로 이동시키고 상태값을 변경해주면 안될까? 하는 생각을 했다. 예를들면, 아래의 코드와 같다.

```jsx
  const handleLogin = async (formData) => {
    try{            
      alert("로그인 되었습니다. 메인으로 이동합니다.");
      navigate("/");  
      const loginData = await handleUserLogin(formData);
      setAccessToken(loginData.accessToken);
      setNickname(loginData.nickname);
      setUserId(loginData.userId);    
      setIsLoggedIn(true);
    }catch(e){
      console.log("에러가 발생했습니다.", e);
    }
  }
```

된다.. 근데 이것이 맞을까? 이렇게 처리를 해도 괜찮을까? 하는 걱정이 있다. 현재 추석 연휴 기간이라 튜터님을 뵙지 못하지만 한번 물어봐야겠다.


#### :pushpin: 2-3) 시도해봄 3 (추천받음)
같은 기수의 대학캠퍼스 첫사랑 선배느낌st 나는 박X호님께서 추천해준 방법도 있었다. 내가 시도했던 방법과 비슷한데, navigate("/") 에서 state값을 전달해서 메인으로 이동 후 리렌더링 하는 방식이었다. 2번에서 시도한 방법은 뭔가 조금 찜찜했지만 이 방법은 찜찜한 느낌이 더 없어서 .. 이 방법으로 적용을했다. 코드는 아래와 같다.

```jsx
// Login.jsx
  const handleLogin = async (formData) => {
    try{
      const loginData = await handleUserLogin(formData);
      setAccessToken(loginData.accessToken);
      setNickname(loginData.nickname);
      setUserId(loginData.userId);    
      alert("로그인 되었습니다. 메인으로 이동합니다.");
      // state 값을 메인으로 전달함
      navigate("/", {
        state: true
      });
    }catch(e){
      console.log("에러가 발생했습니다.", e);
    }
  }

// Main.jsx
  const { state } = useLocation();
  const { setIsLoggedIn } = useAuthStore();
  useEffect(() => {
    setIsLoggedIn(state);
  }, [state]);

```




### :three: 해결방법
2,3 번 둘 다 성공했는데, 이 내용을 튜터님께 보여주고 피드백을 받으면 다시 글을 수정해보려 한다.

### :fire: 마무리
시도해본 것 중 2,3 번이 둘 다 정상적으로 작동은 되는데 아직 허접인 내 입장으로서는 뭐가 확실히 좋은거야! 라고 판단하긴 어렵다. 그러나 같은 기수의 사람들인 이X곤,이X빈,박X호,강X우 분들에게도 감사하고 같이 고민하는 그 시간이 좋았던 것 같다.