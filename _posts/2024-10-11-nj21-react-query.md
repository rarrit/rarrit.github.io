---
title: "[Next.js] React Query 사용법 및 개념에 대해 알아보자"
date: 2024-10-11
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
tags:
  - tanstack query
  - react query
author_profile: true
sidebar_main: true
---

## :ledger: Next.js React Query 사용법 및 개념에 대해 알아보자
Next.js와 사용되는 React Query, 사용법과 개념에 대해 알아보고 React에서 사용되는 것과 어떤 부분이 다른지 알아보자!

### :one: React Query
`React Query`는 fetching, caching, 서버 데이터와의 동기화를 지원해주는 라이브러리로, React 컴포넌트 내부에서 간단하고 직관적으로 API를 사용할 수 있다.

### :two: React Query 사용이유
기존 Redux나 Recoil 등의 상태 관리 라이브러리는 <u>클라이언트와 서버 상태를 함께 관리한다.</u> 하지만 이는 비효율적일 수 있으며, `React Query`는 서버 상태를 분리해서 관리할 수 있어 직관적이고 효율적인 상태 관리를 할 수 있다. 다양한 옵션들을 활용해 **캐싱, 에러 처리, suspense, refresh, 데이터 패칭 조건 설정** 등을 선언적이고 간편하게 이용할 수 있음

#### :pushpin: 2-1) Data Fetching 단순화 (기존 처리 방식)
기존의 방식에서는 아래와 같은 단계를 거쳐야 했음

1. Fetching 코드 작성
2. 데이터를 담아 둘 상태 생성
3. useEffect를 이용해 컴포넌트 마운트 시 데이터를 Fetching한 뒤 상태에 저장

```tsx
import { useEffect, useState } from "react";

const getServerData = async () => {
  const data = await fetch("<https://jsonplaceholder.typicode.com/posts>").then((response) => response.json());
  return data;
};

export default function App() {
  const [state, setState] = useState([]);

  useEffect(() => {
    getServerData()
      .then((dataList) => setState(dataList))
      .catch((e) => setState([]));
  }, []);

  return <div>{JSON.stringify(state)}</div>;
}
```

#### :pushpin: 2-2) Data Fetching 단순화 (Reacth Query 사용)
React Query를 사용하면 useQuery 한 줄로 이 과정을 처리할 수 있음

1. 코드 수 감소로 인한 사이드 이펙트 제거
2. Data Fetching 방식 규격화
3. `enabled` 옵션을 이용한 동기적 실행

```tsx
import { useQuery } from '@tanstack/react-query';

const getServerData = async () => {
  const data = await fetch("<https://jsonplaceholder.typicode.com/posts>").then((response) => response.json());
  return data;
};

export default function App() {
  const { data } = useQuery({
    queryKey: ['data'],
    queryFn: getServerData
  });

  return <div>{JSON.stringify(data)}</div>;
}
```

#### :pushpin: 2-3) 동기적 실행 (이전 방식)
useEffect를 이용해서 의존성 배열에 상태값을 넣어 사용한 방식

```tsx
const [state1, setState1] = useState();
const [state2, setState2] = useState();

useEffect(() => {
  getServerData().then((dataList) => {
    setState1(dataList[0]);
  });
}, []);

useEffect(() => {
  if (state1) {
    getAfterData(state1).then((dataList) => {
      setState2(dataList);
    });
  }
}, [state1]);
```


#### :pushpin: 2-4) 동기적 실행 (React Query 방식)
React Query의 `enabled` 옵션을 사용하면 아래와 같이 간단하게 동기적 실행이 가능하다.

```tsx
const [state1, setState1] = useState();
const [state2, setState2] = useState();

useEffect(() => {
  getServerData().then((dataList) => {
    setState1(dataList[0]);
  });
}, []);

useEffect(() => {
  if (state1) {
    getAfterData(state1).then((dataList) => {
      setState2(dataList);
    });
  }
}, [state1]);
```

### :three: React Query 특징

#### :pushpin: 3-1) queryKey 기반의 캐싱
React Query는 queryKey를 기반으로 데이터를 캐싱한다. `queryKey`는 각 쿼리를 고유하게 식별하는 키로 사용되며, 이를 통해 동일한 키로 반복적인 데이터 요청 시 캐싱된 데이터를 반환하여 불필요한 네트워크 요청을 줄여준다.

- 캐싱이란 특정 데이터의 복사본을 저장하여 이후 동일한 데이터의 재접근 속도를 높이는 것이다. 
- React Query의 캐싱 기능을 이용해 불필요한 API 호출을 막고 캐싱 된 데이터를 이용할 수 있다.

```tsx
const { data } = useQuery({
  queryKey: ['dataKey'],
  queryFn: getServerData
});

// 이후 동일한 queryKey를 사용하면 캐싱된 데이터를 사용합니다.
const { data: cachedData } = useQuery({
  queryKey: ['dataKey'],
  queryFn: getServerData
});
```

#### :pushpin: 3-2) Stale Time, Cache Time
아래와 같은 옵션을 통해 별다른 refresh가 없을 때, 10분 내 재호출 시 API를 호출하지 않고 캐싱 된 데이터를 제공해 준다.

