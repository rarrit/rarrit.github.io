---
title: "[TailWind] text ... 2줄 처리 하는 방법"
date: 2024-10-04
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - css
tags:
  - tailwind
author_profile: true
sidebar_main: true
---

## :ledger: tailwind를 사용해서 text ... 처리하는 방법을 알아보았다.
이번 Next.js 프로젝트를 진행하며 tailwind를 사용했고, description이 정말 긴 경우 좌측의 info의 영역이 원치 않게 넓어져서 공백이 너무 많이 보였다. 공백을 없애주고 원하는 레이아웃을 적용하기 위해 description을 ... 처리하는 방법을 알아보았다. 처음 적용된 레이아웃은 아래와 같다.

![clamp-before](https://github.com/user-attachments/assets/c4b014a1-b15c-4b83-86ef-74c4bc8369e7)

### :one: global.css 설정
... 처리는 이후에 여러 컴포넌트에서 사용할 수 있을 것 같아서 재사용이 가능하도록 global.css의 utilities에 값을 설정해주었다.

```css
@layer utilities {
  .line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }  
}
```

### :two: 사용 예시
... 처리를 할 태그에 `line-clamp-2` 클래스를 적용해주었다.

```tsx
<div className="text-[13px] text-center text-[rgba(199,199,199,1)] line-clamp-2">
  {spell.description}
</div>
```

### :three: 적용 화면
아래와 같이 잘 적용된것을 확인할 수 있다.

![clamp-after](https://github.com/user-attachments/assets/a1530fe8-db51-41f0-9a1d-8149a3d5b9dc)


### :fire: 마무리
디자인상 이쁘게 보이려고, ...처리를 했으나 사용자가 원하는 정보를 얻지 못하는 이슈가 있었다. 이럴 경우 어떻게 처리할까 고민 중, 타이틀 옆에 <u>(마우스를 올리면 상세 정보를 확인할 수 있습니다.)</u> 문구를 적용해주었고, 마우스 호버시 전체 정보를 확인할 수 있도록 적용해주었다.

![clamp-hover](https://github.com/user-attachments/assets/ae9af02d-6e8d-4f79-a73d-42cd14f7b141)