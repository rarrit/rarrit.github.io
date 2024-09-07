---
title: "json-sever + fetch를 사용한 TodoList CRUD"
date: 2024-09-06
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - api
  - library
  - development
tags:
  - json-server
  - fetch
author_profile: true
sidebar_main: true
---

## :ledger: json-server, fetch를 사용해서 TodoList를 만들어보자.

![json-server-todolist1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/e5c3e608-962c-46fc-a90e-76319f40c2a5)

`fetch`, `axios`를 통해 todoList를 만들어보며 json-server와 친해지는 중.

- 작업 코드: [https://github.com/rarrit/json-server-todo](https://github.com/rarrit/json-server-todo)

## :one: db.json 세팅

간단하게 id, title, content 값을 json 형식으로 넣어준다.

```jsx
{
  "todos": [
    {
      "id": 1,
      "title": "todo1",
      "content": "todo1 입니다",
    },
    {
      "id": 2,
      "title": "todo2",
      "content": "todo2 입니다",
    }
  ]
}
```

## :two: CRUD - [R] todoList를 화면에 출력!

리액트를 공부하며 반복해서 만들다 보니 데이터만 전달 받으면, 화면에 출력은 이제 너무나도 익숙하다. 아래의 주석을 순서로 적용

```jsx
// [1] 데이터를 담아줄 state 생성
const [todos, setTodos] = useState([]);

// [2] useEffect로 json-server 데이터 받기
useEffect(() => {
  // json-server 데이터 받기
  fetch("http://localhost:4000/todos")
    .then((response) => response.json())
    // data를 setTodos를 통해 상태를 업데이트해줌
    .then((data) => setTodos(data))
    .catch((e) => console.log("error =>", e));
}, []);

// [3] 여기서 데이터 값이 없다면 암튼 내가 잘못한거임
console.log(todos);

return (
  <>
    {
      // [4] map메서드를 사용해서 뿌려줌
      todos.map((todo) => {
        return (
          <div key={todo.id}>
            <strong>{todo.title}</strong>
            <p>{todo.content}</p>
          </div>
        );
      })
    }
  </>
);
```

## :two: CRUD - [C] Todo를 추가해보자!

json-server의 todos를 가져오고 새로운 데이터를 추가하기 위해 method를 사용해야한다. 개인적으로 headers의 JSON 데이터를 명시하는 것과 문자열로 변환해서 전송하는 방법을 몰라 삽질하다가 검색해서 해결했다..

- `method: 'POST'`: 새로운 데이터를 전송할꺼임~~! 서버님은 받으셈~!
- `'Content-Type' : 'application/json'`: JSON 데이터에요. 확인해주세요.
- `body: JSON.stringify(newTodo)`: 문자열로 전송함니다~

```jsx
// [3] addTodos 실행
const addTodos = (newTodo) => {
  fetch("http://localhost:4000/todos", {
    // POST를 사용하여, 새로운 데이터를 전송함
    method: "POST",
    // 헤더 설정: 아래의 body(본문)가 JSON 형식임을 알려야한다캄
    headers: {
      // JSON 데이터임을 명시
      "Content-Type": "application/json",
    },

    // 데이터를 JSON 문자열로 변환해 전송
    body: JSON.stringify(newTodo),
  })
    // 서버로부터 응답을 받으면 JSON으로 변환.
    .then((response) => response.json())
    // 변환된 JSON 데이터를 처리함
    .then((data) => {
      // [리랜더] 위에서 body로 전송해서 값이 추가되었으니 기존 배열을 유지한 채 새로운 데이터를 추가해줌
      setTodos([...todos, data]);
    })
    .catch((e) => console.log("error =>", e));
};

/*
 * [2] 해당 함수에서 업데이트된 state값을 addTodos의 파라미터에 넣어쥼
 */
const saveTodos = () => {
  const newTodo = {
    // state에 저장된 값 => 입력한 값
    title,
    content,
  };
  // addTodo 에게 전달해쥼
  addTodos(newTodo);

  // 상태 초기화 => 입력 값 초기화
  setTitle("");
  setContent("");
};
return (
  <>
    <h1>json-server todoList</h1>
    title:
    <input value={title} onChange={(e) => setTitle(e.target.value)} />
    <br />
    content:
    <input value={content} onChange={(e) => setContent(e.target.value)} />
    <br />
    {/* [1] 버튼 클릭 시 saveTodos 실행 */}
    <button type="button" onClick={saveTodos}>
      저장
    </button>
    {todos.map((todo) => {
      return (
        <div key={todo.id}>
          <strong>{todo.title}</strong>
          <p>{todo.content}</p>
        </div>
      );
    })}
  </>
);
```

## :two: CRUD - [D] Todo를 삭제해보자!

json-server의 todos를 가져오고 데이터를 삭제해주기 위해 method를 사용해야한다.

- `method: 'DELETE'`: 데이터 삭제할꺼임~~! 서버님은 받으셈~!

```jsx
// [2] deleteTodo 실행
const deleteTodo = (id) => {
  // url에 삭제하려는 todo의 id를 포함시켜서 DELETE 요청을 보냄
  fetch(`http://localhost:4000/todos/${id}`, {
    method: "DELETE",
  })
    .then(() => {
      // set함수를 사용해서 선택한 todo를 제외함
      setTodos(todos.filter((todo) => todo.id !== id));
    })
    .catch((e) => console.log("error =>", e));
};