- `staleTime`: 데이터를 새로 고침할 필요가 없는 유효한 상태로 간주하는 시간
- `gcTime`: 캐시된 데이터가 사용되지 않는 경우 언제 메모리에서 삭제될 지 결정하는 시간

```tsx
const { data } = useQuery({
  queryKey: ['data'],
  queryFn: getServerData,
  staleTime: 10 * 60 * 1000, // 10분
  gcTime: 10 * 60 * 1000  // 10분 // cache time
});
```

#### :pushpin: 3-2) 클라이언트 데이터와 서버 데이터 간의 분리 (onSuccess, onError)
기존의 try, catch 를 사용한 것 처럼 성공과 실패의 분기를 간단하게 구현이 가능하다.

```tsx
const { mutate, isLoading } = useMutation({
  mutationFn: () => {
    return fetch(URL).then(response => response.json());
  },
  onSuccess: (data) => {
   
  },
  onError: (error) => {

  }
});
```

### :four: React Query 대표 기능

#### pushpin: 4-1) useQuery
- GET 요청에 주로 사용된다. 
- 첫 번째 파라미터로 unique key를 포함한 배열이 들어가고, 두 번째 파라미터로 실제 호출하고자 하는 비동기 함수가 들어간다.

```tsx
import { QueryClient, QueryClientProvider, useQuery } from '@tanstack/react-query';

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isLoading, error, data } = useQuery({
    queryKey: ['repoData'],
    queryFn: () =>
      fetch('<https://api.github.com/repos/tannerlinsley/react-query>').then((res) => res.json()),
  });

  if (isLoading) return 'Loading...';

  if (error) return 'An error has occurred: ' + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{' '}
      <strong>✨ {data.stargazers_count}</strong>{' '}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}
```

#### pushpin: 4-2) useMutation
`useMutation`은 주로 **PUT, UPDATE, DELETE** 요청에 주로 사용된다

```tsx
function App() {
  const mutation = useMutation({
    mutationFn: (newTodo) => {
      return fetch('/todos', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newTodo)
      }).then(response => response.json());
    },
  });

  return (
    <div>
      {mutation.isLoading ? (
        'Adding todo...'
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}
          {mutation.isSuccess ? <div>Todo added!</div> : null}
          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: 'Do Laundry' });
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  );
}
```

### :five: React Query 사용법

#### :pushpin: 5-1) React Query 설치방법

```bash
yarn add @tanstack/react-query @tanstack/react-query-devtools
```

#### :pushpin: 5-2) Provider 설정
- app 디렉토리 안에 provider.tsx를 생성해준다.

```tsx
// In Next.js, this file would be called: app/providers.tsx
"use client";

// Since QueryClientProvider relies on useContext under the hood, we have to put 'use client' on top
import {
  isServer,
  QueryClient,
  QueryClientProvider,
} from "@tanstack/react-query";

function makeQueryClient() {
  return new QueryClient({
    defaultOptions: {
      queries: {
        // With SSR, we usually want to set some default staleTime
        // above 0 to avoid refetching immediately on the client
        staleTime: 60 * 1000,
      },
    },
  });
}

let browserQueryClient: QueryClient | undefined = undefined;

function getQueryClient() {
  if (isServer) {
    // Server: always make a new query client
    return makeQueryClient();
  } else {
    // Browser: make a new query client if we don't already have one
    // This is very important, so we don't re-make a new client if React
    // suspends during the initial render. This may not be needed if we
    // have a suspense boundary BELOW the creation of the query client
    if (!browserQueryClient) browserQueryClient = makeQueryClient();
    return browserQueryClient;
  }
}

export default function Providers({ children }: { children: React.ReactNode }) {
  // NOTE: Avoid useState when initializing the query client if you don't
  //       have a suspense boundary between this and the code that may
  //       suspend because React will throw away the client on the initial
  //       render if it suspends and there is no boundary
  const queryClient = getQueryClient();

  return (
    <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
  );
}
```

- export 했던 `Providers`을 root layout에서 children 감싸준다.

```tsx
// In Next.js, this file would be called: app/layout.jsx
import Providers from './providers'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head />
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  )
```

### :six: react의 react query와 차이점

- React는 주로 클라이언트에서 데이터를 가져오는 CSR 방식으로 React Query를 사용하며, Next.js는 서버에서 데이터를 미리 가져와 사용하는 SSR/SSG 방식이 추가된다는 점에서 차이가 있음.
- Next.js에서는 React Query와 함께 서버 사이드 데이터 페칭과 클라이언트 캐싱 동기화를 유기적으로 활용할 수 있으며, SEO와 초기 렌더링 성능에서도 이점이 있음.

### :fire: 마무리
- React Query는 서버 데이터를 손쉽게 관리하고 최적화할 수 있는 강력한 도구이다. 
- 특히 Next.js에서는 CSR뿐만 아니라 SSR, SSG와 같은 다양한 방식으로 데이터를 처리할 수 있어, 성능 향상과 SEO 측면에서도 많은 이점이 있다.