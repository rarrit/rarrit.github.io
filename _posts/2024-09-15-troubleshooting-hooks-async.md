---
title: "[Error: Invalid hook call.] 훅은 항상 동기적으로 호출되어야 한다."
date: 2024-09-15
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:  
  - troubleshooting
  - til
tags:
  - troubleshooting
author_profile: true
sidebar_main: true
---

## :ledger: Tanstack query custom hook을 사용하며 겪은 에러다.

### :one: 개요

![Invalid hook call.](https://github.com/user-attachments/assets/e86e3ada-880e-4af8-8083-095c35821f84)

로그인 기능을 완료 후 Tanstack Query의 custom 훅을 생성하며 겪은 에러이다. 로직을 작성 후 실행했을 때 위와 같은 에러를 접했고 내용은 "Hooks는 함수 컴포넌트의 몸체 내부에서만 호출되며, Hooks 규칙 위반"이라고 설명한다. 내가 작성한 커스텀 훅은 아래와 같다.

```jsx
import { handleUserLogin, handleUserRegister } from "@/core/api/authAPI";
import { QUERY_KEYS } from "@/core/constants/queryKeys";
import { useMutation, useQueryClient } from "@tanstack/react-query"

export const useUserRegister = async () => {
  const queryClient = await useQueryClient();
  return useMutation({
    mutationFn: handleUserRegister,
    mutationKey: [QUERY_KEYS.REGISTER],
  })
}

export const useUserLogin = async () => {
  const queryClient = await useQueryClient();
  return useMutation({
    mutationFn: handleUserLogin,
    mutationKey: [QUERY_KEYS.LOGIN]
  })
}

```

### :two: 분석 및 처리
에러를 번역했을 때 훅의 잘못된 사용이라고 했는데, 처음에 어떠헥 잘못사용된지는 잘 모르겠엇다. 그런데 Error 앞에 (in promise)라고 적혀잇어, 뭔가 비동기 함수로 생성해서 그런가? 하고 async/await 을 지워줬다. 이후 적용하니 정상적으로 로그인 기능이 적용되었다.

```jsx
export const useUserLogin = () => {
  const queryClient = useQueryClient();
  return useMutation({
    mutationFn: handleUserLogin,
    mutationKey: [QUERY_KEYS.LOGIN]
  })
}
```



### :three: 해결방법
해결방법이라기 보다 Hook 자체가 비동기적으로 실행될 필요는 없으므로 async,await을 제거하면 정상적으로 작동한다. 이렇게 판단한것은 [리액트 공식 문서](https://ko.legacy.reactjs.org/docs/hooks-rules.html#explanation)에서 <u>React가 Hook이 호출되는 순서에 의존한다는 것입니다.</u>라고 말해주고 있는데, 즉 훅 자체를 동기적으로 호출되어야 한다고 판단했음

- React Hook은 비동기 함수로 만들 수 없으므로 async 키워드를 제거해야 한다.
- `useQueryClient`는 동기적으로 호출해야 하며, `await`없이 사용해야 한다.

### :fire: 마무리
요즘 기능을 하나 만들면서 만나는 에러가 잦아진 것 같다. 그때마다 찾아보며 에러를 해결하는데 에러형 제발 그만나와줬음..좋..겠..ㅇ..ㅓ