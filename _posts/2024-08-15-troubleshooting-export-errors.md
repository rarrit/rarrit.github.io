---
title: "export default와 export const의 차이점 및 오류 해결 과정"
date: 2024-08-14
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:  
  - troubleshooting
tags:
  - export default
  - export const
author_profile: true
sidebar_main: true
---

## :ledger: export default 와 export const 
리액트 CRUD를 구현하며 컴포넌트를 import 하는 과정에서 겪었던 에러 해결 과정을 정리해보겠다.

### :one: 개요
JavaScript의 `export default`와 `export const`는 모듈을 내보낼 때 사용되는 두 가지 주요 방법인데 이 차이점을 모르고 `export const`를 사용했다가 발생했던 에러이다.
 
### :two: 코드 설명,에러,해결 과정
#### :pushpin: 2-1) 코드 설명
아래는 리엑트에서 컴포넌트로 jsx파일을 생성할 때 기본적으로 생성되는 코드이다. Test 함수와 아래에 `export default 함수명`으로 이 컴포넌트의 리턴되는 값을 다른 jsx파일에서 import 후 사용할 수 있다.

```javascript
import React from 'react'
const Test = () => {
  return (
    <div>
      
    </div>
  )
}
export default Test
```

#### :pushpin: 2-2) 에러 설명
그런데 이번에 다른 사람들의 코드를 참고하던 중 `export const 함수명` 을 보았는데, 그냥 문법의 차이구나 하고 넘기고 `export const Test`로 작성하고 상위 컴포넌트에서 import를 했을 때 `SyntaxError`가 콘솔에 찍혔던 것이었다.<br/>
![export error](https://github.com/user-attachments/assets/01b59f64-6d28-456a-a84d-c51f12f299f3)
번역하자면 요청한 모듈 Test.jsx가 `default`라는 내보내기를 제공하지 않는다고 하는데, 엥? 난 export 해주었는데 뭐가 문제인지 잘 이해를 못했던 것..!

#### :pushpin: 2-3) 해결 과정
해결은 콘솔을 c+c 구글에 c+v 했더니 나와 같은 에러를 겪는 사람들이 있었고 export default로 import할 때와 export const로 import할 때가 다르단 걸 알게되었다. `export const 함수명`으로 할 때 반드시 중괄호를 사용해야 하다는 걸 알게되고 이에 맞춰 적용해주었더니 오류가 해결이 되었다.
```javascript
// 에러가 노출되었던 코드
import Test from '../components/Test';

// 상위 컴포넌트
import { Test } from '../components/Test';
```

### :three: export default와 export const의 차이점
차이점을 아래의 예제를 통해 정리해보았다.

#### :pushpin: 3-1) export default 설명 및 특징
```javascript
// Input 컴포넌트
const Input = ({ value, type, name }) => {
  return (
    <>
      <label>{name}</label>
      <input value={value} type={type}>
    </input>
    </>
  )
}
export default Input;

// Input을 import하는 컴포넌트
import Input from './Input';
// or
import myPowerInput from './Input';
```
- ***설명***
  - `export default`는 모듈에서 하나의 주요 컴포넌트나 함수를 내보낼 때 사용함
  - 기본적으로 내보낸 (export) 모듈은 임이의 이름으로 import 할 수 있음 => 예시 코드의 Input or myPowerInput
- ***특징***
  - `export default`는 <u>모듈에서 하나만 존재</u>할 수 있음
  - Import 할 때 <u>중괄호 `{}`를 사용하지 않음</u>
  - 모듈의 기본값을 export 할 때 유용하게 사용함

#### :pushpin: 3-2) export const 설명 및 특징
```javascript
// Input 컴포넌트
export const Input = ({ value, type, name }) => {
  return (
    <>
      <label>{name}</label>
      <input value={value} type={type}>
    </input>
    </>
  )
}

// Input을 import하는 컴포넌트
import { Input } from './Input';
// import { 내마음대로 작명 } from '/.Input'; 은 불가능!
```
- ***설명***
  - `export const`는 이름이 지정된 내보내기를 할 때 사용함
  - 여러개의 변수,함수,또는 컴포넌트를 한 모듈에서 내보낼 수 있음
- ***특징***
  - 한 파일에서 <u>여러 개의 export</u>가 가능함
  - Import 할 때 <u>반드시 중괄호 `{}`를 사용하여 이름을 정확히 맞춰</u>야 함
  - Import 할 때 <u>이름을 변경할 수 없음</u>

### :four: 주요 차이점 요약
- `export default` 는 모듈의 기본값을 내보내며, Import 할 때 <u>임이의 이름을 사용할 수 있으며</u> 중괄호`{}`를 사용하지 않음
- `export const` 는 이름을 지정한 내보내기 방식이며, import 할 때 중괄호`{}`를 사용하고, 같은 이름으로만 Import 할 수 있음 

### :fire: 마무리
export default와 export const의 특징과 차이점에 대해 알아보았다. 구글링해서 몇 분내로 이해하고 코드를 수정했지만 글을 작성하면서 다시 한번 각각의 특징과 차이점을 이해할 수 있는 좋은 시간이었다. 앞으로는 특징에 맞게 적절한 export 방식을 선택하여 코딩을 진행해보겠다!