---
title: "[Spring Boot] 기본 페이지 구성하기 - 홈화면과 Hello 페이지 연결"
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
author_profile: true
sidebar_main: true
---

## :ledger: 스프링 부트로 간단한 페이지 구성해보기

스프링 부트(Spring Boot) 프로젝트에서 홈화면을 구성하고, 간단한 데이터 전달을 통해 Hello 페이지를 만들어보는 과정을 정리했다.

### :one: 홈화면 추가 (index.html)

먼저 정적 리소스를 관리하는 `resources/static` 디렉토리에 `index.html` 파일을 추가해준다.

- `static` 디렉토리에 위치한 파일은 별도의 컨트롤러 없이도 바로 접근이 가능하다.
- ex) localhost:8080/index.html

```html
<!-- resources/static/index.html -->
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>홈</title>
  </head>
  <body>
    <h1>Welcome to My Spring Boot App!</h1>
    <p><a href="/hello">Hello 페이지로 이동</a></p>
  </body>
</html>
```

### :two: HelloController 생성 및 매핑

컨트롤러를 생성하여 `/hello` 경로로 접속 시 `Hello` 페이지를 보여주도록 설정한다.

- 여기서 `Model` 객체를 통해 데이터를 뷰로 전달하고, 템플릿 이름인 "hello"를 리턴하면 `resources/templates/hello.html`을 찾아 렌더링하게 된다.

```java
package hello.hello_spring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

// 1. @Controller: spring 에서는 @Controller 어노테이션을 적용해줘야한다.
// 2. @GetMapping: web application 에서 /hello 라고 들어오면 이 메서드를 호출한다.

@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "Hello ~ SPRING");
        return "hello";
    }
}

```

### :three: 템플릿 파일 추가 (hello.html)

**타임리프(Thymeleaf) 템플릿 엔진**을 사용하여 `resources/templates`에 `hello.html`을 생성한다.

- `${data}`는 컨트롤러에서 전달한 데이터를 화면에 출력하는 부분이다.
- 템플릿 파일은 반드시 `templates` 폴더 안에 위치해야 하며, `.html` 확장자를 사용해야 한다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <title>hello page</title>
  </head>
  <body>
    <p th:text="'hello, ' + ${data}">헬로</p>
  </body>
</html>
```

### :four: 마무리

스프링 부트 프로젝트에서 정적 페이지(index.html)와 동적 페이지(hello.html)를 구성하고, 컨트롤러를 통해 데이터를 전달하는 기본적인 흐름을 살펴보았다. 간단한 예제지만 실제 웹 애플리케이션 개발의 핵심적인 구조인

- 요청 → 컨트롤러 → 모델 데이터 전달 → 템플릿 렌더링

이라는 과정을 직접 구현해보며 아주아주 조금 친해진듯 싶다.