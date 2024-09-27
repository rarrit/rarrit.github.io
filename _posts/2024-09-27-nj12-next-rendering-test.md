---
title: "[Next.js] 렌더링 패턴(CSR,SSG,ISR,SSR) 구현해보기"
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
  - next rendering
  - csr
  - ssg
  - isr
  - ssr
author_profile: true
sidebar_main: true
---

## :ledger: Next.js와 Json-server를 사용해서 렌더링 4가지 패턴 구현하기
MPA, SPA에 대해 알아보고 Next.js의 렌더링 기법이 무엇이 있는지 알아보자!

### :one: SSG 환경 테스트 해보기
테스트하기 위해 Json-server를 사용했으며, 터미널에 `yarn build && yarn start` 후 모든 콘텐츠가 빌드된 시점의 상태로 고정되었다. 이는 `SSG`의 특성으로 동적인 데이터는 실시간으로 반영하지 않고 빌드 시점의 데이터를 기반으로 페이지를 생성하기 때문일꺼다. 테스트 내용은 아래와 같다.

- `테스트 github`
  - [rarrit github - rarrit/next-js-study-app/tree/feat/ssg](https://github.com/rarrit/next-js-study-app/tree/feat/ssg)
  - `branch: feat/ssg`
- **첫 번째 방법**
  - fetch에 아무 옵션을 주지 않고 데이터를 불러오는 기본 동작을 했다. 
  - 이는 캐싱 없이 매번 새로 요청을 보내는 방식이다.
- **두 번째 방법**
  - fetch에 force-cache 옵션 적용하여, 데이터를 캐시로부터 불러오도록 설정해봤다. 
  - 이를 통해 변경되지 않은 데이터를 지속적으로 반환하는 것을 확인할 수 있었음.
- **세 번째 방법**
  - package.json, tsx파일을 수정함
- **결과** 
  - 저장 후 새로고침을 해도 동일한 페이지 출력 (수정된 내용이 반영되지 않음)


### :two: SSR 환경 테스트 해보기
`SSR`의 특성상 매 요청마다 서버에서 데이터를 가져와 페이지를 갱신할 수 있음을 확인할 수 있었음. 아래는 테스트 내용은 아래와 같다.

- `테스트 github`
  - [rarrit github - rarrit/next-js-study-app/tree/feat/ssr](https://github.com/rarrit/next-js-study-app/tree/feat/ssr)
  - `branch: feat/ssr`
- **첫 번째 방법**
  - fetch에 no-store 옵션을 적용하여, 서버에 매번 새로운 요청을 보내도록 설정함.
  - 해당 옵션은 캐싱 비활성화 후 각 요청을 서버에서 데이터를 새로 가져오게함
- **두 번째 방법** 
  - page.tsx 컴포넌트에 dynamic 옵션을 추가해서 동적 콘텐츠를 로드하는 방식을 테스트함
  - 매 요청마다 서버에서 렌더링되는 것을 확인할 수 있는지 확인함
- **결과**
  - 요청이 있을 때 마다 지속해서 갱신해 준다. (최신 데이터를 반영해줌)
  - hydration이 완료되기 전까지의 시간. 즉 TTI(Time To Interactive)가 관건임

### :three: ISR 환경 테스트 해보기 
특정 타임에 랜더링이 되도록 하는 `ISR`을 테스트해봤다. `fetch`에서 `next` 옵션을 사용해서 `revalidate: 3` (second) 를 넣어주면된다. 그럼 즉 3초마다 업데이트를 해준다는 것임 테스트 내용은 아래와 같다.

- `테스트 github`
  - [rarrit github - rarrit/next-js-study-app/tree/feat/isr](https://github.com/rarrit/next-js-study-app/tree/feat/isr)
  - `branch: feat/isr`
- **첫 번째 방법**
  - `fetch` 함수에 `next: { revalidate: 3 }` 옵션 추가해줘서 3초마다 데이터를 갱신하도록 설정
  - 새로운 요청시마다 3초 후에 새로운 데이터를 반영해주는지 확인함
- **두 번째 방법**
  - Page.tsx 컴포넌트에 `revalidate` 속성을 추가해서 페이지 자체가 일정 시간 주기로 갱신되도록 설정함 
  - 페이지에서 사용하는 데이터가 정해진 주기마다 업데이트되는걸 확인함
- **결과**
  - 주어진 시간에 한 번씩 갱신해줌 (사용자가 새로고침 안해도 주어진 시간에 한 번씩 최신 데이터 가져와줌)

### :four: CSR 환경 테스트 해보기

- `테스트 github`
  - [rarrit github - rarrit/next-js-study-app/tree/feat/csr](https://github.com/rarrit/next-js-study-app/tree/feat/csr)
  - `branch: feat/csr`

- **첫 번째 방법**
  - `"use client"` 를 최상단에 입력해서 CSR 환경을 설정해줌 
  - 이를 통해 서버가 아닌 클라이언트 측에서 렌더링이 이루어지고, 모든 데이터 요청 및 갱신이 클라이언트에서 처리됨.
  - 참고: [Client Component 사용법 - RARRIT NOTE](https://rarrit.github.io/til/next/nj10-next-server-client-component/)
- **결과**
  - 요청이 있을 때 마다 지속해서 갱신해줌
  - client side rendering이기 때문에, Loading에 관련된 state 제어를 통해 사용자에게 알려줄 수 있음

### :fire: 마무리
`Next.js`의 렌더링 4가지 패턴을 테스트해보면서 렌더링 방식을 유연하게 혼합해 사용할 수 있는 것이 정말 큰 장점인 것 같다. 프로젝트의 요구사항에 맞춰 개발할 때 편리할 듯!