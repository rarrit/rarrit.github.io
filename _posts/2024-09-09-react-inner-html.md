---
title: "React에서 템플릿 리터럴 br 적용 (dangerouslySetInnerHtml)"
date: 2024-09-09
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - react
  - troubleshooting
tags:
  - react innerhtml
  - dangerouslySetInnerHtml
author_profile: true
sidebar_main: true
---

## :ledger: 리액트에서 innerHTML을 어떻게 쓸까?

이번 개인과제 진행 중 템플릿 리터럴로 작성한 문법에서 `<br/>` 태그가 적용이 안되는 현상을 경험했다. 하지만 나에겐 구글형이 있기에 그다지 두렵지 않았다는 것! 리액트에서 `innerHTML`을 어떻게 사용하는지 알아보자!

### :one: dangerouslySetInnerHTML 이란?

명칭보소.. 일단 정말 길고 이름부터 `dangerously` 위험하고 알려주고있다. 코드에서 HTML을 설정하는 것이 위험에서 쉽게 노출될 수 있어서 그렇다는데, 이름부터 "응 위험한 거임~"이라고 알려주고 있다. 새롭게 생긴 문법 중 `setHTMLUnsafe`을 [유튜브-제로초](https://www.youtube.com/shorts/rYtl-kVPWwk) 보다 알게 되었는데, 궁금하신 분은 영상 보시길!

### :two: dangerouslySetInnerHTML 사용 방법

문법은 아래와 같다.

```jsx
// 태그 안에 아무것도 들어가지 않고 아래와 같이 작성하면 됌
// 마크다운 문법으로 작성하니 {{ _html: }} 이 부분이 사라진다.
// <p dangerouslySetInnerHTML={{ __html: }}/>
<p dangerouslySetInnerHTML={{ __html: }}/>
```

### :three: dangerouslySetInnerHTML 적용 예시

개인 과제에서 적용한 예시이다.

```jsx
// mbtiResult.js
export const mbtiResult = (mbti) => {
  switch (mbti) {
    case "ESTP" :
      return `
        (*istp랑 좀 비슷한 듯!!댓글도 estp인데 istp랑 비슷하다 뭐 그런댓글 많이 보임 둘이 좀 호환되는거같아)<br/><br/>
        외로움 오지게 탐<br/><br/>
        손재주 좋아서 취미가 베이킹, 뜨개질, 인형만들기<br/><br/>
        리더쉽 있음 조별활동 조장 혹은 반장 도맡아 함<br/><br/>
        좀 관종<br/><br/>
        표현을 아끼지 않음<br/><br/>
        어른들이 좋아함<br/><br/>
        밖에서 사람 만나는거 개좋아하는데 게을러서 나가기가까지가 싫음<br/><br/>
        하고싶은거 다 해야됨 못하면 혼자서 부들부들하다가 곧 까먹음<br/><br/>
        걍 대충살고 눈치도 안봄<br/><br/>
        스트레스도 잘 안받음 근데 그렇게 적극적인 편도 아님 걍 사는대로 삼<br/><br/>
        공감능력 조금 부족<br/><br/>
        남한테 관심 없고 생각하는것도 귀찮음<br/><br/>
        모임에서 어느새 내가 분위기 주도하고 있음(정신차리고 보면 내가 다 역할 정해주고 조장하고 있음)<br/><br/>
        근자감 쩔<br/><br/>
      `;
    case ....:
    default: ...
  }
}
// TestResultItem
<p className="descText minSans" dangerouslySetInnerHTML={{__html: mbtiResult(data.result)}} />

```

### :four: dangerouslySetInnerHTML 위험성

HTML 삽입하는 방법이 XSS공격에 취약하다는 것에 대해 잘 몰랐는데, 구글링 중 직접 공격을 해보면서 알아보는 글이 있어서 흥미롭게 보았다. 해당 글에서는 `dangerouslySetInnerHTML` 이 얼마나 위험한지 경험을 토대로 작성되어있는데, innerHTML을 안전하게 하는 방법 또한 찾아봐야 겟다.

- [XSS 공격을 직접 해보면서 알아가기 - 민동준](https://dj-min43.medium.com/xss-%EA%B3%B5%EA%B2%A9%EC%9D%84-%EC%A7%81%EC%A0%91-%ED%95%B4%EB%B3%B4%EB%A9%B4%EC%84%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-c2c1d9baf7ec)

### :fire: 마무리

이름부터 위험해보이는 `dangerouslySetInnerHTML`를 사용해서 문제를 해결했지만 찜찜하다. 현재 과제 제출 기한이 급박해서 적용했지만 추후 다른 방법이 어떤게 있는지 찾아봐야겠다.
