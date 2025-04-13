---
title: "[spring] Intellij 실행버튼 안나올 때 적용 방법 (김영한 - 스프링 입문)"
date: 2025-04-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - spring  
tags:
  - spring
  - intellij
author_profile: true
sidebar_main: true
---

## :ledger: [spring] Intellij 실행버튼 안나올 때 적용 방법 (김영한 - 스프링 입문)
스프링 강의를 보며 따라하던 중 `작명Application.class`파일에서 실행(재생)버튼이 활성화가 되지 않아서 난감했다. 인프런 댓글, 구글링 결과 많은 사람이 겪은 문제로 여러 글을 참고하여 적용해봤으나 실패했고 `gpt`를 통해 활성화 버튼을 노출시켰으며 방법은 아래와 같다.

### :one: build.gradle 파일 수정
루트의 `build.gradle`파일의 `repositories`를 찾고 `gradlePluginPortal()`을 추가한다.

```java
repositories {
  mavenCentral() // 원래 있음
  gradlePluginPortal() // 추가
}
```

### :two: 캐시 클리어 및 빌드
`gradlePluginPortal()`을 추가했으면, 터미널에서 아래의 명령어를 순차적으로 입력한다.

```bash
./gradlew clean # 캐시 클리어
./gradlew build # 다시 빌드
```

### :three: 참고
아마 실행버튼이 생기기 전에 `IntelliJ IDEA > 설정 > Gradle` 쪽에 `Gradle 프로젝트`영역이 없었는데 위와 같이 적용 후 생긴것을 확인할 수 있다.

![intellij - setting](https://github.com/user-attachments/assets/2eb2c3c0-ba62-4394-b9f2-a6060b42a576)