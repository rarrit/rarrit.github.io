---
title: "TanStack Query란 무엇인가? (설치 및 사용 방법)"
date: 2024-09-07
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - library
  - api
tags:
  - tanstack query
author_profile: true
sidebar_main: true
---

## :ledger: TanStack Query에 대해 알아보자!

### :one: TanStack Query 란 무엇인가?

`TanStack Query`는 서버에서 데이터를 가져오고 캐시를 관리하며, 자동으로 데이터를 갱신해주는 React 라이브러리다. 주로 API 데이터를 효율적으로 관리할 때 사용되며, 복잡한 비동기 코드를 단순화 시켜줌

### :two: TanStack Query 왜 씀?

리액트를 사용하며 비동기 처리를 할 때 `useEffect` 를 통해 `fetch`나 `axios`를 사용해서 `useState`로 생성한 state에 저장하고 활용했었다. 개개인마다 이것이 편할 수 있는데, 컴포넌트 내에서 <u>로딩,에러,데이터 등 여러 상태를 관리를 직접 해야했다.</u> 중복된 코드도 많아질 수 있었고 각 상태에 따른 로직이 컴포넌트 내부에 분산되어 있어서 코드가 복잡해지는 어려움이 생길 수 있으며 여러 불편함이 생겨 TanStack Query와 같은 도구가 등장하게 되었다.

#### :pushpin: 2-2) Tanstack Query의 장점

- `데이터 캐싱`

  - TanStack Query는 데이터를 요청할 때 자동으로 캐시하여, 동일한 데이터를 다시 요청할 필요가 없게 만들어줌.
  - 동일한 데이터를 사용하는 여러 컴포넌트에서 불필요한 네트워크 요청을 줄일 수 있음.

- `자동 갱신`

  - 데이터를 일정 시간마다 자동으로 갱신할 수 있어, 실시간 데이터를 쉽게 유지 관리할 수 있음.
  - 이를 통해 최신 상태의 데이터를 사용자가 항상 볼 수 있게 해줌.

- `비동기 로직 간소화`

  - React에서 데이터를 요청하고 관리하는 일반적인 방법은 useEffect와 useState를 활용하여 데이터를 가져오고, 상태를 관리하는 복잡한 코드를 작성하는 것이었음. 하지만 TanStack Query를 사용하면 이러한 로직을 훨씬 간단하게 처리할 수 있음.

- `에러 및 로딩 상태 관리`

  - 비동기 요청에 대한 로딩 및 에러 상태를 자동으로 관리해주기 때문에, 이를 직접 처리할 필요가 없어짐.

- `서버 상태 관리`

  - 서버에서 데이터를 가져오는 비동기 작업은 클라이언트 상태와 달리 매우 복잡함. 서버 상태는 캐시되거나 동기화되어야 하고, 로딩 및 오류 상태를 처리해야 하며, 다른 작업과 조화를 이루어야 하는데 TanStack Query는 이러한 서버 상태를 쉽게 관리할 수 있는 도구를 제공.

- `요약`
  - 복잡한 비동기 로직을 단순화 해줌
  - 캐싱, 동기화, 리패칭 등의 기능을 쉽게 구현할 수 있음
  - 비동기 요청의 에러 및 로딩 등 자동으로 관리해줌
  - 서버 상태 관리를 더 쉽고 효율적으로 관리하게 됌

### :three: TanStack Query 설치 방법

터미널에 아래의 명령어를 입력하여 설치해요!

```bash
# npm
npm install @tanstack/react-query

# yarn
yarn add @tanstack/react-query
```

### :four: TanStack Query 기본 설정

main.jsx나 App.jsx에서 `QueryClient`와 `QueryClientPorivder`를 사용해 React 애플리케이션에 TanStack Query를 설정함.

```jsx
// main.jsx
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import "./index.css";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

createRoot(document.getElementById("root")).render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
);
```

### :five: useQuery (CRUD - R)

