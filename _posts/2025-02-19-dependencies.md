---
title: "dependencies vs devDependencies 차이를 알아보자"
date: 2025-02-19
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - development
  - til
tags:
  - package.json
  - dependencies
  - devdependencies
author_profile: true
sidebar_main: true
---

## :ledger: dependencies vs devDependencies 차이를 알아보자

리액트 프로젝트를 진행하면서 다양한 라이브러리를 사용해봤을 것이다. 그런데 `package.json`을 보면 `dependencies`와 `devDependencies` 두 가지가 존재하는데, 이 둘의 차이는 무엇일까?

### :one: dependencies란?

`dependencies`는 프로젝트가 실제로 동작하는 데 필요한 라이브러리를 의미한다. 즉, 프로덕션 환경에서도 필수적인 패키지들이다.

#### :pushpin: 1-1) 예시

- `react`와 `react-dom`은 리액트 애플리케이션이 실행되기 위해 필수적으로 필요하다.
- `axios`는 API 요청을 보낼 때 사용되므로 런타임에서 필요하다.

```javascript
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "axios": "^1.5.0"
  }
}
```

### :two: devDependencies란?

`devDependencies`는 개발 과정에서만 필요한 라이브러리들을 의미한다. 즉, 코드가 빌드되거나 테스트될 때 사용되지만, 프로덕션에서는 필요하지 않다.

#### :pushpin: 2-1) 예시

- `eslint`: 코드 스타일을 체크하는 도구로, 실행 중에는 필요하지 않다.
- `jest`: 테스트를 위한 라이브러리로, 프로덕션에서는 사용되지 않는다.
- `webpack`: 번들링을 도와주는 도구로, 개발 중에는 필요하지만 배포 후에는 사용되지 않는다.

```javascript
{
  "devDependencies": {
    "eslint": "^8.40.0",
    "jest": "^29.6.0",
    "webpack": "^5.88.0"
  }
}
```

### :three: dependencies vs devDependencies 차이 정리

| `비교 항목`       | `dependencies`                 | `devDependencies`     |
| ----------------- | ------------------------------ | --------------------- |
| 용도              | 실제 애플리케이션 실행 시 필요 | 개발 및 빌드 도구     |
| 예제 패키지       | React, Axios                   | Jest, Webpack, ESLint |
| 배포 시 포함 여부 | 포함됨                         | 포함되지 않음         |

#### :four: 설치 방법 차이

패키지 설치할 때 기본적으로 `dependencies`에 추가된다.

```bash
yarn add axios  # dependencies에 추가됨
```

개발용 패키지는 -D 또는 --dev 옵션을 사용하여 devDependencies에 추가할 수 있다.

```bash
yarn add -D eslint  # devDependencies에 추가됨
```

### :fire: 정리

- 프로덕션에서 필요한 라이브러리는 `dependencies`에 추가해야 한다.
- 개발 과정에서만 필요한 라이브러리는 `devDependencies`에 추가해야 한다.

실제로 프로젝트를 진행할 때, 배포 환경과 개발 환경을 구분하여 필요한 패키지만 포함되도록 하면 불필요한 용량 증가를 방지할 수 있다고 한다.
