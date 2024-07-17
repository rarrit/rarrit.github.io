---
title: "깃(Git) Checkout과 Switch의 차이점"
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

## :ledger: Checkout & Switch 차이

Git을 사용할 때 브랜치를 전환하거나 파일을 복원하는 작업은 매우 빈번하게 발생한다.<br/>
이 작업을 수행할 때 주로 사용되는 명령어가 `git checkout`이었지만, Git 2.23.0 버전부터는 `git switch`와 `git restore`라는 새로운 명령어가 도입되었다. <br/>
`git checkout`과 `git switch`의 차이점에 대해 알아보자!

### :one: Git Checkout

`git checkout` 명령어는 Git에서 매우 다재다능하게 사용되는 명령어다.<br/>
이 명령어는 두 가지 주요 기능을 가지고 있다.

1. **브랜치 전환**: 현재 작업 중인 브랜치를 다른 브랜치로 전환

    ```bash
    git checkout 브랜치명
    ```

2. **파일 복원**: 특정 커밋 또는 HEAD의 상태로 파일을 복원

    ```bash
    git checkout commit -- file-path
    ```

그러나, `git checkout`의 다기능성 때문에 사용자가 명령어의 의도를 명확히 파악하기 어려운 경우가 많다.<br/>
이러한 혼란을 줄이기 위해 Git 2.23.0 버전부터는 `git switch`와 `git restore`가 도입되었다.

### :two: Git Switch

`git switch` 명령어는 브랜치를 전환하는 기능에만 집중한다.<br/>
`git checkout`이 하는 여러 작업 중에서 브랜치 전환 기능을 분리하여 보다 명확하게 사용할 수 있다.

#### :pushpin: 2-1) 사용법

1. **브랜치 전환**:

    ```bash
    git switch 브랜치명
    ```

2. **새로운 브랜치 생성 후 전환**:

    ```bash
    git switch -c 새로운브랜치명
    ```

3. **브랜치를 특정 커밋에서 생성 후 전환**:

    ```bash
    git switch -c 새로운브랜치명 commit
    ```

### :three: 차이점 요약

| 기능                        | git checkout                 | git switch                    |
|-----------------------------|------------------------------|-------------------------------|
| 브랜치 전환                 | `git checkout 브랜치명`      | `git switch 브랜치명`         |
| 새로운 브랜치 생성 후 전환  | `git checkout -b 브랜치명`   | `git switch -c 브랜치명`      |
| 파일 복원                   | `git checkout commit -- file` | N/A                           |

### :fire: 마무리

`git switch` 명령어는 `git checkout`에서 브랜치 전환 기능을 분리하여 보다 직관적이고 명확하게 사용할 수 있도록 도와주며 새로운 Git 사용자뿐만 아니라 숙련된 사용자에게도 `git switch`를 사용하는 것이 더 명확하고 오류를 줄일 수 있는 방법이 될 수 있다.

기존의 `git checkout` 명령어는 여전히 사용 가능하지만, Git의 최신 기능을 활용하기 위해서는 `git switch`와 `git restore`를 사용하는 것이 좋다고 생각한다.


