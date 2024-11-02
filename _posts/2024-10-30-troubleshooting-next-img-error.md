---
title: "[Next.js] 외부 이미지 로드 오류 해결하기"
date: 2024-10-30
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - runtime error
author_profile: true
sidebar_main: true
---

## :ledger: Next.js에서 외부 이미지 로드 오류 해결하기

Next.js 프로젝트에서 외부 이미지를 `next/image` 컴포넌트를 사용하여 로드할 때 다음과 같은 오류가 발생할 수 있다. 이전에도 같은 이슈가 있었고 해결했는데, 이번에 작업하면서 또 마주쳤을 때 기억이 안나서 글을 작성해봤다.

![error image](https://github.com/user-attachments/assets/75453a23-5038-4330-a858-c918e86e8e9d)

### :one: 원인

이 오류는 `next/image`가 외부 이미지 소스를 로드할 때, 해당 소스의 호스트가 허용되지 않는 경우 발생한다. 기본적으로 Next.js는 보안상의 이유로 외부 호스트에서 이미지를 로드하기 전에 해당 호스트를 명시적으로 허용해야 한다.

### :two: 해결 방법

이 문제를 해결하기 위해서는 `next.config.js` 파일에서 허용할 호스트를 설정해줘야함.

#### :pushpin: 2-1) `next.config.js` 파일 열기

프로젝트 루트 디렉토리에서 `next.config.js` 파일을 찾아준다.

#### :pushpin: 2-2) 호스트 추가하기

`images` 설정에 `domains` 배열을 추가하여 외부 이미지를 로드할 호스트를 지정한다. 예시는 아래와 같다.

```javascript
// next.config.js
const nextConfig = {
  images: {
    domains: ["gocamping.or.kr"], // 허용할 도메인 추가
  },
};

module.exports = nextConfig;
```

### :three: 해결이 안된다면?

설정을 변경한 후 에러가 해결되지 않으면 서버를 재시작해보면 수정된 것을 확인할 수 있다.

### :fire: 마무리

Next.js에서 외부 호스트의 이미지를 로드하기 위해서는 해당 호스트를 next.config.js의 images.domains 배열에 추가해야한다.
