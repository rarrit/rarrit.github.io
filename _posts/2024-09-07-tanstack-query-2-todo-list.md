---
title: "TanStack Query를 사용한 TodoList CRUD 구현"
date: 2024-09-07
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - mini
tags:
  - tanstack query
  - json-server
author_profile: true
sidebar_main: true
---

## :ledger: TanStack Query + json-server를 사용해서 TodoList를 만들어보자.

![json-server-todolist1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/e5c3e608-962c-46fc-a90e-76319f40c2a5)

- 작업 코드: [https://github.com/rarrit/tanstack-query-todo](https://github.com/rarrit/tanstack-query-todo)

### :one: 프로젝트 환경 설정

#### :pushpin: 1-1) Tanstack query 설치

```bash
yarn add @tanstack/react-query
```

#### :pushpin: 1-2) axios 설치

```bash
yarn add axios
```

#### :pushpin: 1-3) JSON Server 설치

```bash
yarn add json-server # 설치
yarn json-server db.json --port 4000 # 실행
```

#### :pushpin: 1-4) db.json 구조 정의

루트 폴더에 생성 후 Todo 데이터 작성

```jsx
{
  "todos": [
    {
      "id": "1",
      "title": "리액트 공부하기 1",
      "isDone": true
    },
    {
      "id": "7f41",
      "title": "Tanstack Query 공부하기",
      "isDone": false
    },
  ]
}
```

### :two: TodoList CRUD - Read

`useQuery`를 사용해서 json-server의 todo 리스트를 가져오고 UI를 그려줌

```jsx
const fetchTodos = async () => {
  const response = await axios.get("http://localhost:4000/todos");
  return response.data;
};

const {
  data: todos,
  isPending,
  isError,
} = useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodos,
});
```

### :three: TodoList CRUD - Create

`useMutation`을 사용하여 새로운 Todo를 추가해주고 `invalidateQueries`를 통해 변경된 리스트를 다시 불러옴

```jsx
const addTodo = async (newTodo) => {
  await axios.post("http://localhost:4000/todos", newTodo);
};

const addMutation = useMutation({
  mutationFn: addTodo,
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]);
  },
});
```

### :four: TodoList CRUD - Update

`useMutation`을 사용하여 기존 Todo를 수정해줌

```jsx
const updateTodo = async (updatedTodo) => {
  await axios.put(`http://localhost:4000/todos/${updatedTodo.id}`, updatedTodo);
};

const updateMutation = useMutation({
  mutationFn: updateTodo,
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]);
    setEditTodo(null); // 수정 상태 초기화
  },
});
```

### :five: TodoList CRUD - Delete

`useMutation`을 사용하여 Todo를 삭제하고 `invalidateQueries`를 통해 삭제된 리스트를 다시 불러옴

```jsx
const deleteTodo = async (id) => {
  await axios.delete(`http://localhost:4000/todos/${id}`);
};

const deleteMutation = useMutation({
  mutationFn: deleteTodo,
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]);
  },
});
```

### :six: TodoList CRUD - 전체 구현 코드

```jsx
import { useMutation, useQuery, useQueryClient } from "@tanstack/react-query";
import axios from "axios";
import "./App.css";
import { useState } from "react";

const App = () => {
  // QueryClient를 사용해 캐시된 데이터를 업데이트하거나 무효화할 수 있습니다.
  const queryClient = useQueryClient();

  // Todo를 추가하기 위해 필요한 상태
  const [todoItem, setTodoItem] = useState("");

  // 수정할 Todo 항목을 관리하는 상태
  const [editTodo, setEditTodo] = useState(null);

  // Todos 데이터를 가져오는 비동기 함수
  const fetchTodos = async () => {
    const response = await axios.get("http://localhost:4000/todos");
    return response.data;
  };

  // 새로운 Todo를 추가하는 비동기 함수
  const addTodo = async (newTodo) => {
    await axios.post("http://localhost:4000/todos", newTodo);
  };

  // 기존 Todo를 업데이트하는 비동기 함수
  const updateTodo = async (updatedTodo) => {
    await axios.put(
      `http://localhost:4000/todos/${updatedTodo.id}`,
      updatedTodo
    );
  };

  // Todo를 삭제하는 비동기 함수
  const deleteTodo = async (id) => {
    await axios.delete(`http://localhost:4000/todos/${id}`);
  };

  // useQuery: Todo 리스트를 서버에서 가져와 캐싱하고 컴포넌트에서 사용할 수 있게 함
  const {
    data: todos,
    isPending,
    isError,
  } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  // useMutation: Todo를 추가하는 요청
  const addMutation = useMutation({
    mutationFn: addTodo,
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]); // 데이터를 새로고침
    },
  });

  // useMutation: Todo를 업데이트하는 요청
  const updateMutation = useMutation({
    mutationFn: updateTodo,
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]);
      setEditTodo(null); // 수정 상태를 초기화
    },
  });

  // useMutation: Todo를 삭제하는 요청
  const deleteMutation = useMutation({
    mutationFn: deleteTodo,
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]);
    },
  });

  if (isPending) {
    return <div>로딩중입니다...</div>;
  }

  if (isError) {
    return <div>데이터 조회 중 오류가 발생했습니다...</div>;
  }

  return (
    <>
      <h1>Tanstack Query TodoList</h1>
      <form
        onSubmit={(e) => {
          e.preventDefault();

          if (editTodo) {
            // 수정모드에서는 기존 Todo를 업데이트함
            updateMutation.mutate({
              ...editTodo,
              title: todoItem,
            });
          } else {
            // 추가모드에서는 새로운 Todo를 추가함
            addMutation.mutate({
              title: todoItem,
              isDone: false,
            });
          }

          setTodoItem("");
        }}
      >
        <input
          type="text"
          value={todoItem}
          onChange={(e) => setTodoItem(e.target.value)}
        />
        <button>{editTodo ? "수정" : "추가"}</button>
      </form>

      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            style={{ display: "flex", alignItems: "center", gap: "10px" }}
          >
            <h4>{todo.title}</h4>
            <p>{todo.isDone ? "Done" : "Not Done"}</p>
            <button onClick={() => deleteMutation.mutate(todo.id)}>삭제</button>
            <button
              onClick={() => {
                setTodoItem(todo.title);
                setEditTodo(todo); // 수정모드로 전환
              }}
            >
              수정
            </button>
          </li>
        ))}
      </ul>
    </>
  );
};

export default App;
```

### :fire: 마무리

스탠다드반으로 옮긴 후 TanStack Query를 사용해서 Create 부분을 반복적으로 만들었다. 이후 복습하기 위해 만들다가 뭔가 아쉬워서 CRUD를 구현해보았는데, 생각보다 시간이 많이 걸렸다. 이전에 Fetch를 사용해서 만들 때도 시간이 정말 많이 걸렸는데.. 그래도 사용해보며 느낀건 이전에 `useEffect`와 `fetch`를 사용해서 데이터를 불러오고 state에 담아주고 했던 모든 과정보다 많이 축약되고 가벼운 느낌으로 무척 편리함을 느꼈다. 이번 개인 과제를 통해 더 잘 사용할 수 있기를 기대해본다.

#### :pushpin: 관련 링크

- [TanStack Query란 무엇인가? (설치 및 사용 방법)](https://rarrit.github.io/react/library/api/tanstack-query-1/)
