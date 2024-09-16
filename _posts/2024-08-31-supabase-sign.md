---
title: "React + Supabase 시작하기 (2), 회원가입 및 로그인"
date: 2024-08-31
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - supabase
  - til
tags:
  - supabase
  - database
author_profile: true
sidebar_main: true
---

## :ledger: Supabase 로그인,로그아웃,회원가입 기능을 추가해보자.

`Supabase`를 사용하여 리액트에서 로그인,로그아웃,회원가입 기능 추가 방법입니다.

### :one: user table 생성 및 참조(Table Editor)

`supabase`의 좌측 메뉴 Authentication -> Users 를 보면 기본적으로 `supabase`에서 유저 테이블을 관리를 해준다. 하지만 보안상의 문제로 [공식 문서](https://supabase.com/docs/guides/auth/managing-user-data)에서 추천하지 않는다. 그렇기에 <u>public 스키마에서 user 테이블을 만들고 auth 스키마에있는 users 테이블을 참조</u>하는 방법으로 진행한다.

#### :pushpin: 1-1) user 테이블 생성

- `Supabase` 프로젝트 좌측 메뉴인 `Table Editor`에 들어가서 `Create a new Table`를 클릭하여 생성해준다.

#### :pushpin: 1-2) user 테이블 세팅

- 유저 테이블에 필요한 값을 설정해주고 `Enable Row Level Security (RLS)` 체크를 해제한다.
  - 보안 관련된 내용인데, 명확히 이해하지 못했고 해당 부분을 체크하면 DB에 값이 저장되지 않아서 해제했다. 자세한 내용은 [공식문서](https://supabase.com/docs/guides/database/postgres/row-level-security) 에서 확인이 가능하다.

![supabase table set1](https://github.com/user-attachments/assets/75d24cd8-9242-4307-863e-cb527f0d1394)

#### :pushpin: 1-3) auth의 users 참조 세팅

- 테이블에 값을 설정했으면, id 옆의 파일 아이콘을 클릭하여, `auth.users`를 참조할 수 있게 설정해준다.

![supabase table set2](https://github.com/user-attachments/assets/253311bf-9004-452d-90b2-6c96c7cebe42)

#### :pushpin: 1-4) auth의 users 연동

- 1-3에서 참조 설정 후 SQL코드를 입력하여 연동해줘야한다.
- [공식문서](https://supabase.com/docs/guides/auth/managing-user-data)를 참고하여 아래 이미지와 같이 연결해주고 이미지에 없지만 SQL문 하단의 `Run`을 클릭하여 실행시키면 된다.

![supabase table set3](https://github.com/user-attachments/assets/3267f988-7099-464d-b29b-613685d92a35)

### :two: auth 설정

`supabase` 좌측 메뉴의 Authentication를 들어가면 Users 메뉴가 보인다. 이 부분이 `supabase`에서 관리해주는 유저 테이블이다. [1-3]에서 참조해주었으니 유저가 추가(회원가입)될 때 마다 자동적으로 public 스키마의 user 테이블에도 자동으로 값이 추가된다.

#### :pushpin: 2-1) email 설정

`Authentication` 메뉴에 Providers 탭에 들어가면 다양한 로그인 방식이 있다. 기본적으로 `Email`로 세팅되어있는데, 이 부분에서 반드시 설정해줘야 할 것이 있다. (처음에 이 부분을 몰라서 1시간 넘게 고생함..) 그것은 바로 `Confirm email`을 해제해줘야 하는데 이 항목이 무엇이냐면 <u>회원가입 할 때 사용자가 메일에서 확인을 해줘야한다는 것</u>이다. 일단 기본 사용법을 연습하고 있던 나는 필요 없는 기능이라 체크를 해제해주었다.

![supabase email set](https://github.com/user-attachments/assets/e5cbbb68-28b8-4546-b6a4-f743ed6ed35a)

### :three: 회원가입 (SignUp.jsx)

#### :pushpin: 3-1) 작성 코드

```jsx
// import
import { useState } from "react";
import { supabase } from "../supabaseClient";
import { useNavigate } from "react-router-dom";

const SignUp = () => {
  const navigate = useNavigate();

  // state 처리 : input 의 값을 담아줌
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSignUp = async (e) => {
    // 새로고침 방지
    e.preventDefault();

    // supabase의 signUp 메서드를 상요하여, user의 email, password 값을 추가해줌
    await supabase.auth.signUp({
      email,
      password,
    });

    // 위의 코드가 실행된 후 alert 메세지 출력
    alert("축하합니다! 회원가입이 완료됐어요. 로그인 페이지로 이동할께요.");
    // useNavigate 사용하여, 로그인 페이지로 이동
    navigate("/signin");
  };
};

export default SignUp;
```

#### :pushpin: 3-2) 실행 결과

`signup` 페이지에서 회원가입 버튼을 클릭하면, auth 스키마와 public 스키마의 user가 연동되어 추가된 모습을 볼 수 있다.

![auth user](https://github.com/user-attachments/assets/7b2d8c67-7c8a-47c6-92b6-abe84bf13ce6)

![public user](https://github.com/user-attachments/assets/445c111a-5b34-49c0-92a3-fe2cc578cb70)

### :four: 유저 데이터 전역 관리 (UserContext.jsx)

회원가입 진행 후 로그인을 하려면, DB에 저장된 user의 정보를 가져와야한다. user의 정보는 회원가입 뿐만아니라, 다양한 처리를 해주기 위해 context API를 사용하여 전역으로 관리한다.

#### :pushpin: 4-1) 작성 코드

```jsx
// 처음 렌더링될 때 user의 값은 불러져와 있지 않기 때문에 null로 설정한다.
const [user, setUser] = useState(null);

// 컴포넌트가 처음 랜더링될 때 유저의 데이터를 한번만 받아오기 위해 useEffect를 사용한다.
useEffect(() => {
  // 비동기 함수로, Supabase의 getSession 메서드를 사용하여 현재 사용자의 세션 정보를 가져온다
  const getSession = async () => {
    const response = await supabase.auth.getSession();
    // supabase의 메서드를 사용하여, user의 데이터를 state에 저장한다.
    setUser(response.data.session.user);
  };

  getSession();
}, []);
```

### :four: 로그인 (SignIn.jsx)

#### :pushpin: 4-1) 작성 코드

```jsx
// import
import { useContext, useState } from "react";
import { supabase } from "../supabaseClient";
import { UserContext } from "../contexts/UserContext";

const SignIn = () => {
  // 현재 사용자의 정보를 가져옴
  const { user } = useContext(UserContext);

  // 로그인한 값을 처리하기 위해 state 생성
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSignIn = async (e) => {
    // 새로고침 방지
    e.preventDefault();

    try {
      //  Supabase의 인증 API를 사용하여 이메일과 비밀번호로 로그인한다.
      await supabase.auth.signInWithPassword({
        email,
        password,
      });
    } catch (error) {
      console.log("로그인 error => ", error);
    }
  };

  return (
    <div>
      <h1>Sign In</h1>
      <form onSubmit={handleSignIn}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <button type="submit">Sign In</button>
      </form>
    </div>
  );
};

export default SignIn;
```

### :five: 로그아웃

로그아웃 처리는 간단하다. supabase의 메서드인 `signOut`을 사용하여 클릭시 실행하면 된다.

- 자세히 알 고 싶으면 [공식문서](https://supabase.com/docs/guides/auth/signout)를 추천!

#### :pushpin: 5-1) 작성 코드

```jsx
const { setUser } = useContext(UserContext);
const handleSignOut = async () => {
  // 로그아웃
  await supabase.auth.signOut();
  // 로그아웃 이후 상태를 null 변경해줌
  setUser(null);
};
```

### :six: 라우팅 처리

로그인 여부를 확인하여 로그인 되어있을 경우 접근,접근 불가한 페이지를 설정한다.

#### :pushpin: 6-1) 로그인일 때 접근 불가능하게 처리

```jsx
const AuthRoute = () => {
  // 유저를 전달 받음
  const { user } = useContext(UserContext);
  if (user) {
    alert("현재 로그인된 상태입니다.");
    // 로그인 되어있을 경우 접속이 불가능한 페이지는 아래와 같이 메인 페이지로 이동시킴
    return <Navigate to="/" />;
  }
  return <Outlet />;
};

// 로그인 되어잇을 경우 로그인,로그아웃 페이지에 접근하면 메인 페이지로 이동
<Route element={<AuthRoute />}>
  <Route path="/signin" element={<SignIn />} />
  <Route path="/signup" element={<SignUp />} />
</Route>;
```

#### :pushpin: 6-2) 로그인이 되어있어야 접근 가능하게 처리

```jsx
const PrivateRoute = () => {
  // 유저를 받아옴
  const { user } = useContext(UserContext);
  if (!user) {
    alert("로그인이 필요한 페이지입니다.");
    // 로그인 페이지로 이동시킴
    return <Navigate to="signin" />;
  }
  return <Outlet />;
};
```

### :fire: 마무리

요번 팀 과제에서 나의 역할에 로그인,로그아웃이 없지만 한 번 구현해보고싶은 마음이 컷었다. `supabase`를 처음 사용하면서 아무래도 세팅하는 부분이 정말 어려웠던 것 같다.. 이 부분만 잘 세팅되고 데이터를 전달받기만 한다면 개인적으로 70%는 성공한 것 같은...
