---
title: "[CSS] React, Next 에서 className에 조건부 클래스를 적용하는 법"
date: 2024-10-25
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - css
  - react
  - next
tags:
  - css
author_profile: true
sidebar_main: true
---

## :ledger: [CSS] React, Next 에서 className에 조건부 클래스를 적용하는 방법을 알아보자!

스파르타 마지막 최종 프로젝트에서 같은 팀원이 `className={cn('', {'': props})}`를 사용하는 것을 보았다. 그동안 props 값을 사용해서 `${props ? props : null}`로 값을 적용해줬는데 새로운 방법을 알게되어 정리해보았다.

### :one: 단일 조건부 스타일링 방법

단일 조건을 사용해 클래스를 적용하는 방법은 아래와 같다.

```tsx
import cn from "clsx";

function MyComponent({ isActive }) {
  return (
    <div className={cn("base-class", { "active-class": isActive })}>
      Hello World
    </div>
  );
}
```

- `isActive`가 `true`일 경우, className은 `"base-class active-class"`로 적용된다.
- `isActive`가 `false`일 경우, `"base-class"`만 적용된다.

`clsx`는 객체 형태로 조건을 받아서 `true`인 key(className)만 적용해 주기 때문에 `isActive` 값에 따라 자동으로 클래스를 추가/제거한다.

### :two: 여러 조건을 적용하려면?

만약 `props`가 여러개라면? 각 조건에 맞는 클래스를 적용해야 할 경우에도 `clsx`를 사용할 수 있다. 각 클래스 이름을 `props` 값에 따라 조건부로 추가할 수 있기 때문에 복잡한 조건도 쉽게 처리할 수 있다. 방법은 아래와 같다.

```tsx
mport cn from 'clsx';

function MyComponent({ isActive, isDisabled, isPrimary }) {
  return (
    <button
      className={cn(
        'base-class', // 기본으로 항상 적용되는 클래스
        {
          'active-class': isActive, // `isActive`가 true일 때만 적용
          'disabled-class': isDisabled, // `isDisabled`가 true일 때만 적용
          'primary-class': isPrimary // `isPrimary`가 true일 때만 적용
        }
      )}
    >
      Button
    </button>
  );
}
```

위 코드에서는 다음과 같은 조건들이 설정된다.

- `isActive`가 `true`라면 className에 `"active-class"`가 추가된다.
- `isDisabled`가 `true`라면 `"disabled-class"`가 추가된다.
- `isPrimary`가 `true`라면 `"primary-class"`가 추가된다.

### :three: clsx의 장점

- `가독성`: 코드가 깔끔해지며, 조건부 클래스를 관리하기가 쉬워진다.
- `유연성`: 여러 조건을 간단히 처리할 수 있으며, false, null, undefined 등은 자동으로 무시되어 불필요한 클래스가 추가되지 않는다.
- `재사용성`: 복잡한 조건부 스타일링이 필요한 경우에도 재사용하기 쉽다.

### :fire: 마무리

현재 react, next 프로젝트를 진행하면서 `tailwind`를 정말 많이 사용하는데, className에 정말 많은 값이 들어간다. 조건부 스타일링 방법을 사용해서 코드도 줄일 수 있고 불필요한 `null, undefined, false`를 작성하지 않아도 된다는게 정말 편리한 것 같다.
