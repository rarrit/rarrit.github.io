---
title: "[Spring-boot] Execution failed for task ':processResources'. 빌드 문제 해결 방법"
date: 2025-04-15
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - spring
tags:
  - spring 
  - build error
author_profile: true
sidebar_main: true
---

## :ledger: Execution failed for task ':processResources'. 빌드 문제 해결 방법
작업 중 intellij에서 갑자기 아래와 같은 에러가 발생했다. 

![gradle-build-cache-error01](https://github.com/user-attachments/assets/9478b722-7c67-4b4b-8f85-c23ce75bd975)


### :one: 원인
갑작스럽게 발생한 에러였지만, 조사해보니 자주 발생하는 `Gradle` 빌드 오류라는 것을 알게 되었다.

- `Execution failed for task ':processResources' + Failed to clean up stale outputs` 이 메시지는 빌드 중간에 리소스 관련 캐시나 임시 파일 정리가 제대로 안 돼서 생기는 문제 
- 보통 Gradle 캐시/빌드 디렉토리 문제로 알 수 있음

### :two: 해결 방법
해결 방법은 아래와 같다.

#### :pushpin: 2-1) 캐시 삭제하기 (터미널)

```bash
./gradlew clean # 빌드 캐시 삭제
```

만약 캐시를 삭제해도 문제가 해결되지 않으면 **강제 빌드 캐시 삭제**를 해본다.

```bash
./gradlew clean build --refresh-dependencies # 강제 빌드 캐시 삭제
```

#### :pushpin: 2-2) intellij 에서 클린 후 빌드
만약 터미널이 익숙하지 않다면 **intellij** 우측 Gradle(코끼리 아이콘)을 클릭해서 `build/clean`, `build/build`를 순차적으로 클릭한다.

![gradle-build-cache-error02](https://github.com/user-attachments/assets/d91856a4-11a1-4d9c-9c84-0144630bba8e)


#### :pushpin: 2-3) build 디렉토리 삭제
만약 캐시를 삭제해도 문제가 해결되지 않으면, 수동으로 `build` 디렉토리를 삭제하는 방법이 있다.

1. intellij 에서 삭제
2. 터미널로 삭제

```bash
rm -rf build/ # 빌드 디렉토리 삭제
```

### :three: 마무리
intellij에서 작업하다 `Execution failed for task ':processResources'.` 빌드 에러가 나면 아래와 같이 대처하자.

1. 캐시 삭제 후 빌드
2. 빌드 디렉토리 삭제 후 빌드

