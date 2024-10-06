---
title: "[Next.js] use client 컴포넌트 metadata 에러"
date: 2024-10-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - metadata
author_profile: true
sidebar_main: true
---

## :ledger: next.js의 use client 컴포넌트에서 metadata 설정하던 중 겪은 에러
next.js를 사용하여 프로젝트를 진행하며 metadata를 설정하던 중 겪은 에러다.

### :one: 개요
로테이션 챔피언 데이터를 불러오는 페이지에서 metadata를 설정해주었다. 설정한 코드는 아래와 같다.

- **작성 코드**

```tsx
"use client";

import { Suspense, useEffect, useState } from "react";
// import { getChampions, getRotations } from "../utils/serverApi";
import { ChampionInfo } from "../types/Champion";
import { getChampionRotation } from "../utils/riotApi";
import Image from "next/image";
import { RIOT_BASE_URL } from "../api/apiKey";
import Link from "next/link";
import Loading from "./loading";

export const metadata = {
  title: 'LOL INFO APP - Rotation 페이지',
  description: '리그오브레전드 로테이션 챔피언 페이지입니다.',
};


const RotationPage = () => {
  const [rotationChampion, setRotationChampion] = useState<ChampionInfo[]>([]); 

  useEffect(() => {
    const fetchData = async () => {
      const rotationsChampion = await getChampionRotation();
      console.log(rotationsChampion);
      // 필터링된 로테이션 챔피언 목록을 상태에 저장
      setRotationChampion(rotationsChampion);
    };

    fetchData();
  }, []);

  

  return (
    ... 코드코드
  )
```

위와 같이 작성하고 저장하니 아래와 같은 에러가 노출했다..!

![metadata Error](https://github.com/user-attachments/assets/3e0658d3-8a58-42fb-bdd1-40f017bfc974)

### :two: 원인
Next.js 에서 에러는 항상 친절한 것 같다. 내용은 `You are attempting to export "metadata" from a component marked with "use client", which is disallowed. Either remove the export, or the "use client" directive. Read more: ` 으로 번역하면 "use client" 컴포넌트에서 메타데이터를 내보내려고 시도하는데 이를 혀용하지 않는다고 한다.

### :three: 해결방법
그렇다면, CSR 환경에서 metadata를 설정할 수 없다는 것인데.. 해당 페이지는 필수 구현 요소 중 하나로 CSR로 구현하는 것이었다. 해결하기 위해 생각한 내용은 아래와 같다.

1. 처음 생각한 방법은 서버 컴포넌트로 변경하려 했지만 필수 구현 조건에 해당하지 않아서 PASS..! 
2. 그렇다면 상위 컴포넌트를 서버 컴포넌트로 만들면 되지 않을까? 
  - 클라이언트 로직을 분리해서 서버 컴포넌트에서 불러오면 될 것 같지만 현재 프로젝트가 완성 단계라 솔직히 건들기가 싫었음.. 다른 방법은 없는지 찾아보았다.
3. Next.js에서 기본적으로 공통 layout.tsx 에서 메타 데이터를 관리하는데, 그렇다면 rotation 페이지에서 layout.tsx 를 생성해서 메타데이터를 작성해준다면? 가능하지 않을까란 생각을 하게되었다.

#### :pushpin: 3-1) 코드 적용

```tsx
export const metadata = {
  title: 'LOL INFO APP - Rotation 페이지',
  description: '리그오브레전드 로테이션 챔피언 페이지입니다.',
};

const RotationLayout = ({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) => {
  
  return (
    <>{children} </>
  )
  
}

export default RotationLayout
```

#### :pushpin: 3-2) 적용 화면
레이아웃에서 적용 후 화면을 확인해보니 잘 적용된 것을 확인할 수 있었다.

![meta-example](https://github.com/user-attachments/assets/03177d9b-9cc7-445f-ba14-a9b81a1a5dfa)


### :fire: 마무리
Next.js 를 배우며, 어려운 부분이 많지만 프레임워크로서 정말 다양한 기능을 제공해주는 것 같다. 해결방법 중 2번으로 갔으면 뭔가 작업이 오래 걸렸을 것 같은데 다행히 3번을 통해 쉽게 적용할 수 있어서 좋았다.