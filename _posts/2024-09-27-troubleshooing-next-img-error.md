---
title: "[Next.js] Img 에러 해결 방법 Warning: Using 'img' could result in slower LCP ..."
date: 2024-09-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - next js error
  - img error
author_profile: true
sidebar_main: true
---

## :ledger: Next.js에서 Img 에러 해결 과정
이번 Next.js 공부중에 겪은 img error 입니다. 

### :one: 개요
SSG를 테스트하기 위해 터미널에 `yarn build && yarn start` 명령어를 입력했더니 아래와 같은 에러 메시지가 터미널에 출력되었다.

![troubleshooting-next-img-error](https://github.com/user-attachments/assets/bfb50955-994c-46c7-a4de-ed1ac87f860b)


### :two: 원인
- 경고는 `Next.js` 프로젝트에서 `<img>` 태그 대신 Next.js에서 제공하는 `<Image />` 컴포넌트를 사용하라는 권고였다. 
- `<Image>` 컴포넌트는 `Next.js`에서 이미지를 자동으로 최적화하여 **LCP(최대 콘텐츠 페인트 - LightHouse 테스트해보면 앎)** 성능을 개선하고 필요한 경우 더 낮은 대역폭을 사용하도록 도와줌

### :three: 해결방법
해결 방법은 아래와 같다.

1. Next.js의 Image 컴포넌트를 import 해준다.
2. `<img>` 태그를 `<Image>` 태그로 변경한다.

```tsx
import { Product } from "../page";
// 임폴트하셈
import Image from "next/image";

const NewProductList = async () => {
  const res = await fetch("http://localhost:4001/products", {
    cache: "no-store",
  });
  const data: Product[] = await res.json();
  
  const newData = data.filter((p) => !p.isNew);

  // Error 발생시키기
  if(Math.random() > 0.5) throw new Error("오류!");
  

  return (
    <div className="flex gap-2 oveflow-auto ">
      {newData.map((product) => (
        <div className="flex" key={product.id}>
          {/* 사용하셈 */}
          <Image
            className="rounded-sm object-scale-down"
            width={80}
            src={product.images}
            alt={product.title}
          />
          <div className="flex flex-col justify-between">
            <div>
              <h2 className="text-md font-medium">{product.title}</h2>
              <p className="mt-4 font-thin">{product.price.amount}$</p>
            </div>
          </div>
        </div>
      ))}
    </div>
  );
};

export default NewProductList;
```

### :fire: 마무리
처음 에러 메시지를 보고 Image 를 작성 후 엔터를 쳐도 import가 되지 않아서 구글링을 진행해서 적용했다. Image 컴포넌트로 변경 후 해결했다.

#### :pushpin: 도움이 된 블로그
- ![Next Js에서 img 오류 해결방법 - eh_space](https://velog.io/@space086/Next-Js%EC%97%90%EC%84%9C-img-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95)