return (
  <>
    <h1>json-server todoList</h1>
    title: <input value={title} onChange={(e) => setTitle(e.target.value)} />
    <br />
    content: <input
      value={content}
      onChange={(e) => setContent(e.target.value)}
    />
    <br />
    <button type="button" onClick={saveTodos}>
      저장
    </button>
    {todos.map((todo) => {
      return (
        <div key={todo.id}>
          <strong>{todo.title}</strong>
          <p>{todo.content}</p>
          {/* [1] 삭제 버튼을 클릭하면 deleteTodo 실행, 매개변수로 해당 todo의 id값을 넣어쥼 */}
          <button type="button" onClick={() => deleteTodo(todo.id)}>
            삭제
          </button>
        </div>
      );
    })}
  </>
);
```

## :two: CRUD - [U] Todo를 수정해보자!

기존의 todo에 수정 버튼을 클릭하면 상단의 Input을 통해 값을 수정할 수 있도록 적용했다.

- `method: 'PATCH'`: 데이터 일부 변경 할꺼임~~! 서버님은 받으셈~!
  - `method: 'PUT'`은 덮어쓰는거로 이해하면 됌 PATCH는 일부 변경

```jsx
// [2] 매개변수로 전달받은 todo를 set함수를 통해 상태를 전부 업데이트해준다.
const editTodo = (todo) => {
  setEditId(todo.id);
  setEditTitle(todo.title);
  setEditContent(todo.content);
};

// [5] updateTodo 실행, 주석 참고
const updateTodo = () => {
  // title, content 값을 변경한 값으로 할당함
  const updatedTodo = {
    title: editTitle,
    content: editContent,
  };

  // url에 변경하려는 todo의 id를 포함시켜서 PATCH 요청을 보냄
  fetch(`http://localhost:4000/todos/${editId}`, {
    method: "PATCH",
    // 헤더 설정: 아래의 body(본문)가 JSON 형식임을 알려줌~~
    headers: {
      // JSON 데이터임을 명시
      "Content-Type": "application/json",
    },
    // 데이터를 JSON 문자열로 변환해 전송
    body: JSON.stringify(updatedTodo),
  })
    .then((response) => response.json())
    .then((data) => {
      setTodos(todos.map((todo) => (todo.id === editId ? data : todo)));
      setEditId(null);
      setEditTitle("");
      setEditContent("");
    })
    .catch((e) => console.log("업데이트 에러", e));
};

return (
  <>
    <h1>json-server todoList</h1>
    {/* [3] editId 값이 업데이트 될 경우 기존 추가 폼에서 수정 폼으로 일괄 변경됨 */}
    title:
    <input
      // editId 값이 업데이트 될 경우 editTitle 아닐 경우 title
      value={editId ? editTitle : title}
      onChange={(e) =>
        // editId 값이 업데이트 될 경우 setEditTitle 아닐 경우 setTitle
        {
          editId ? setEditTitle(e.target.value) : setTitle(e.target.value);
        }
      }
    />
    <br />
    content:
    <input
      value={editId ? editContent : content}
      onChange={(e) => {
        editId ? setEditContent(e.target.value) : setContent(e.target.value);
      }}
    />
    <br />
    {/* [4] 값을 입력 후 수정 버튼을 클릭 시 updateTodo 함수 실행 */}
    <button type="button" onClick={editId ? updateTodo : saveTodos}>
      {editId ? "수정" : "저장"}
    </button>
    {todos.map((todo) => {
      return (
        <div key={todo.id}>
          <strong>{todo.title}</strong>
          <p>{todo.content}</p>
          {/* [1] 수정 버튼을 클릭하면 editTodo 매개변수에 todo를 넣고 실행 */}
          <button type="button" onClick={() => editTodo(todo)}>
            수정
          </button>
          <button type="button" onClick={() => deleteTodo(todo.id)}>
            삭제
          </button>
        </div>
      );
    })}
  </>
);
```

### :fire: 마무리

이번 수업에서 axios를 사용햇다. 이전에는 fetch를 사용해서 데이터 처리를 했는데, 어떤 차이점이 있는지 알아볼 겸 fetch를 사용해서 todoList CRUD를 구현해봤다. 다음 글에서는 axios를 사용해서 todoList CRUD를 구현해보며 직접 느껴봐야겠다.

#### :pushpin: 알게된 부분

이전 강의를 통해 배웠던 `HTTP 메서드`를 직접 사용해보며 어떻게 사용하는지 알 수 있었다. 특히 Create 를 진행하며, 추가되었을 때 리랜더가 되지 않아서 방법을 찾던 중 데이터를 전송 후 다시 Set함수를 사용하여 업데이트 해줘야된다는 걸 알게되었다.

#### :pushpin: 어려웠던 부분

처음 데이터를 전송하는 방법에 대해 잘 몰라가지고 고민을 좀 많이하다가 결국 [검색 - seoltang.log](https://velog.io/@seoltang/fetch-POST-Request)해서 처리했다. 어려웠던 부분이라 치면 문법 자체가 익숙하지 않아서 Update할 때 최대한 기억해서 작성해보려 햇으나 까먹어서 복붙해버림

#### :pushpin: 개선할 부분

fetch 뿐만 아니라 axios에서도 `HTTP 메서드`와 headers, body를 사용할텐데 문법과 개념에 익숙해지도록 해야함

#### :pushpin: 관련 링크

- [json-server란 무엇인가?](https://rarrit.github.io/api/library/development/json-server-1/)
- [json-sever 친해지기 (2) - axios를 사용한 TodoList CRUD](https://rarrit.github.io/api/library/development/json-server-3-todo-axios/)
