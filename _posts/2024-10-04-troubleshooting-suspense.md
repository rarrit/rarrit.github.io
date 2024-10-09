---
title: "[Next.js] CSR 환경에서 Suspense 사용해보았다."
date: 2024-10-04
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - hydration
author_profile: true
sidebar_main: true
---

## :ledger: next.js에서 use client 환경에서 서스펜스를 사용해보았다.
이번 Next.js를 사용한 [리그오브레전드 정보 프로젝트](https://rarrit.github.io/mini/til/next-lol06/)에서 겪었던 에러입니다.

### :one: 개요
이번 Next.js 프로젝트에서 `Suspense` 를 사용하여 로딩작업 중 CSR환경에서 적용되지 않는 이슈가 있었다. 어떤 문제였고 어떻게 해결했는지 작성해보았다.

### :two: 원인
CSR에서 서스펜스를 사용하려고 시도했을 때 발생했던 문제는 데이터 패칭과 관련된 로딩 상태처리였다. 서스펜스는 데이터가 준비되지 않았을 때 대체 UI를 표시하는 기능이 있지만 Next.js의 `use client` 환경, 즉 CSR 환경에서 사용할 때 아래와 같은 제한이 있었다.

1. React 의 Suspense가 CSR에서 비동기 데이터 로딩을 처리하지 않음
  - 그냥 안되는거다. 서스펜스는 클라이언트 환경에서 데이터 패칭을 직접적으로 지원하지 않으며 해당 환경에서 사용할 경우 제대로 작동하지 않거나 서버측에서 미리 데이터를 준비하는 형태가 아니라면 로딩 상태가 안나옴
2. 서스펜스는 원래 서버 측에서 데이터를 미리 로드함
  - 클라이언트 환경에선 비동기 데이터를 처리하지 못함. 고로 CSR환경에서 서스펜스를 사용하려했던 것 자체가 `?` 였음..

### :three: 해결방법
해결 방법은 기존에 서스펜스로 로딩처리를 했던 방법에서 `useState`를 사용해서 로딩 처리를 적용했다. 

```tsx

const RotationPage = () => {
  const [rotationChampion, setRotationChampion] = useState<ChampionInfo[]>([]); 
  const [loading, setLoading] = useState<boolean>(true);
  
  useEffect(() => {
    const fetchData = async () => {
      const rotationsChampion = await getChampionRotation();
      console.log(rotationsChampion);
      // 필터링된 로테이션 챔피언 목록을 상태에 저장
      setRotationChampion(rotationsChampion);
      setLoading(false);
    };

    fetchData();
  }, []);
  
  return (
    <div id="championList" className="w-full bg-lol03 bg-fixed bg-center bg-no-repeat py-[60px] px-[15px]">
    { 
      loading ? (
        <div className="loading-ui fixed z-[9999] bg-[#111]">
          <p className="bg-loading">쉿! 로테이션 데이터 받아오는중</p>
        </div>
      ) :  rotationChampion.length > 0 ? (
        // 데이터 출력
      ) : (
        <p>로테이션 챔피언이 없습니다.</p>
      )
    }
    </div>
  )
}
```


### :fire: 마무리
Next.js의 4가지 렌더링 환경에서 작업을 해보면서 어떤 차이점이 있는지 이번 프로젝트를 통해 많이 이해해볼 수 있는 기회인 것 같다. `Next.js`의 CSR 환경에서 React의 서스펜스를 사용하는 것은 잘못되었으며, 비동기 데이터를 처리하는 과정에서 사용하는게 옳은 것임을 알 수 있게 되었다. 요즘 공부하면서 사용법에 대해서만 집중하고 어떻게 동작되는지 이해하지 않고 넘어갔던 폐해가 아닐까 싶다. 