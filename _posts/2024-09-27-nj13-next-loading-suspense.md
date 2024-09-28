---
title: "[Next.js] Suspense, Loading UI, Streaming SSR"
date: 2024-09-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next 
  - development
tags:
  - next js
  - suspense
  - loading ui
  - streaming ssr
author_profile: true
sidebar_main: true
---

## :ledger: Suspense, Loading UI, Streaming SSR에 대해 알아보자!
`Next.js`에서 기본 제공해주는 지연 렌더링, 로딩 UI, 서버사이드 스트리밍에 대해서 알아보자.

### :one: 서버의 요청이 느릴 경우 시뮬레이션
서버의 요청이 느릴 경우를 알아보기 위해 인위적으로 딜레이를 추가해서 느린 네트워크 상황을 시뮬레이션 해보자.

#### :pushpin: 1-1) json-server delay 
`package.json` 파일을 열고 `json-server` 스크립트에 딜레이를 추가한다. (5초 지연 = --delay 5000)

```json
"scripts" : {
  "sever" : "json-server --watch db.json --port 4000 --delay 5000"
}
```

#### :pushpin: 1-2) 서버 시작
JSON 서버를 시작한다.

```bash
# npm
npm run server

# yarn
yarn server
```

#### :pushpin: 1-3) 비동기 데이터 로딩
첫 번째 방법으로 비동기적으로 데이터를 로딩 해서 페이지의 다른 요소들이 먼저 렌더링 될 수 있도록 적용

```tsx
const [isLoading, setIsLoading] = useState(false);
const [data, setData] = useState<Product[]>([]);

useEffect(() => {
  setIsLoading(true);
  fetch('http://localhost:4000/products')
    .then((res) => res.json())
    .then((data) => {
      setData(data);
      setIsLoading(false);
    });
}, []);

if (isLoading) return <div>Loading...</div>;

return (
  {/* Loading 후 */}
)
```

#### :pushpin: 1-4) 캐싱 활용
데이터가 자주 변경되지 않는다면, 캐싱을 호라용해서 데이터 로딩 시간을 단축할 수 있다. 빌드 시 데이터를 미리 가져오고, 이를 저엊ㄱ 페이지로 제공할 수 있음. (API가 느리다면 빌드시간도 오래걸림)

```tsx
import ProductList from '@/components/ProductList';
import { Product } from '@/types';

const ProductsPage = async () => {
  const response = await fetch('http://localhost:4000/products',
  {cache: "force-cache"});
  const products: Product[] = await response.json();

  return (
    <div>
      <h1>Products</h1>
      <ProductList products={products} />
    </div>
  );
};

export default ProductsPage;
```

### :two: Loading UI, Suspense 사용하기

#### :pushpin: 2-1) 로딩 컴포넌트 작성
로딩 상태를 표시할 간닿난 컴포넌트를 작성함

- 로딩 컴포넌트를 `app` 하위에 생성할 경우 전역으로 사용됨
- 만약 특정 폴더 안에서 loading 컴포넌트를 생성하면 특정 폴더 하위 컴포넌트에서 사용됨
  - `app > product > loading.tsx` : product 페이지의 로딩임

```tsx
// 전역 사용
// app > loading
export default function Loading() {
  return <div>Loading...</div>;
}

// 특정 컴포넌트 로딩 처리 product>loading.tsx
export default function Loading(){
  return (
    <div>
      프로덕트 페이지 로딩
    </div>
  )
}

```

#### :pushpin: 2-2) Suspense, Streaming SSR 사용
`Next.js 14`에서는 React의 서스펜스와 스트리밍 기능을 사용해서 서버 사이드 렌더링을 더욱 효율적으로 처리할 수 있으며 이는 데이터를 부분적으로 스트리밍하여 사용자가 페이지를 더 빨리 볼 수 있게 한다.


1. `ProductList`와 `NewProductList`에서 각각 바라보는 JSON 서버를 다르게 주고 Delay도 다르게 적용했다.
2. `Suspense`를 사용해서 딜레이되는 시간동안 만들어놓은 `Loading` UI가 보이게 적용함
3. [1] 번에서 Delay를 다르게 주었지만 먼저 로딩되는 순서대로 화면에 노출됨

```tsx
import { Suspense } from "react";
import NewProductList from "./_components/NewProductList";
import ProductList from "./_components/ProductList";
import Loading from "./loading";

export default async function Home() {
  return (
    <>
      <Suspense fallback={<Loading/>}>
        <ProductList/>
      </Suspense>  
      <div className="p-8 m-4">
        <Suspense fallback={<Loading/>}>
          <NewProductList/>
        </Suspense>                              
      </div>            
    </>
  );
}

```

### :fire: 마무리
`Next.js`의 `Loading UI`, `Suspense`, `Streaming SSR`은 모두 사용자 경험을 개선하고 페이지 로딩 속도를 최적화하는 데 매우 중요한 역할을 한다. 특히 많은 데이터와 느린 네트워크 환경에서 이러한 기술을 적절히 활용하면 사용자에게 더 좋은 웹 사이트 경험을 제공할 수 있다.