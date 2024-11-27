---
title: "[6차] 스파르타 최종 프로젝트 - 메인 페이지 작업"
date: 2024-11-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: [6차] 스파르타 최종 프로젝트 - 메인 페이지 작업

최종 프로젝트에서 메인 페이지를 배정받아 작업을 진행했다.

### :one: 공통 섹션 컴포넌트 작업

먼저 와이어프레임을 확인 후 메인에서 반복적으로 사용되는 컴포넌트를 분리했다. `MainSection` 같은 경우가 가장 반복적으로 사용되는데, 중간에 배경색상이 들어가는 타입도 있어서 아래와 같이 적용했다.

```tsx
type MainSectionProps = {
  background?: string;
  children: React.ReactNode;
};

const MainSection = ({ background, children }: MainSectionProps) => {
  return (
    <div
      className="main_section py-[40px] max-767:py-[20px]"
      style={{ background: `${background}` }}
    >
      <div className="m-auto w-full max-w-[1360px] px-[30px] max-767:px-[15px]">
        {children}
      </div>
    </div>
  );
};

export default MainSection;
```

`props`로 background와 children을 전달 받아 배경이 있는 타입과 아닌 타입으로 분류하고, type에 옵셔널로 지정해주었다.

### :two: 페이지 구조

공통으로 사용하는 `MainSection` 컴포넌트로 각 섹션을 분리하고, 안쪽 컨텐츠는 레이아웃은 태그와 tailwind로 ui를 만들었고, 안쪽의 기능이 필요한 부분은 따로 컴포넌트로 분리를 했다. 아래와 같이 작업하면서, 메인 페이지는 서버 컴포넌트로 적용하여 `metadata`를 사용하려했고, 안쪽 컴포넌트는 각 기능에 맞게 클라이언트 컴포넌트와 서버 컴포넌트로 분리해서 작업을 진행했다.

```tsx
import MainCamps from "@/_components/main/MainCamps";
import MainIcons from "@/_components/main/MainIcons";
import MainReviews from "@/_components/main/MainReviews";
import MainSection from "@/_components/main/MainSection";
import MainSlide from "@/_components/main/MainSlide";
import MainTop from "@/_components/main/MainTop";
import MainUserCard from "@/_components/main/MainUserCard";
import MainSearch from "@/_components/main/MainSearch";

import {
  dehydrate,
  HydrationBoundary,
  QueryClient,
} from "@tanstack/react-query";
import Link from "next/link";
import { SERVER_PAGE_URL } from "@/_utils/common/constant";

export default function Home() {
  const queryClient = new QueryClient();

  return (
    <HydrationBoundary state={dehydrate(queryClient)}>
      <div className="main_area">
        <div className="inner">
          {/* main_top */}
          <MainSection>
            <MainTop />
            <MainSearch />
          </MainSection>
          {/*// main_top */}

          {/* main_sec01 */}
          <MainSection background="#fff">
            <div className="flex flex-wrap content-stretch gap-[20px]">
              <div className="main-slide-wrap w-[calc(100%-515px)] overflow-hidden rounded-[12px] shadow-custom max-1460:w-[calc(100%-500px)] max-1280:w-[calc(100%-400px)] max-1160:w-[calc(100%-360px)] max-989:w-full">
                <MainSlide />
              </div>
              <div className="flex-1 rounded-[12px] border border-[#d9d9d9] bg-[#fff] p-[30px] max-1160:p-[20px]">
                <MainUserCard />
              </div>
            </div>
          </MainSection>
          {/*// main_sec01 */}

          {/* main_sec02 */}
          <MainSection>
            <MainIcons />
          </MainSection>
          {/*// main_sec02 */}

          {/* main_sec02 */}
          <MainSection background="#fff">
            <h2 className="flex items-center justify-center text-center text-[32px] font-bold max-767:text-[24px]">
              Hot Best Pick
            </h2>
            <div className="mb-[30px] mt-[20px] flex items-center justify-end max-989:mb-[20px] max-989:mt-[15px]">
              <Link
                href={SERVER_PAGE_URL.camps(1)}
                className="text-[18px] max-989:text-[16px] max-767:text-[14px]"
              >
                캠핑장 더보기
              </Link>
            </div>
            <MainCamps />
          </MainSection>
          {/*// main_sec02 */}

          {/* main_sec03 */}
          <MainSection background="#F2F2F2">
            <h2 className="flex items-center justify-center text-center text-[32px] font-bold max-767:mt-[20px] max-767:text-[24px]">
              후기
            </h2>
            <MainReviews />
          </MainSection>
          {/*// main_sec03 */}
        </div>
      </div>
    </HydrationBoundary>
  );
}
```

### :three: 기능 컴포넌트

기능이 들어가는 컴포넌트에서는 맡은 기능이 아닌 경우 ui 처리만 완료하고 데이터는 더미 데이터를 사용하여 적용했다. 이후에 PR을 올릴 때 각 영역의 컴포넌트에 대해 자세히 작성해주었다.

#### :pushpin: 3-1) 타이틀명: 메인 1차 작업 완료

PR: [https://github.com/wldud7788/fire-spot/pull/18](https://github.com/wldud7788/fire-spot/pull/18)

1. `[작업 완료]`

- MainSection: 공통 섹션 컴포넌트
- MainTop: 메인 최상단 영역, 검색바 추가 필요
- MainSlide: 메인 슬라이드 영역, Slide 컴포넌트로 구성됨
- MainUserCard: 메인 유저 정보 영역, 비회원/로그인 조건 처리
- MainIcons: 메인 아이콘 리스트 영역, 이미지와 텍스트 전달받으면 처리
- MainCamps: 메인 캠프 리스트 영역, getTotalData 함수를 통해 리스트 출력

2. `[변경 사항]`

- `Slide.tsx`
  - 기존: swiper 기본 옵션만을 사용해서 출력되게 되어있음
  - 문제발생: 슬라이드 안쪽 컨텐츠를 자유롭게 적용이 불가능함 (컴포넌트 내에서 지정한 컨텐츠만 노출됨)
  - 수정: 공통으로 사용하기 위해 children 으로 변경해주고, 데이터가 1개일 때 한개의 데이터만 노출될 수 있도록 조건부 처리해줌
- `CampCard.tsx`
  - 메인과 일반 리스트에 디자인 및 데이터 출력이 달라서, props로 type을 추가해줬습니다.
  - 기존 CampCard에 width가 적용되어, 다른 컴포넌트에서 사용할 때 적용된 width만 사용할 수 있어서 삭제해줬습니다. (사용하는 컴포넌트에서 넓이 지정,
- `getTotalData`
  - 메인에서 데이터 4개만 필요해서, 기존 캠핑 전체 리스트를 불러오는 서버액션 함수 파라미터에 numOfRows 값을 추가했습니다.
  - numOfRows: 노출 갯수

3. `[대기 및 요청 사항]`

- 메인 최상단 검색바 적용 필요 (ㅈㅇ님)
- 아이콘 리스트 적용 필요
- 후기 리스트 적용 필요 (ㅇㅈ님)

### :fire: 회고

이전에는 모든 작업 후 commit 을 했었는데, 이번 팀 프로젝트에서는 자잘하게 기능이 완성 할 때 마다 커밋을 해서 다른 팀원은 모르겠지만 개인적으로 어떤 기능을 어떻게 작업했는지 보기가 좀 더 수월했다. 컴포넌트의 경우 스스로 정의된 것이 없다보니 반복되는 것과 기능 중심으로 분리했는데, 스스로는 만족했지만 잘 분리했는지는 솔직히 자신은 없는 것 같다.
