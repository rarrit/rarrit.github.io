---
title: "깃(Git) 사용법 및 용어정리"
date: 2024-06-28
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

## :ledger: Git 사용법과 용어정리!

### :one: Git 사용법

1. **사용자 정보 설정**<br/>
깃을 처음 사용할 때 어떤 사람이 사용하는지 알 수 있게 사용자를 등록하도록 한다.

    ```bash
    # 사용자 정보 설정
    git config --global user.name "이름 입력"
    git config --global user.email "이메일 입력"

    # 계정이 잘 들어갔는지 확인하는 방법
    git config --list

    # 계정 삭제 방법
    git config --unset --global user.name
    git config --unset --global user.email
    ```

2. **저장소 초기화**<br/>
git 에서 파일을 감시한다. 해당 폴더에서 .git 폴더가 보이지 않으면 [보기>표시>숨은파일] 체크해주면 보인다.
   
    ```bash
    git init
    ```

3. **원격 저장소 연결**<br/>
연결할 원격 저장소 URL 을 입력한다.
     
    ```bash
    git remote add origin 원격 저장소 URL # 원격 저장소 연결
    git remote rm origin 원격 저장소 URL  # 원격 저장소 삭제
    git remote -v                         # 연결된 원격 저장소 확인
    ```

4. **파일 추가 및 커밋**<br/>
파일작업 후 변경된 내용을 확인하고 싶을 땐 `git status` 를 입력하여 확인 후 진행한다.
     
    ```bash
    git add 파일명                # 파일 추가
    git add 파일명1 파일명2        # 특정 파일 추가
    git add .                     # 모든 변경 파일 추가
    git commit -m "커밋 메시지"   # 커밋
    ```

5. **브랜치 생성 및 전환**
    ```bash
    git branch 브랜치명       # 브랜치 생성
    git checkout 브랜치명    
    ```

6. **원격 저장소와의 동기화**
    ```bash
    git pull origin 브랜치명   # 원격 저장소의 변경 내용 가져오기
    git push origin 브랜치명  # 로컬 변경 내용 원격 저장소에 반영
    ```

7. **브랜치 병합**
    ```bash
    git switch 기준 브랜치    # 기준 브랜치로 전환
    git merge 병합할 브랜치  # 병합
    ```

### :two: 유용한 Git 명령어

- **로그 확인**: `git log`
- **상태 확인**: `git status`
- **파일 비교**: `git difftool 파일명`
- **Stash 적용**: `git stash apply`
- **Stash 목록 확인**: `git stash list`
- **Stash 삭제**: `git stash drop`
   

### :three: Git의 용어 정리

1. **Repository (저장소)**: 프로젝트의 파일들과 변경 이력들을 저장하는 곳이며, 로컬 저장소와 원격 저장소로 나뉜다.
   - **로컬 저장소**: 자신의 컴퓨터에 저장된 저장소
   - **원격 저장소**: 네트워크 서버에 저장된 저장소 (예: GitHub, GitLab)

2. **Clone**: 원격 저장소의 전체 내용을 로컬 저장소로 복사한다. `git clone URL`

3. **Commit**: 파일의 변경 사항을 저장소에 기록한다. 각 커밋은 고유한 해시 값을 가지며, 변경 이력 추적에 사용됨. `git commit -m "메시지"`

4. **Branch (브랜치)**: 독립적으로 작업을 진행할 수 있는 가지를 의미한다. 기본 브랜치는 `main` 또는 `master` 이며, 새로운 기능 개발이나 버그 수정을 위해 브랜치를 생성한다. `git branch 브랜치명`

5. **Merge**: 두 브랜치를 하나로 합치는 작업. `git merge [브랜치명]`

6. **Checkout * switch**: 특정 브랜치나 커밋으로 작업 디렉토리를 전환한다. `git checkout 브랜치명`, `git switch 브랜치명`

7. **Pull**: 원격 저장소의 변경 내용을 로컬 저장소로 가져온다. `git pull`

8. **Push**: 로컬 저장소의 변경 내용을 원격 저장소로 업로드한다. `git push`

9. **Stash**: 현재 작업 중인 변경 사항을 임시로 저장하고, 작업 디렉토리를 깨끗하게 만든다. `git stash`

10. **Fetch**: 원격 저장소의 변경 사항을 로컬 저장소로 가져오지만, 로컬 작업 디렉토리에 적용하지는 않는다. `git fetch`

### :fire: 마무리

Git은 협업과 버전 관리를 효율적으로 할 수 있는 강력한 도구이다. 
주로 혼자서 사용해봤는데, 사용하면서 업데이트 해볼 예정!

더 많은 정보를 원한다면 Git의 [공식 문서](https://git-scm.com/doc)를 참고하세요.