`useQuery`는 서버에서 데이터를 가져오고 캐싱하는 역할을 한다. 이를 통해 데이터를 여러 컴포넌트에서 쉽게 재사용할 수 있으며, 데이터를 요청할 때 로딩 상태나 오류 상태를 자동으로 관리함.

- `queryKey` : 캐싱할 데이터의 고유 키로, 동일한 키로 데이터를 요청할 때 캐시된 데이터를 반환함.
- `queryFn` : 서버에서 데이터를 가져오는 비동기 함수임

```jsx
// axios를 통해 todos의 데이터를 전달 받음
const fetchTodos = async () => {
  const response = await axios.get("url/todos");
  return response.data;
};

// useQuery를 사용
const { data, isLoading, error } = useQuery({
  queryKey: ["todos"], // 데이터를 캐싱할 키
  queryFn: fetchTodos, // 서버에서 데이터를 가져오는 함수
});

// 로딩중일 경우
if (isLoading) return <div>Loading...</div>;
// 에러일 경우
if (error) return <div>Error occurred</div>;

return (
  <ul>
    {data.map((todo) => (
      <li key={todo.id}>{todo.title}</li>
    ))}
  </ul>
);
```

### :six: useMutation (CRUD - CUD)

`useMutation`은 서버에서 데이터를 생성,수정,삭제하는 작업을 처리함. 데이터를 성공적으로 업데이트한 후 `invalidateQueries`를 사용해서 <u>캐시된 데이터를 무효화</u>하고 화면에 최신 데이터를 표시해줌

```jsx
// todo 추가 함수
const addTodo = async (newTodo) => {
  await axios.post("url/todos", newTodo);
};

// useMutation: Todo를 추가하는 요청
const addMutation = useMutation({
  mutationFn: addTodo,
});

return (
  <>
    <form
      onSubmit={(e) => {
        e.preventDefault();

        addMutation.mutate({
          title: todoItem,
          isDone: false,
        });
      }}
    >
      <input
        type="text"
        value={todoItem}
        onChange={(e) => setTodoItem(e.target.value)}
      />
      <button>추가</button>
    </form>
  </>
);
```

### :seven: invalidateQueries

`invalidateQueries`는 특정 `queryKey`를 기반으로 캐시된 데이터를 무효화하여, 새로운 데이터가 서버로부터 다시 가져오도록 한다. 이 기능은 데이터를 수정, 삭제, 추가한 후 기존 데이터를 최신 상태로 유지할 때 매우 유용함.

```jsx
// QueryClient를 사용해 캐시된 데이터를 업데이트하거나 무효화할 수 있음
const queryClient = useQueryClient();

// todo 추가 함수
const addTodo = async (newTodo) => {
  await axios.post("url/todos", newTodo);
};

// useMutation: Todo를 추가하는 요청
const addMutation = useMutation({
  mutationFn: addTodo,
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]); // 후후 최신 상태로 유지해주마
  },
});

return (
  <>
    <form
      onSubmit={(e) => {
        e.preventDefault();

        addMutation.mutate({
          title: todoItem,
          isDone: false,
        });
      }}
    >
      <input
        type="text"
        value={todoItem}
        onChange={(e) => setTodoItem(e.target.value)}
      />
      <button>추가</button>
    </form>
  </>
);
```

### :fire: 마무리

`TanStack Query`는 React에서 서버 상태 관리를 간소화하고, 데이터를 효율적으로 캐시하며, 비동기 데이터 처리 로직을 크게 개선해주는 강력한 도구이다. 이를 통해 복잡한 비동기 API 요청 및 상태 관리를 보다 간편하게 만들 수 있으며 특히 여러 컴포넌트에서 동일한 데이터를 재사용하거나, 서버에서 주기적으로 데이터를 갱신해야 할 때 매우 유용하다. 문법에 익숙해지기 위해 TodoList CRUD 만들어 보았는데, 관련 링크를 통해 확인 가능!

#### :pushpin: 관련 링크
