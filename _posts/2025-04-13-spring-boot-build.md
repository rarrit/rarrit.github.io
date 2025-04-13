---
title: "[Spring Boot] jar 파일로 패키징하고 실행하기"
date: 2025-04-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "archive"
toc_sticky: true
categories:
  - spring
tags:
  - spring
  - jar
  - build
author_profile: true
sidebar_main: true
---

## :ledger: 스프링 부트 프로젝트 jar 파일로 빌드하고 실행하기

홈 화면과 Hello 페이지 구성을 마쳤다면, 이제 프로젝트를 **jar 파일로 패키징하고 실행**하는 방법을 정리했다.

- Spring Boot에서는 Gradle 또는 Maven을 이용해 프로젝트를 쉽게 빌드할 수 있다.

---

### :one: Gradle을 이용한 jar 빌드

터미널에서 아래의 명령어를 실행하면 된다.

```bash
./gradlew build #macOS/Linux
gradlew build #Windows
```

- 프로젝트 루트 디렉토리에 `build/libs` 폴더가 생기고, `.jar` 파일이 생성된다.
- ex) `hello-spring-0.0.1-SNAPSHOT.jar`
- 빌드할 때 클린 후 하는것이 좋다.


### :two: jar 파일 실행하기

jar 파일은 아래의 명령어로 실행할 수 있다.

- 정상적으로 실행되면, 스프링 부트 내장 톰캣 서버가 뜨고 localhost:8080에서 확인할 수 있다.

```bash
java -jar build/libs/hello-spring-0.0.1-SNAPSHOT.jar
```

- 실행을 종료하고싶으면 터미널에서 `ctrl + c` 입력

#### :pushpin: 2-1) 포트 변경 방법

기본 포트는 8080이지만, 다른 포트를 사용하고 싶다면 실행 시 옵션을 줄 수 있다.

```bash
java -jar build/libs/hello-spring-0.0.1-SNAPSHOT.jar --server.port=9090
```

#### :pushpin: 2-2) application.properties 포트 지정하기

`src/main/resources/application.properties` 파일을 만들어 포트를 미리 지정할 수도 있다.

```properties
server.port=9090
```

### :three: 마무리

간단하게 빌드하는 방법을 알아보았다.<br/>

- 개발 → 빌드 → 실행
- index.html → 정적 페이지
- hello.html → 동적 페이지 + 데이터 전달
