---
title: "[mjt] 엄격 모드(Strict Mode)에 대해 알아보자."
date: 2024-11-28
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - mjt
tags:
  - strict mode
author_profile: true
sidebar_main: true
---

## :ledger: 엄격 모드(Strict Mode)에 대해 알아보자.

자바스크립트에서 `"use strict"`를 한번쯤 본 적이 있다. 그런데 이거 왜 쓰는지 알 지 못했는데(사실 배웠음.. 기억이 안났을 뿐), 스터디 내용에 있어서 공부하게 되었다.

### :one: 언제 쓰는거임?

엄격 모드는 [Javascript Info 문서](https://ko.javascript.info/strict-mode)에서 보았을 때 자바스크립트가 발전하면서 새로운 기능이 추가되다 보니 기존 기능에서 변경된 내용도 생기고 그로인해 호환성 문제가 생겨버렸다.

### :fire: 회고
