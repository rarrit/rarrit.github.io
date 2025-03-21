---
title: "[3차] 스파르타 최종 프로젝트 - 기능 세부 정의, 프로젝트 세팅, 기능 분배"
date: 2024-10-22
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

## :ledger: [3차] 스파르타 최종 프로젝트 - 기능 세부 정의, 프로젝트 세팅, 기능 분배

3차에서는 2차에서 정한 기능을 조금더 세밀하게 정의 및 분배하고 프로젝트 세팅 및 버셀에 배포하는 과정을 진행했다.

### :one: 기능 세부 정의

재사용 가능한 기능 및 컴포넌트를 정리하고, 개발 과정에서 발생할 수 있는 이슈와 추가해야 할 사항들을 더 세부적으로 확인했다. 시간이 정말 오래 걸리긴 했지만, 각자 개발을 진행하는 단계에서 미리 발생할 수 있는 문제를 미리 제거한다고 생각하며 진행했다. 무엇보다 이번 회의를 통해 다음 회의 시간을 단축시킬 수 있다는 점에서 만족스러웠다.

### :two: 프로젝트 세팅

최종 프로젝트는 `Next.js`를 선택하게 되었다. 서비스를 운영할 목표로 생각하고 진행하게 되었을 때 클라이언트 관점으로 생각하게 되어 기능도 조금 더 안정적으로 구현할 수 있는 방법을 알아볼 수 있었고, 무엇보다 SEO에서 강점인 이유에서 선택하게 되었다. 이후에 코드 컨벤션을 통해 사용하기로 한 `pnpm`, `lunch.json`, `Husky`, `Sentry` 그리고 상태 관리 라이브러리인 `Zustand` 와 `tanstack query` 및 `supabase`, `prettier`등 세팅을 했는데, 이러한 과정들에서 여러 시행착오가 많았다. 어려움이 있었지만 화면공유하고 같이 문제를 해결해나가는 과정 또한 좋게 생각하고있다.

### :three: 기능 분배

기능을 분배하는 과정에서는 그래도 정말 빨랐다. 아무래도 기능을 정의하는 과정에서 힘을 많이 쏟게되다 보니 분배하는 과정에서는 각자 어떤 부분을 하고싶은지 의견을 내서 쉽게 결정할 수 있었다.

### :fire: 회고

3차 과정까지 기능 기획과 역할 분배를 마쳤는데, 회의 시간이 길어져서 힘들긴 했다. 하지만 초반에 기능을 세밀하게 정의한 덕분에, 개발 단계에서 훨씬 수월하게 진행할 수 있을 거란 생각이 든다.
