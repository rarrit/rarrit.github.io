---
title: "tailwind CSS npx tailwindcss init -p 에러 해결 방법"
date: 2024-02-12
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - troubleshooting
  - til
tags:
  - tailwind css
author_profile: true
sidebar_main: true
---

## :ledger: tailwind CSS npx tailwindcss init -p 에러 해결 방법

테일윈드 버전대에 따른 에러 해결 과정입니다.

### :one: 개요

리액트로 tailwind CSS를 Install 하는 과정에서 `npx tailwindcss init -p` 명령어를 쳤을 때 아래와 같은 에러가 노출되었다.

![error-image](https://github.com/user-attachments/assets/d4651098-17bd-4263-8865-b43f8c941f7e)

이전까지 잘 되었던 것이 않되니까 뭔가 설정이 잘못되었나 싶었고, 로그를 봐도 사실 뭔가 뭔지 모르겠었다. 로그는 아래와 같았다.

![log-image](https://github.com/user-attachments/assets/f864f743-0e73-4e2a-a0f7-65b1b2dc4a36)

### :two: 시도한 방법

첫 번째로 gpt에 물어보고 tailwind가 있는지 확인하고, node, npm 버전 등 확인해 보았는데 문제는 해결되지 않았다. 구글에 검색을 해도 모두 `npx tailwindcss init -p` 명령어 입력 후 나와 같은 문제는 찾지 못했고 **리액트 테일윈드 설정** 에서 **tailwindcss npx error**로 검색해서 아래의 내용을 확인해보았다.

![search-image](https://github.com/user-attachments/assets/166335ca-4235-45dc-8f65-db8ba337661a)

### :three: 해결 방법

위의 이미지의 ![링크](https://www.threads.net/@fdaytalk/post/DFVToEbtWIq/how-to-fix-npx-tailwindcss-init-error-in-tailwind-v4-solvedif-youve-recently-tri?hl=ko)로 들어가면 **tailwind v4**에서 일어난 오류를 수정하는 방법을 알려주는데 테일윈드 4버전에서 생긴 오류로 ![fadytalk.com - 해결방법](https://l.threads.net/?u=http%3A%2F%2Fwww.fdaytalk.com%2Fnpx-tailwindcss-init-npm-error-could-not-determine-executable-to-run%2F&e=AT3Yn9aWwgLpjfdu8Izi6UJ6shgzj_qPAFwBORfR3dTF1NxMoAs6C97ISaJxBOXSFA7mOr9gaL3Vhae0AAfyZ6HBZ46DLUK4L2-3HT5rUYmQmjOhV-DHI0olj5Nd9qjn)링크에서 해결 방법을 확인해서 수정해주었다.

1. tailwind 4버전에서는 따로 적용해야 되는 것이 있음
2. 3버전으로 인스톨하니 정상적으로 해결됨

### :fire: 마무리

`tailwind` v4버전을 사용하게 된다면 해당 링크를 참고해서 적용해주면 될 것 같다.
