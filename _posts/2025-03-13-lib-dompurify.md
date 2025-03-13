---
title: "[React] 안전하게 dangerouslySetInnerHTML 사용하기 (DOMPurify)"
date: 2025-03-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
  - lib
tags:
  - dangerouslySetInnerHTML
  - XSS
  - DOMPurify
author_profile: true
---

## :ledger: 안전하게 dangerouslySetInnerHTML 사용하기 (DOMPurify)

자바스크립트에서 `innerHTML`을 사용하면 HTML 문자열을 직접 삽입할 수 있지만, 리액트에서는 `dangerouslySetInnerHTML`을 사용해야 한다. 그러나 이름에서 알 수 있듯이 **크로스 사이트 스크립팅(XSS)** 공격에 취약하여 주의가 필요하다. 이를 방지하기 위해 `DOMPurify` 라이브러리를 활용하는 방법을 알아보자.

### :one: dangerouslySetInnerHTML

`React`에서 `HTML` 문자열을 렌더링하려면 `dangerouslySetInnerHTML` 속성을 사용해야 한다. 그러나 이 방식은 보안적으로 위험할 수 있다.

- 아래의 예시에서는 `script`가 실행될 위험이 있으며, 사용자 입력이 포함된 경우 악성 스크립트가 실행될 가능성이 있다.

```javascript
// dangerouslySetInnerHTML 예시
const data = "<script>alert('XSS 공격!')</script><p>안전한 HTML?</p>";

function DangerousComponent() {
  return <div dangerouslySetInnerHTML={{ __html: data }} />;
}
```

#### :pushpin: 1-1) 크로스 사이트 스크립팅(XSS)

`XSS(크로스 사이트 스크립팅)` 공격은 웹 애플리케이션에서 사용자 입력을 필터링하지 않고 그대로 렌더링할 때 발생한다. 공격자가 악성 스크립트를 삽입하면 브라우저에서 실행될 수 있어 보안 취약점이 발생할 수 있다.

예를 들어, 댓글 입력 필드에서 `<script>alert('해킹됨!')</script>` 같은 내용을 입력하면 그대로 실행될 수 있다.

### :two: DOMPurify 사용법

위에서 설명했듯이 `dangerouslySetInnerHTML`을 사용하면 **XSS**공격에 취약하고 이를 방지하려면 `DOMPurify` 라이브러리를 사용하여 HTML 문자열을 정제해야한다. 설치 및 사용방법은 아래와 같다.

#### :pushpin: 2-1) 설치방법

```bash
# npm
npm install dompurify

# yarn
yarn add dompurify
```

#### :pushpin: 2-2) 적용방법

```javascript
import DOMPurify from "dompurify";

const data = "<script>alert('XSS 공격!')</script><p>안전한 HTML?</p>";
const sanitizedHtml = DOMPurify.sanitize(data);

function SafeComponent() {
  return <div dangerouslySetInnerHTML={{ __html: sanitizedHtml }} />;
}
```

### :fire: 요약

- `dangerouslySetInnerHTML`을 사용할 때 XSS 공격에 주의해야 한다.
- `DOMPurify`를 사용하여 `HTML`을 정제하면 안전하게 렌더링할 수 있다.
- `sanitize()` 함수를 활용하여 사용자 입력을 필터링해야 한다.

#### :pushpin: 참고

- [LogRocket - Using dangerouslySetInnerHTML in a React application](https://blog.logrocket.com/using-dangerouslysetinnerhtml-react-application/)
- [dompurify - npm](https://www.npmjs.com/package/dompurify)
