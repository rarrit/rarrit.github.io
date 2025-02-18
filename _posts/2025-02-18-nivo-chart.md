---
title: "[React] 리액트 차트 라이브러리 Nivo Chart 사용법"
date: 2025-02-18
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
  - lib
tags:
  - chart
author_profile: true
sidebar_main: true
---

## :ledger: [React] 리액트 차트 라이브러리 Nivo Chart 사용법

프로젝트에서 차트를 사용할 일이 있어서 어떤 라이브러리를 사용할 지 구글링한 결과 `Nivo Chart`를 알게 되었다. `Nivo Chart` 가 무엇인지, 사용법은 어떻게 되는지 정리해봤다.

### :one: Nivo Chart란?

[Nivo](https://nivo.rocks/)는 **React 기반 데이터 시각화 라이브러리**로 다양한 차트를 손쉽게 만들 수 있다. D3.js(데이터 시각화 무료 오픈소스 javascript 라이브러리 라고함 [What is D3?](https://d3js.org/what-is-d3))를 기반으로 만들어졌으며, `SVG`, `Canvas`, `WebGL`을 활용한 차트 렌더링을 지원해서 성능이 좋다고한다.

### :two: Nivo의 장점과 단점

#### :pushpin: 2-1) 장점

1. **손쉬운 사용** → `props`만으로 대부분의 차트 설정 가능
2. **반응형 지원** → `responsive` 옵션으로 화면 크기에 따라 자동 조정
3. **다양한 차트 제공** → 바 차트, 라인 차트, 파이 차트, 트리맵 등
4. **애니메이션 지원** → 부드러운 차트 트랜지션 효과
5. **SVG & Canvas 지원** → 성능 최적화 가능
6. **테마 커스터마이징 가능** → 색상, 스타일을 자유롭게 설정

#### :pushpin: 2-2) 단점

1. **D3.js에 비해 커스터마이징이 제한적** → 매우 복잡한 차트 커스텀은 어렵다
2. **패키지 용량이 비교적 큼** → 불필요한 차트까지 포함될 가능성 있음 (`필요한 것만 인스톨하자`)
3. **학습 곡선** → 기본적인 차트는 쉽지만, 고급 기능 사용 시 학습 필요

### :three: Nivo Chart 설치 방법

설치 방법은 아래와 같다. 필요한 것만 설치!

- `@nivo/core`: 기본 컴포넌트 및 스타일 (필수적으로 설치해야함)
- `@nivo/사용할 차트`: 사용하려는 차트
  - `@nivo/bar` → 바 차트
  - `@nivo/line` → 라인 차트
  - `@nivo/pie` → 파이 차트
  - `@nivo/radar` → 레이더 차트
  - `@nivo/heatmap` → 히트맵
  - `@nivo/geo` → 지도 시각화

```bash
// yarn
yarn add @nivo/core @nivo/bar @nivo/line @nivo/pie @nivo/radial-bar @nivo/radar @nivo/tree @nivo/boxplot

// npm
npm i @nivo/core @nivo/bar @nivo/line @nivo/pie @nivo/radial-bar @nivo/radar @nivo/tree @nivo/boxplot
```

### :four: Nivo Chart 사용 방법

이번에 사용할 차트는 Bar 차트여서 [Nivo](https://nivo.rocks/bar/)에서 우측에 있는 `chart`, `code`, `data` 를 확인하여 적용해봤다.

#### :pushpin: 4-1) 차트 컴포넌트 생성

모든 라이브러리가 그렇듯 옵션을 처음보면 머리가 아픈데, 어쩔 수 없이 하나씩 테스트해봤다. (주석으로 남겨둠)

```jsx
// components/example/Chart.jsx
import { ResponsiveBar } from "@nivo/bar";

const Chart = ({ data /* see data tab */ }) => {
  console.log("data ===>", data); // 상위컴포넌트에서 props로 잘 전달되었는지 확인해봄
  return (
    <ResponsiveBar
      data={data} // 표시할 데이터
      keys={["hot dog", "burger", "sandwich", "kebab", "fries", "donut"]} // 데이터의 키 값
      indexBy="country" // 막대그래프 X축(분류 기준) 설정
      margin={{ top: 50, right: 130, bottom: 50, left: 60 }} // 차트 바깥 여백
      padding={0.3} // 막대 간 간격
      valueScale={{ type: "linear" }} // "linear" 값이 선형적으로 배치됨
      indexScale={{ type: "band", round: true }} // band: 범주형 데이터(카테고리), round: 반올림
      colors={{ scheme: "yellow_orange_red" }} // 막대 색상
      defs={[
        // 패턴 정의
        {
          id: "dots",
          type: "patternDots",
          background: "inherit",
          color: "#38bcb2",
          size: 4,
          padding: 1,
          stagger: true,
        },
        {
          id: "lines",
          type: "patternLines",
          background: "inherit",
          color: "#eed312",
          rotation: -45,
          lineWidth: 6,
          spacing: 10,
        },
      ]}
      fill={[
        // 특정 항목 패턴
        {
          match: {
            id: "fries",
          },
          id: "dots",
        },
        {
          match: {
            id: "sandwich",
          },
          id: "lines",
        },
      ]}
      borderColor={{
        from: "color",
        modifiers: [["darker", 1.6]],
      }}
      axisTop={null} // 상단 설정
      axisRight={null} // 우측 설정
      axisBottom={{
        // 하단 설정
        tickSize: 5,
        tickPadding: 5,
        tickRotation: 0,
        // legend: "country",
        legendPosition: "middle",
        legendOffset: 32,
        truncateTickAt: 0,
      }}
      axisLeft={{
        // 좌측 설정
        tickSize: 10,
        tickPadding: 5,
        tickRotation: 0,
        // legend: "food",
        legendPosition: "middle",
        legendOffset: -40,
        truncateTickAt: 0,
      }}
      labelSkipWidth={12} // 막대 크기 일정 크기 이하일경우 숨김
      labelSkipHeight={12} // 막대 크기 일정 크기 이하일경우 숨김
      labelTextColor={{
        from: "color",
        modifiers: [["darker", 1.6]],
      }}
      legends={[
        // 차트 오른쪽 하단 표시될 범례
        {
          dataFrom: "keys",
          anchor: "bottom-right", // 오른쪽 아래
          direction: "column",
          justify: false,
          translateX: 120,
          translateY: 0,
          itemsSpacing: 2,
          itemWidth: 100,
          itemHeight: 20,
          itemDirection: "left-to-right",
          itemOpacity: 0.85,
          symbolSize: 20,
          effects: [
            {
              on: "hover",
              style: {
                itemOpacity: 1,
              },
            },
          ],
        },
      ]}
      role="application" // 애플리케이션 요소로 인식
      ariaLabel="Nivo bar chart demo" // 스크린 리더가 읽을때 사용할 설명
      barAriaLabel={(e) =>
        e.id + ": " + e.formattedValue + " in country: " + e.indexValue
      } // 막대의 값, 카테고리 나타내는 함수
    />
  );
};

export default Chart;
```

#### :pushpin: 4-1) 차트 컴포넌트 사용

사용할 컴포넌트에서 import 해서 데이터를 `props`로 전달해서 사용했다.

- `Chart` 컴포넌트의 경우 상위 요소에서 width, height를 지정하지 않으면 노출이 않되니 설정해주도록한다.

```jsx
const Example = () => {
  const chartData = [
    {
      country: "AD",
      "hot dog": 200,
      "hot dogColor": "hsl(179, 70%, 50%)",
      burger: 187,
      burgerColor: "hsl(26, 70%, 50%)",
      sandwich: 36,
      sandwichColor: "hsl(63, 70%, 50%)",
      kebab: 191,
      kebabColor: "hsl(93, 70%, 50%)",
      fries: 123,
      friesColor: "hsl(250, 70%, 50%)",
      donut: 155,
      donutColor: "hsl(237, 70%, 50%)",
    },
    // .... 복사해온 값
  ];

  return (
    <div className="w-[100vw] h-[300px]">
      <Chart data={chartData} />
    </div>
  );
};
```

### :five: Nivo 선택 이유

1. 구글 검색했을 때 가장 많이 추천하고 있는 차트였다. (그럼 사용한 사람이 많으니 참고 글도 많을거라 생각)
2. 반응형이 가능하고 기본적이 차트 사용시에 어려움은 크게 없다.
3. 커스텀이 가능하다.

### :fire: 마무리

리액트에 다양한 차트 라이브러리가 있지만 처음으로 사용해본 라이브러리라 다음에 다른 라이브러리를 사용해보면 `Nivo` 사용성에 대해 좀 더 명확하게 판단할 수 있을 것 같다.
