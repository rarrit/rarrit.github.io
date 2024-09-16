---
title: "supabase의 SQL문법과 DML문법 정리"
date: 2024-09-03
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

## :ledger: supabase의 SQL문법과 데이터 조작 언어를 정리해보자!

이번에 프로젝트를 진행하며 사용한 `Supabase`! 나중에 다시 사용하게 되었을 때 참고하기 위해 정리해봤다.

### :one: SQL 문법

`FROM`, `SELECT`, `WHERE`, `ORDER`, `LIMIT`

- `.from(Post)`: 'Post' 테이블에서
- `.select(*)`: 모든 컬럼을 선택함
- `.eq('userId', 1)`: 'userId'가 1인 행만 필터링함
- `.order('created_at', {ascending: false})`: 'create_at' 기준으로 내림차순으로 정렬함
- `.limit(10)`: 상위 10개의 결과만 반환함

```jsx
// Post 테이블 전체에서 userId가 123인 게시물을 create_at기준으로 내림차순으로 정렬 후 상위 10개의 결과를 선택함
const { data, error } = await supabase
  .from("Post")
  .select("*")
  .eq("userId", 123)
  .order("created_at", { ascending: false })
  .limit(10);
```

### :two: DML 문법

`DML`은 데이터 조작 언어라고 함.

#### :pushpin: 2-1) INSERT

- `INSER` : 데이터를 삽입해줌

```jsx
// Post 테이블에 title, content, userId 삽입
const { data, error } = await supabase
  .from("Post")
  .insert([{ title: "Hi", content: "my name is rarrit", userId: 1 }]);
```

#### :pushpin: 2-2) UPDATE

- `UPDATE` : 업데이트 해줌

```jsx
// Post 테이블에서 userId가 1인 게시물에 title을 업데이해줌
const { data, error } = await supabase
  .from("Post")
  .eq("userId", 1)
  .update({ title: "my name is kimminkyu" });
```

#### :pushpin: 2-3) DELETE

- `DELETE` : 삭제함

```jsx
// Post 테이블에서 userId가 1인 게시물을 삭제함
const { data, error } = await supabase.from("Post").eq("userId", 1).delete();
```

### :fire: 마무리

현재까지 사용해본 것들 위주로 정리를 해봤다. SQL문법과 DML문법을 조합해서 CRUD구현까지는 가능했으며, 데이터를 어떻게 불러오느냐에 따라 최적화가 되는지 고통을 받는지 경혐해봐서(항상 데이터가 1억개가 있다는 가정하에 생각하기로 결심).. 앞으로 추가로 사용한 문법이 되면 업데이트 할 예정!!
