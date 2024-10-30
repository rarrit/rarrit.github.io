---
title: "[4차] 스파르타 최종 프로젝트 - 프로젝트 중 의견 대립 시 소통 방법과 개선점"
date: 2024-10-24
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: [4차] 스파르타 최종 프로젝트 - 의견 대립 소통 방법

벌써 최종 프로젝트를 진행한지 1주일이 끝나가는 상황이다. 오늘 디자이너분들과 회의 중 디자인 요청사항이 있었고, 의견차이가 있었는데 이런 경우 어떻게 대처하면 좋았을지 돌아보기 위해 글을 작성해본다.

### :one: 카카오 소셜 로그인 폼에 인터렉션을 줄 수 있나요?

디자이너분의 질문은 위와 같았다. 디자이너분께서 피그마로 인터렉션을 설명해주셨는데, 먼저 들었던 생각은 인터렉션 자체에 대해 어려움은 없을 것 같지만 카카오 로그인 페이지나 모달(윈도우 팝업)의 경우 외부 URL로 리디렉션되기 때문에 해당 페이지에 접근이 불가하다고 생각이 들었다. 그래서 설명을 들은 후 의견을 제시했는데, 내용은 아래와 같다.

- 카카오 로그인의 경우 외부 URL(화면을 보여주며 설명)로 연결되므로 접근이 불가능하고 그렇기에 수정이 어렵습니다.

### :two: 의견이 다를 경우 대처 방법

위와 같이 답변을 드렸지만, 디자이너분의 입장에서는 이해가 안되었던 부분이 있었던 것 같다. 디자이너분께서는 iframe을 사용하여 카카오 페이지를 불러온 후 인터렉션을 적용하면 되지 않냐고 말씀을 주셨는데, 이 부분에 대해서 명확하게 답변을 드리지 못했다.

#### :pushpin: 2-1) 잘못한점

디자이너분이 iframe으로 구현할 수 있지 않냐는 추가 질문에 명확하게 답변을 하지 못했다. 이유는 뭔가 가능할 거 같으면서도 그렇게 구현하는게 맞나? 라는 스스로 확실하게 대답할 수 없었던 것. 이러한 상황에서 해당 내용에 대해 구현이 가능한지, 방법이 있는지 찾자는 말 보다 "일반적인 웹사이트에서 그렇게 구현된 사이트는 보지 못한 것 같다." 와 "레퍼런스가 있는지 보여달라" 라고 말했다. 어떻게 보면 이러한 대답과 요청이 나쁘다고 생각하지 못했지만 디자이너 질문에 근본적인 해답은 아니었던 것 같다. 그렇기에 더 의견이 좁혀지지 못했던 느낌이다.

#### :pushpin: 2-2) 잘한점

회의가 길어지며, 의견은 좁혀지지 않았고 튜터님께 찾아갔다. 질문의 답은 내가 생각한 부분과 일치했고 디자이너분도 수긍하게 되었다. 의견이 좁혀지지가 않고 길어질 때 상급자를 찾아 가는 것이 좋은 방법이 아닐까 싶다.

### :fire: 회고

- `개선점`: 의견 대립이 있을 때, 추측보다는 구현 가능성을 확실히 조사하여 명확한 답변을 드리는 것이 중요하다는 점을 배웠다.
- `의견이 좁혀지지 않을 경우`: 시간을 효율적으로 사용하기 위해 튜터님이나 상급자의 의견을 구하는 것이 도움이 된다.