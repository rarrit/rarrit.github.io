---
title: "[TS] 타입 오류를 체크하는 방법 (npx tsc --noEmit)"
date: 2024-10-29
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - ts
tags:
  - typescript
  - npx tsc --noEmit
author_profile: true
sidebar_main: true
---

## :ledger: [TS] 타입 오류를 체크하는 방법 (npx tsc --noEmit)

vercel에 배포할 때 ESLint를 통해 발생하기도 하고 이번 프로젝트에서 사용하는 Husky를 통해 `any`타입 사용을 제한하게 되었을 때와 또는 작업 중 빠드린 경우 에러가 노출되는데 `push`하기 전 체크하는 방법이 있다. 방법은 `npx tsc --noEmit`을 사용하면되는데, 무엇인지 알아보자.

### :one: npx tsc -noEmit 이란?

`npx tsc --noEmit` 명령어는 TypeScript 컴파일러를 사용하여 타입 검사만 수행하고, 실제로 JavaScript 파일을 생성하지 않는 옵션이다. 즉, 코드의 타입 오류를 확인할 수 있지만, 출력 파일은 생성하지 않는다.

### :two: 사용이유

TypeScript는 JavaScript에 타입을 추가하여 보다 안전한 코드를 작성할 수 있도록 도와주는 프로그래밍 언어이며, TypeScript를 사용하는 과정에서 컴파일러인 `tsc`를 자주 사용하게 된다. 그 중 `npx tsc --noEmit` 명령어는 특정한 상황에서 매우 유용하게 활용함.

### :three: 사용법

터미널을 열고 아래의 명령어를 입력한다.

```bash
npx tsc --noEmit
```

명령어를 입력하면 터미널에 해당 오류메세지가 발생되고, 오류가 없다면 아무런 메시지도 출력하지 않는다.

### :fire: 마무리

`npx tsc --noEmit` 명령어는 TypeScript 프로젝트에서 타입 오류를 빠르게 체크할 수 있게 해주며, 이 옵션을 활용함으로써, 정의한 타입이 잘 적용되었는지 확인하거나 타입 오류를 빠르게 체크할 수 있어 정말 유용하게 사용할 수 있다.
