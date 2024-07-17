---
title: "깃(Git) Restore 사용법"
date: 2024-06-30
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - git
tags:
  - git
author_profile: true
sidebar_main: true
---

## :ledger: 깃(Git) 의 유용한 명령어 Restore!

### :one: Git Restore 이란?

`git restore`는 `git switch`와 같이 Git의 새로운 명령어 중 하나로, 작업 디렉토리의 파일 변경사항을 되돌리거나 스테이징 영역에서 파일을 제거하는 데 사용된다.<br/>
이 명령어는 기존의 git checkout과 git reset 명령어의 일부 기능(특정 커밋으로 변경)을 대체하며, 보다 명확한 사용 목적을 가지고 있다.

### :two: Git Restore 사용법

#### :pushpin: 2-1) 작업 디렉토리의 변경사항 되돌리기
작업 중인 파일의 변경사항을 되돌리려면 다음과 같이 `git restore` 명령어를 사용할 수 있다.

```bash
git restore 파일명
git restore example.txt // 예시
```

이 명령어는 example.txt 파일의 변경사항을 되돌리고, 파일을 마지막 커밋 상태로 되돌린다.

#### :pushpin: 2-2) 스테이징 영역에서 파일 제거하기
스테이징 영역에 추가된 파일을 스테이징에서 제거하려면 --staged 옵션을 사용하면된다.

```bash
git restore --staged 파일명
git restore --staged example.txt // 예시
```
이 명령어는 example.txt 파일을 스테이징 영역에서 제거하고, 파일의 변경사항을 유지한다.


#### :pushpin: 2-3) 특정 커밋으로 되돌리기
특정 커밋의 상태로 파일을 되돌리려면 `--source` 옵션을 사용하면된다.

```bash
git restore --source=커밋 해시 파일명
git restore --source=aaabb111 example.txt // 예
```

이 명령어는 `example.txt` 파일을 `aaa111` 커밋의 상태로 되돌립니다.


#### :pushpin: 2-4) 디렉토리 변경사항 전체 되돌리기
디렉토리 전체의 변경사항을 되돌리려면 디렉토리명을 명시한다.

```bash
git restore 디렉토리명
git restore src/ // 예시
```

이 명령어는 `src` 디렉토리 내의 모든 파일의 변경사항을 되돌린다.

### :exclamation: 주의사항
- git restore는 작업 중인 파일의 변경사항을 되돌리기 때문에, 되돌리기 전에 중요한 변경사항은 커밋하거나 별도로 저장해 두는 것이 좋다.
- 이 명령어는 Git 2.23.0 이상 버전에서 사용 가능하며, Git 버전을 확인하려면 `git --version` 명령어를 사용해서 확인한다.

## :fire: 마무리
`git restore` 명령어는 파일의 변경사항을 쉽게 되돌리고, 스테이징 영역에서 파일을 제거하는 등 작업 디렉토리와 스테이징 영역을 효율적으로 관리할 수 있는 도구이다.<br/>
기존의 `git checkout`과 `git reset` 명령어의 일부 기능을 보다 명확하게 분리하여 사용하기 쉽게 만든 명령어로 자주 사용할 수 있도록 하자!

