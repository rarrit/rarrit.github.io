---
title: "폼 관리에 유용한 Zod와 React Hook Form이란 무엇인지 알아보자"
date: 2024-10-09
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - react
  - lib
tags:
  - react hook form
  - zod
author_profile: true
sidebar_main: true
---

## :ledger: Zod와 React Hook Form이란?
React로 폼을 관리할 때 유용한 두 가지 도구가 있다. 바로 **React Hook Form**과 **Zod**이다. 두 라이브러리는 폼을 쉽고 강력하게 관리할 수 있도록 돕는 도구이며. Zod와 React Hook Form을 어떻게 함께 사용할 수 있는지 알아보자

### :one: 제어 컴포넌트와 비제어 컴포넌트
React Hook Form과 Zod에 대해 알기 전에 제어 컴포넌트와 비제어 컴포넌트가 무엇인지 알아보자.

#### :pushpin: 1-1) 제어 컴포넌트
제어 컴포넌트는 <u>사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트</u>한다. React에서 변경할 수 있는 state가 일반적으로 컴포넌트의 state 속성에 유지되며 useState()에 의해 업데이트 된다.

- 아래의 코드에서 inputValue가 `setInputValue`를 통해 변경될 때 마다 리렌더링 된다.

```tsx
import React, { useState } from 'react';

function MyInput() {
  const [inputValue, setInputValue] = useState(null);

  const handleChange = (e) => {
    setInputValue(e.target.value);
  };

  return <input onChange={(e) => handleChange(e)} value={inputValue} />;
}
```

#### :pushpin: 1-2) 비 제어 컴포넌트
반대로 React에서 값이 제어되지 않는 컴포넌트를 비 제어 컴포넌트라고 한다.

- 아래의 코드에서 값이 변화하더라도 input을 제외한 필드가 리렌더링 되지 않는다.
  - `useRef`를 사용해 `inputNode`를 `input`요소에 직접 연결한다. useState를 사용하지 않기 때문에 입력 필드의 값이 변화해도 React는 이를 상태 변경으로 인식하지 않으며, 컴포넌트 리렌더링이 발생하지 않는다.
```tsx
import React, { useRef } from 'react';

function Test() {
  return <MyInput />;
}

function MyInput() {
  const inputNode = useRef(null);

  return <input ref={inputNode} />;
}

export default MyInput;
```

#### :pushpin: 1-3) 제어 vs 비 제어 컴포넌트

- 즉각적으로, 실시간으로 값에 대한 피드백이 필요하다 → `제어 컴포넌트 사용`
- 즉각적인 피드백이 불필요하고 제출시에만 값이 필요하다, 불필요한 렌더링과 값 동기화가 싫다 → `비 제어 컴포넌트 사용`

| `기능` | `제어 컴포넌트` | `비 제어 컴포넌트` | 
| 일회성 정보 검색 (예: 제출) | O | O | 
| 제출 시 값 검증 | O | O | 
| 실시간으로 필드값의 유효성 검사 | O | X | 
| 조건부로 제출 버튼 비활성화 (disabled) | O | X | 
| 실시간으로 입력 형식 적용하기 (숫자만 가능하게 등) | O | X | 
| 동적 입력 | O | X | 


### :two: React Hook Form
`React Hook Form`은 React 애플리케이션에서 폼을 효율적으로 관리하고 검증하기 위한 `비 제어 컴포넌트`기반 라이브러리다.

#### :pushpin: 2-1) React Hook Form 장점
비 제어 컴포넌트는 상태를 직접 관리하지 않기 때문에 리렌더링을 줄이고 성능을 향상 시킨다.
1. `빠른 성능`
  - 비제어 컴포넌트를 사용하여 리렌더링을 최소화함
2. `간편한 사용`
  - 최소한의 코드로 폼을 관리할 수 있음
3. `유연성`
  - 다양한 검증 라이브러리와 통합이 가능함

#### :pushpin: 2-2) 설치 방법
터미널에 아래의 명령어를 입력한다.

```bash
yarn add react-hook-form
```

#### :pushpin: 2-3) 기본 폼 구성 및 사용법

- `useForm` 
  - React Hook Form에서 제공하는 훅으로 폼과 관련된 여러 기능을 제공함.
- `register` 
  - 각 input 요소를 폼 데이터에 등록하고, 검증 규칙을 적용함.
- `handleSubmit` 
  - 폼을 제출할 때 호출되는 함수로, 폼의 유효성 검사를 통과했을 경우만 onSubmit 함수가 호출됨.
- `formState.errors` 
  - 폼 검증 중 발생한 에러 메시지를 포함하며, 해당 에러를 이용해 각 필드의 에러 상태를 렌더링함.
- `onSubmit` 
  - 폼이 검증을 통과하면 실행되는 함수로, 여기서는 제출된 데이터를 콘솔에 출력하도록 설정함.

