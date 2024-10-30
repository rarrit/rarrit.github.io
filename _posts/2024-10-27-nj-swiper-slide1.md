---
title: "[Next.js] swiper 라이브러리를 사용한 공통 슬라이드 만들기 1차"
date: 2024-10-27
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
  - lib
tags:
  - swiper
  - slide
author_profile: true
sidebar_main: true
---

## :ledger: [Next.js] swiper 라이브러리를 사용한 공통 슬라이드 만들기 1차

next.js 에서 swiper 라이브러리를 사용해서 공통 슬라이드를 만들어 봤다. 이전에 javascript로 슬라이드를 구현해봐서 next.js에서도 구현해보려 했지만, 맡은 기능과 일정을 고려해봤을 때 일단 기능 구현부터 하기로했고, 추후 시간이 남으면 적용하기로했다.

### :one: swiper 설치

처음 [공식문서](https://swiperjs.com/react)에서 next.js가 없어서 당황했지만 react를 참고해서 진행했다. 이번 프로젝트에서는 `pnpm` 을 사용해서 터미널에 아래와 같이 입력 후 `swiper` 라이브러리를 설치했다.

```bash
pnpm add swiper
```

### :two: swiper 기본 옵션 사용 및 타입 정의

`swiper`라이브러리에 기본적으로 내장되어있는 옵션들을 최대한 활용해서, 사용자가 다양하게 커스텀할 수 있도록했다. 옵션은 아래와 같다.

- `slidePerview`: 슬라이드 개수
- `spaceBetween`: 간격
- `onChangeEvent`: 슬라이드 변경 이벤트
- `useAutoplay`: 자동 재생
- `useNavigation`: 네비게이션
- `usePagination`: 페이지네이션 기능

```ts
type SlideProps = {
  slidePerview?: number;
  spaceBetween?: number;
  onChangeEvent?: () => void;
  useAutoplay?: boolean;
  useNavigation?: boolean;
  usePagination?: boolean;
};
```

사용할 옵션들을 props를 통해 전달받도록 작업했다.

### :three: 1차 PR

처음 PR을 했을 때 뭘 잘못했는지 못느꼇다. 근데, 이후에 작업을 하면서 정말 스스로에게 어이없었는데.. 문제의 코드는 아래와 같다.

- `문제의 코드`

<details>
<summary>문제의 코드 보기</summary>
<div markdown="1">

```ts
"use client";

import { useEffect } from "react";
import { Swiper, SwiperSlide } from "swiper/react";
import { Autoplay, Navigation, Pagination } from "swiper/modules";

import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";

type SlideProps = {
  slidePerview?: number;
  spaceBetween?: number;
  onChangeEvent?: () => void;
  useAutoplay?: boolean;
  useNavigation?: boolean;
  usePagination?: boolean;
};

// props는 해당 컴포넌트에서 직접 값을 정의함
const Slide = ({
  slidePerview,
  spaceBetween,
  onChangeEvent,
  useAutoplay = true,
  useNavigation = true,
  usePagination = true,
}: SlideProps) => {
  useEffect(() => {
    // 페이지에서 SSR로 랜더링 시 커스텀 훅으로 생성해서 전달 받아야함
    if (onChangeEvent) onChangeEvent();
  }, [onChangeEvent]);

  const dummyData = [
    { id: 1, name: "slide 1" },
    { id: 2, name: "slide 2" },
    { id: 3, name: "slide 3" },
    { id: 4, name: "slide 4" },
  ];

  // 활성화할 모듈을 조건부로 설정
  const modules = [
    ...(useAutoplay ? [Autoplay] : []),
    ...(useNavigation ? [Navigation] : []),
    ...(usePagination ? [Pagination] : []),
  ];

  return (
    <Swiper
      spaceBetween={spaceBetween}
      slidesPerView={slidePerview}
      onSlideChange={onChangeEvent}
      loop={false}
      autoplay={useAutoplay}
      modules={modules}
      navigation
      pagination={{ clickable: true }}
      className="h-[300px] w-full"
    >
      {dummyData.map((data) => (
        <SwiperSlide
          key={data.id}
          className="flex items-center justify-center bg-gray-200"
        >
          {data.name}
        </SwiperSlide>
      ))}
    </Swiper>
  );
};

export default Slide;
```

</div>
</details>

문제는 다음과 같았다.

1. 처음 데이터가 없다고 더미데이터로 테스트 후 그대로 PR을 올렸다. 그렇다면 작업자 입장에서 사용할 때, 더미 데이터로 되어있는 슬라이드를 보고 `?` 할 것이다.
2. `슬라이드 콘텐츠가 고정`되어 슬라이드를 커스텀 할 수 없다. 더미데이터도 문제지만 `<SwiperSlide>` 안쪽 요소에 `children` 을 주지 않고 더미데이터의 이름을 주고잇었다.

솔직히 슬라이드 사용하는 첫 번째 타자가 나여서 다행이지 만약 다른 작업자가 이 코드를 받고 진행한다 했으면, 나에 대한 안좋은 감정이 생겻을 수도..! 그리고 이와 같이 작업하면서 PR 할때 코드 리뷰가 얼마나 중요한지 다시 한번 느끼게 된 계기였다.

### :four: 1차 작업의 문제점 보완

위에서 설명한 것들을 수정한 코드는 아래와 같다.

- `수정된 코드`

<details>
<summary>수정된 코드 보기</summary>
<div markdown="1">

```ts
"use client";

import { ReactNode, useEffect } from "react";
import { Swiper, SwiperSlide } from "swiper/react";
import { Autoplay, Navigation, Pagination } from "swiper/modules";

import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";

type SlideProps = {
  slidePerview?: number;
  spaceBetween?: number;
  onChangeEvent?: () => void;
  useAutoplay?: boolean;
  useNavigation?: boolean;
  usePagination?: boolean;
  children: ReactNode;
};

// props는 해당 컴포넌트에서 직접 값을 정의함
const Slide = ({
  slidePerview,
  spaceBetween,
  onChangeEvent,
  useAutoplay = true,
  useNavigation = true,
  usePagination = true,
  children, // children 사용하여, 안쪽 컨텐츠가 자유롭게 적용할 수 있게 함
}: SlideProps) => {
  useEffect(() => {
    // 페이지에서 SSR로 랜더링 시 커스텀 훅으로 생성해서 전달 받아야함
    if (onChangeEvent) onChangeEvent();
  }, [onChangeEvent]);

  // 활성화할 모듈을 조건부로 설정
  const modules = [
    ...(useAutoplay ? [Autoplay] : []),
    ...(useNavigation ? [Navigation] : []),
    ...(usePagination ? [Pagination] : []),
  ];

  return (
    <Swiper
      spaceBetween={spaceBetween}
      slidesPerView={slidePerview}
      onSlideChange={onChangeEvent}
      loop={false}
      autoplay={useAutoplay}
      modules={modules}
      navigation
      pagination={{ clickable: true }}
      className="w-full"
    >
      {Array.isArray(children) ? (
        children.map((child, index) => {
          return <SwiperSlide key={index}>{child}</SwiperSlide>;
        })
      ) : (
        <SwiperSlide>{children}</SwiperSlide>
      )}
    </Swiper>
  );
};

export default Slide;
```

</div>
</details>

위의 코드로 변경하며 개선한 내용은 아래와 같다.

1. `children을 통한 동적 콘텐츠 적용`하여 `Slide` 컴포넌트에서 `children`을 사용하여 외부에서 전달되는 콘텐츠를 자유롭게 표시할 수 있도록 개선했다. 이를 통해 안쪽 children 요소에 다양한 콘텐츠를 배치할 수 있게 되었다.
2. children이 배열인지 단일 요소인지에 따라 조건부 렌더링을 적용해, 배열인 경우 각 항목을 SwiperSlide로 렌더링하고 단일 요소일 때도 제대로 렌더링되도록 적용했다. 데이터가 한 개일때를 고려해서 안정성을 높였다? 이 표현이 맞는지 좀 어색한데, 그렇다.

### :five: 사용 예시

test page를 생성하고 아래와 같이 적용했을 때, Slide의 안쪽 콘텐츠가 잘 적용되어 노출되었고, `onChangeEvent` 또한 잘 적용되는 것을 확인할 수 있었다.

```ts
"use client";

import Slide from "@/_components/slide/Slide";

const SlideTestPage = () => {
  const test = () => {
    console.log("test");
  };
  return (
    <Slide slidePerview={3} spaceBetween={10} onChangeEvent={test}>
      <div>슬라이드1</div>
      <div>슬라이드2</div>
    </Slide>
  );
};

export default SlideTestPage;
```

### :fire: 마무리

현재 내 지식으로는 여기까지가 최선이었으며, 튜터님의 피드백을 통해 리팩토링 기간에 많은 옵션을 추가하고 다양항 상황에서 사용될 수 있는 공통 슬라이드를 만들어보고싶다. 이전에 작성한 모달도 이후에 수정이 되었는데.. 공통 작업은 많이 어려운 것 같다.
