---
title: "[2차] 스파르타 최종 프로젝트 - 기능,코드컨벤션,깃헙룰 정의"
date: 2024-10-21
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

## :ledger: [2차] 스파르타 최종 프로젝트 - 기능,코드컨벤션,깃헙룰 정의

2차에서는 기능, 코드컨벤션, 깃룰을 정의했다.

### :one: 기능정의서 작성

지금까지 팀,개인 과제를 하면서 기한이 짧다보니 기능은 러프하게 정의했던 것이 있었고, 이번 최종 팀 프로젝트에서는 `디자이너와 협업`을 하기 때문에 개발팀에서 어떤 기능을 사용할 것이고 페이지마다 어떻게 구성되어야 하는지 명확하게 전달할 필요가 있었다. 사용한 관리 툴은 `google sheet`와 `trello`를 사용했고, 아래와 같이 작성했다.

- 각 페이지를 한 눈에 볼 수 있게, 뎁스별로 구성함
- 페이지마다 어떤 컨텐츠가 들어가야 하는지 상세히 작성
- 해당 페이지에서 들어가는 기능 또한 상세히 작성

해당 내용을 논의할 때 너무 고마웠던 것은 디자이너 분들이 같이 참여하고 싶다고 하셔서, 컨텐츠 구성하는 것에 있어서 도움이 정말 많이 되었다. 또한 튜터님들도 시트를 확인해서 피드백을 주셨던게 기능을 정의함에 있어 정말 수월해졌다.

### :two: Code Convention

코드 컨벤션 또한 지금까지 다양한 팀과 개인적으로 사용했던 것 중 좋았던 것과 새롭게 사용하고 싶은 것들을 논의하고 적용했는데, 이번 프로젝트를 통해 나는 개인적으로 `pnpm`, `sentry`, `lunch.json`, `husky`, `pr template`를 사용해 볼 수 있게되어 걱정도 있지만 설레는 마음이 컸다.

- `pnpm`
  - 빠르고 효율적인 패키지 매니저로, 특히 디스크 공간을 절약하고 의존성 설치 속도가 빠르다.
  - 동일한 의존성을 사용하는 프로젝트 간에 중복된 파일을 저장하지 않고 공유 폴더를 사용해 설치 공간을 최적화하기 때문에, 대규모 프로젝트에서 유리하다.
- `Sentry`
  - 에러 추적 및 성능 모니터링 도구입니다. 주로 웹 애플리케이션이나 모바일 앱에서 발생하는 오류를 실시간으로 추적하고 보고하는 데 사용함
- `lunch.json`
  - 주로 프로젝트의 환경 설정이나 데이터 초기화를 자동화하기 위한 스크립트 구성 파일로 사용됨.
  - 특정 환경에서 필요한 초기 작업이나 설정을 명시하여 개발자들이 일관된 환경에서 작업할 수 있도록 도와줌.
- `Husky`
  - Git **훅(Hook)**을 관리하는 도구로, 커밋 전 검사나 푸시 전 테스트 같은 자동화된 작업을 설정할 수 있음.
  - 이를 통해 코드 퀄리티를 높이고, 코드가 Git에 푸시되기 전에 린트, 테스트 등의 필수 작업이 자동으로 수행되도록 하여 실수를 방지함.
- `PR 템플릿 (Pull Request Template)`
  - 팀 내에서 일관된 PR 작성을 유도하여 코드 리뷰 과정을 개선하기 위해 사용함.
  - PR 작성 시 필요한 정보나 체크리스트를 미리 제공함으로써 커뮤니케이션 효율성을 높이고, 코드 리뷰자가 더 쉽게 리뷰할 수 있도록 도와줌.

<details>
<summary>Code Convention 상세 내용</summary>

1. Prettier

```jsx
  {
    "printWidth": 120, // 한 줄의 최대 너비를 120자로 설정
    "tabWidth": 2, // 탭의 너비를 2칸으로 설정
    "useTabs": false, // 탭 대신 스페이스를 사용
    "semi": true, // 문장의 끝에 세미콜론을 자동으로 추가
    "singleQuote": false, // 작은 따옴표 대신 큰 따옴표를 사용
    "bracketSpacing": true, // 중괄호와 객체 리터럴에서 괄호 사이에 공백 추가 { a: 1 }
    "trailingComma": "none", // 마지막 항목 뒤에 쉼표를 추가하지 않음
    "jsxBracketSameLine": true, // JSX 요소의 닫는 태그를 같은 줄에 위치
    "plugins": ["prettier-plugin-tailwindcss"] // Tailwind CSS 클래스 정렬을 위한 Prettier 플러그인 추가
  }
```

2. 타입 정의
   1. only interface 사용
   2. props 일 경우 type
3. 상태관리
   1. 클라이언트: zustand
   2. 서버: tanstack query
4. 디렉터리 구조
   - 구조 정의
     1. 페이지 구조
        1. (pages) : 페이지 디렉토리
           1. 예시: (pages)/(mypage)
           2. 예시: (pages)/(community)
     2. 컴포넌트 구조: 아래와 같이 적용한 이유는 이전에는 \_components에서 모든 컴포넌트를 관리했었는데, 팀 회의 하면서 각 페이지 폴더 안에 컴포넌트가 있을 때 확실히 컴포넌트의 사용성을 알 수 있음
        1. \_components: 공통 컴포넌트 디렉토리
           1. 예시1) \_components/Button.tsx
        2. /page/component: 페이지 내 컴포넌트
           1. 예시2) mypage/DashBoard.tsx
     3. 공통 apiKey 관리
        1. api/apiKey : env 환경 변수 설정 세팅
     4. 서버액션 관리
        1. 공통: utils/공통서버액션명.ts (유저면 유저Action)
        2. 페이지: 페이지명/페이지Actions.ts
     5. 타입 관리
        1. 공통: types/Champing.ts
        2. 페이지내에서 관리: page명/TypeName.ts
     6. 에러 관리
        1. app/global-error.tsx : csr 에서만 나옴
        2. app/error.tsx: gloabal-error props로 전달받으면 ssr 에서도 나옴
     7. 로딩 관리
        1. app/loading.tsx