```tsx
"use client";

import { useForm } from "react-hook-form";

const SignInForm = () => {
  const {
    register, // input 요소와 연결해 폼 데이터를 수집하는 메서드
    handleSubmit, // 폼 제출 시 호출되는 함수
    formState: { errors }, // 폼 검증 에러를 가져오는 객체
  } = useForm();

  // onSubmit: 폼이 유효하게 제출되면 호출됨
  const onSubmit = (data) => {
    console.log(data); 
  };

  return (
    // handleSubmit 으로 감싼 onSubmit은 폼 검증 후 유효한 경우 onSubmit 함수를 실행함
    <form
      onSubmit={handleSubmit(onSubmit)}
      className="flex flex-col gap-4 p-5 items-center w-full m-auto"
    >
      <div className="flex flex-col gap-2">
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="text"
          {...register("email", {
            // 필수 항목 검증
            required: "Email is required",
            // 이 패턴에 맞추지 않으면 메시지 출력할 수 있음
            pattern: {
              value: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/,
              message: "Invalid email address",
            },
          })}
          placeholder="Email"
          className="text-black"
        />
        {/* 이메일 검증 에러 메시지 */}
        {errors.email && (
          <p className="text-red-500">{errors.email.message}</p>
        )}
      </div>

      <div className="flex flex-col gap-2">
        <label htmlFor="password">Password</label>
        <input
          type="password"
          {...register("password", {
            required: "Password is required",
          })}
          placeholder="Password"
          className="text-black"
        />
        {errors.password && (
          <p className="text-red-500">{errors.password.message}</p>
        )}
      </div>
      <button
        className="bg-gray-800 text-white px-4 py-2 rounded-md"
        type="submit"
      >
        Sign In
      </button>
    </form>
  );
};

export default SignInForm;
```

### :three: Zod
Zod는 TypeScript와 JavaScript를 위한 스키마 선언 및 검증 라이브러리다.

#### :pushpin: 3-1) Zod 장점

- `직관적인 API`
  - 선언적인 방식으로 스키마를 정의할 수 있습니다.
- `타입 안전성`
  - TypeScript와의 강력한 통합을 제공합니다.
- `유연성` 
  - 다양한 검증 규칙과 커스텀 메시지를 지원합니다.

#### :pushpin: 3-2) 설치 방법
터미널에 아래의 명령어를 입력한다.

```bash
yarn add zod
yarn add @hookform/resolvers
```

#### :pushpin: 3-3) 사용법
- Zod와 React Hook Form을 함께 사용하면 더욱 강력한 폼 관리 및 데이터 검증이 가능하다. 
- 예를 들어, 사용자가 입력한 폼 데이터를 Zod로 유효성 검사하고, 그 결과에 따라 폼을 제출하는 흐름을 만들 수 있음

```tsx
import { z } from 'zod';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';

// z.object를 통해 스키마를 생성함
const signInSchema = z.object({
  // 이메일과 패스워드의 타입 정보 객체를 만들어줌
  email: z.string().email({ message: '유효한 이메일을 입력해주세요.' }),
  password: z.string().min(6, "비밀번호는 최소 6자 이상이어야 합니다."),
  username: z.string().min(3, "사용자 이름은 최소 3자 이상이어야 합니다."),
  age: z.number().min(18, "나이는 18세 이상이어야 합니다."),
});

export default function TestForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(signInSchema), // Zod 스키마로 유효성 검사
  });

  const onSubmit = (data) => {
    console.log(data); // 제출된 데이터가 콘솔에 출력됨
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* 이름 입력 필드 */}
      <div>
        <label>이름</label>
        <input {...register("username")} />
        {errors.username && <p>{errors.username.message}</p>}
      </div>

      {/* 나이 입력 필드 */}
      <div>
        <label>나이</label>
        <input type="number" {...register("age")} />
        {errors.age && <p>{errors.age.message}</p>}
      </div>

      {/* 이메일 입력 필드 */}
      <div>
        <label>이메일</label>
        <input type="email" {...register("email")} />
        {errors.email && <p>{errors.email.message}</p>}
      </div>

      {/* 비밀번호 입력 필드 */}
      <div>
        <label>비밀번호</label>
        <input type="password" {...register("password")} />
        {errors.password && <p>{errors.password.message}</p>}
      </div>

      <button type="submit">제출하기</button>
    </form>
  );
}
```


### :fire: 마무리
Zod와 React Hook Form 에 대해 알아보고 같이 사용했을 때 폼 데이터를 안전하게 관리하고, 유효성 검사까지 간편하게 처리할 수 있음을 알게 되었다. Zod의 타입스크립트 지원 덕분에 타입 안전성도 확보할 수 있었으며, 복잡한 폼 관리도 쉽게 해결할 수 있을 것 가탇.