5. Tailwind
   1. utilities 생성 할 경우 팀원에게 공유해주기
      1. 초기 프로젝스 세팅 후 공통 스타일 정의
      2. 추가할 경우 주석으로 어느 페이지or컴포넌트에서 사용하는지 명시
6. CASE
   1. 변수, 함수: Camel Case
   2. 상수: UPPER_SNAKE_CASE
   3. 이미지명: cabab-case (예: img-01)
   4. 함수→props: on함수명 (예: onSubmit)
7. 주석
   1. 함수 만들 때 간단하게 어떤 용도의 함수인지 주석 달아주기
   2. 함수 주석 사용 /\*\* \*/
      1. 이렇게 사용할 경우 호출하는 함수에서 마우스를 올릴 때 주석 설명이 보임
8. console.log
   1. git push 할 때 최대한 지우는걸 지향함
9. 렌더링 방식
   1. 메인: ISR (캠핑장 데이터는 크게 변동될 일이 없음)
   2. 일반 페이지: SSR (SEO)
   3. 외부 url 접근 필요 없을 경우: CSR (마이페이지)
   4. 가이드 페이지: SSG (데이터 변동 없음)
10. sentry
    1. https://teawon.github.io/react/Sentry/
11. pnpm 사용
    1. 사용예시: yarn add react-router-dom ⇒ pnpm i react-router-dom
12. lunch.json

```jsx
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    },
    {
      "name": "Next.js: debug full stack",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev",
      "serverReadyAction": {
        "pattern": "- Local:.+(https?://.+)",
        "uriFormat": "%s",
        "action": "debugWithChrome"
      }
    }
  ]
}
```

</details>

### :three: Github Rules

깃헙룰도 이번에는 `approve`를 반드시 사용하여, 이전 팀 프로젝트와 다르게 반드시 코드리뷰를 하기로 이야기가 정해졌다. 브랜치는 작은 기능 단위로 시작하기로 했고, 커밋 규칙과, pull 규칙을 반드시 지키기로했다.

- `template(템플릿) 사용`
- `approve(승인) 규칙 사용`
  - 최소 한명 코멘트나 리뷰를 받아야 merge 하기
- `브랜치 규칙`
  - (dev, qa, live) 배포할 때 문제가 있을 수가 있어서, 미리 선작업하는게 좋음
  - 배포가 안되는 이유는 빌드가 깨지는 상황에서 이루어지는데, 그러한 상황을 dev환경에서 머지하여 안정적으로 라이브 환경에 배포할 수 있음 이러한 경험을 할 수 있다.
  - 예시: 메인 만들고 dev분기 → 깃허브 푸시 → 브랜치 2개 → 이 2개로 배포
  - 공통 기능 컴포넌트: feat/calender, feat/chat, feat/search (겹칠 경우)
  - 페이지 컴포넌트: page/commu
- `commit 규칙`
  - 커밋은 최대한 기능단위로, 함수단위로 작성. 왜냐하면 깃은 협업 툴이기도 하지만 형상관리(버전관리) 목적도 있기 때문
  - 커밋규칙
    | 작업 타입 | 작업내용 |
    | ----------- | ----------------------------------------------- |
    | ✨ docs | readme, 문서 수정할 때 사용 |
    | 🎉 add | 초기 세팅,파일 생성 및 기능 추가 |
    | ♻️ refactor | 코드 리팩토링 |
    | 🩹 fix | 코드 수정 |
    | 🔥 del | 기능/파일을 삭제 |
    | 🍻 test | 테스트 코드를 작성 (배포할 때, 그럴때 사용하자) |
    | 💄 style | css |
- pull 규칙
  - pull을 받을 때 원격 브랜치와 로컬 브랜치를 서로 같은 브랜치로 맞추어주고, 다른 브랜치로 넘어가 merge를 시킨다.
    - ex: dev(origin) -> feat/main(local) (x)
    - dev(origin) -> dev(local) -> feat/main(local) (o)

### :fire: 회고

- 이번 최종 프로젝트를 진행하면서 기능을 상세하게 정의하는 과정은 특히 어려웠다. 초반에는 빠뜨린 기능이 없을지 고민했으며, 팀원들과 의견을 나누면서 기능이 추가되기도 했다. 하지만 이렇게 초기 단계에서 기능을 명확하게 정의하다 보니, 개발 도중 발생할 수 있는 이슈들을 상당 부분 미리 파악할 수 있었고, 덕분에 개발 과정에서 이슈를 최소화할 수 있다고 생각이 들었다.
- 코드 컨벤션과 깃헙(GitHub) 룰을 설정하는 것도 지금까지는 일관되게 지키지 못한 부분도 많았지만, 이번 프로젝트를 통해 깃헙에서 사용하는 반드시 지켜야 하는 규칙들을 알 수 있게되었고 중요성 또한 다시한번 알게되었다.
- 비록 과정 중에 어려움도 많았지만, 기능을 상세히 정의하고 개발 전반의 규칙을 정립하면서 프로젝트의 완성도를 높일 수 있다고 생각한다. 앞으로도 이러한 경험을 바탕으로 더 나은 협업 환경을 만들고, 개인적으로도 프로세스를 개선하는 노력을 지속해야겠다.
