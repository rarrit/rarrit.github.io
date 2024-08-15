---
title: "리액트(react.js) Props란 무엇인가?"
date: 2024-08-15
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - til
tags:
  - react
  - props
author_profile: true
sidebar_main: true
---


## :ledger: 리액트의 Props에 대해 알아보자
리액트를 배우면서 쉽게 생각했다가 어떻게 사용하느냐에 따라 어렵게 느껴졌던 `Props`다.. `Props`는 리액트에서 컴포넌트 간에 데이터를 전달하는 중요한 개념으로 까먹지 않기 위해 정리해보았다. (쓰면 미세하게라도 기억이 남아서 다시 돌아오게 되더라구요..!)

### :one: Props란 무엇인가?
Props는 "Properties"의 줄임말로, 컴포넌트에 데이터를 전달하기 위해 사용되는 <u>입력값</u>이라고 생각하면 된다. 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때 사용되며, 자식 컴포넌트는 전달받은 props를 이용해서 자신을 랜더링한다. 확실히 이해해야될 부분은 Props는 읽기 전용으로 자식 컴포넌트에서 변경할 수 없고 오직 부모(상위)컴포넌트에서만 props를 전달할 수 있다.

### :two: Props와 자바스크립트의 매개변수
처음 props를 이해할 때 자바스크립트의 매개변수와 정말 비슷하구나라고 생각했었고 어떤 차이점이 있는지 알아보았다. 그 중 가장 큰 차이점이라고 느꼈던 것은 매개변수는 함수내에서 조작이 가능하지만, props를 전달받은 컴포넌트에서 props를 조작할 수 없다는것! 

#### :pushpin: 2-1) 비교
아래의 테이블을 통해 어떤 점이 다른지 비교해보자.

| 특징	| 자바스크립트 매개변수 |	리액트 Props |
| 데이터 전달 방식 |	함수 호출 시 인수로 전달 |	부모 컴포넌트에서 자식 컴포넌트로 전달 |
| 데이터의 변경 가능 여부 |	함수 내부에서 변경 가능 |	<u>읽기 전용, 변경 불가</u> |
| 데이터 구조	| 단일 값 또는 여러 값 (배열, 객체 등) |	객체 형태로 전달 |
| 주요 목적 |	함수의 동작을 결정하기 위한 데이터 전달 |	컴포넌트의 렌더링 결과를 결정하기 위한 데이터 전달 |
| 디폴트 값 |	디폴트 매개변수를 지정할 수 있음	| 기본값 설정 가능 (props를 받지 않았을 때) |

#### :pushpin: 2-2) 결론
공통적으로 데이터를 전달하는 것이고 다른점은 사용되는 환경과 목적인 것 같다.
- 자바스크립트의 매개변수는 ***일반적으린 함수 호출***에서 사용되는 데이터 전달 방법으로, 함수 내부 동작을 유연하게 만들기 위해 사용함
- 리액트의 ***props***는 <u>컴포넌트 간에 데이터를 전달</u>하고, 컴포넌트의 랜더링을 동적으로 제어하는 데 사용되며 컴포넌트를 재사용 가능하게 만들어주고 부모-자식 관계에서 컴포넌트가 서로 데이터를 주고 받을 수 있게 해준다.

### :three: Props를 사용하는 이유
Props는 리액트에서 컴포넌트를 재사용 가능하게 만들고, 컴포넌트 간에 데이터를 전달하는 역할을 한다. 이 덕분에, 하나의 컴포넌트를 다양한 상황에서 재사용할 수 있게 해줌. 예를들면 같은 버튼 컴포넌트를 여러 곳에서 사용하지만, 버튼에 표시되는 텍스트,색상 등 다른 부분이 있을 수 있는데 이러한 부분을 Props를 통해 상황에 따라 다르게 표시할 수 있다는 것! (최고최고)

### :four: Props를 사용하는 방법
#### :pushpin: 4-1) Props 전달
부모 컴포넌트에서 자식 컴포넌트로 Props를 전달하는 방법은 쏘이지 하다. 자식 컴포넌트를 사용할 때 HTML의 속성 key-value 형식으로 값을 전달하면 된다.
1. 문자열(String)
  - 문자열을 `props`로 전달할 때는, 속성 값을 `따옴표('')`로 감싸서 전달함
  ```jsx
  function Test(){
    return <TestComponent name="응애 애기 Props">
  }
  ```
2. 숫자(Number) 및 불리언(Boolean)
  - 숫자나 불리언 값을 `props`로 전달할 때는 `중괄호{}`를 사용함. 중괄호 안에 자바스크립트 표현식이 들어갈 수 있음
  ```jsx
  function Test(){
    return <TestComponent age={20} man={false} >
  }
  ```
3. 객체(Object)
  - 객체를 `props`로 전달할 때는, 마찬가지로 `중괄호{}`를 사용하여 객체를 전달함
  ```jsx
  function Test(){
    const infoData = {
      name: 'rarrit',
      age : 30,
    };
    return <TestComponent infoData={infoData} >
  }
  ```
4. 함수(Function)
  - 버튼이나 인풋에서 자주 사용되는 Click, Change 함수를 `props`로 전달할 수 있음.
  ```jsx
  function Test(){
    const sayHello = () => {
      alert("어서오고~");
    };
    return <TestComponent Func={sayHello} >
  }
  ```
5. JSX 요소
  - 처음 봤고 잘 사용하지 않을 것 같지만.. JSX도 `props`로 JSX요소를 전달할 수 있음
  ```jsx
  function Test(){
    return <TestComponent text={<p>hello hello~</p>} >
  }
  ```
6. Spread 연산자
  - 한 번에 여러 개의 `props`를 객체 형태로 전달할 때 ***Spread 연산자 (...)***를 사용함
  ```jsx
  function Test(){
    const infoData = {
      name: 'rarrit',
      age : 30,
    };
    return <TestComponent {...infoData} >
  }
  ```

#### :pushpin: 4-2) Props 받기
- 자식 컴포넌트에서는 함수의 매개변수로 props를 받아 사용할 수 있다. 
- `props`는 객체 형태로 전달되며, 이 객체 안에 부모 컴포넌트에서 전달한 값들이 있음 고로 아래의 `ChildComponent`의 name은 rarrit, age는 30이 된다.
```jsx
// 부모 컴포넌트
function ParentComponent(){
  return <ChildComponent name='rarrit' age={30}/>
}

// 자식 컴포넌트
function ChildComponent(props) {
  return (
    <>
      <p>Name: {props.name}</p>
      <p>Age : {props.age}</p>
    </>
  )
}
```

#### :pushpin: 4-3) 구조 분해 할당
구조 분해 할당을 사용하면 `props` 객체에서 특정 값을 쉽게 추출할 수 있으며 가독성이 좋아짐
```jsx

// 부모 컴포넌트
function ParentComponent(){
  return <ChildComponent name='rarrit' age={30}/>
}

// 자식 컴포넌트
function ChildComponent({ name, age }) {
  return (
    <>
      <p>Name: {props.name}</p>
      <p>Age : {props.age}</p>
    </>
  )
}
```

### :five: Props의 활용
props를 사용하면서 다양하게 활용하는 법도 알게되어서 정리해본다.
#### :pushpin: 5-1) 기본값 설정
컴포넌트가 특정 props를 받지 못했을 경우 기본값을 설정할 수 있다. 이를 통해 오류 방지가능!
```jsx
function Test({ name = 'rarrit'}) {
  return <h1>Hello, {name}</h1>;
}
```

#### :pushpin: 5-2) 조건부 랜더링
요번 과제에서 테이블에 값이 없을 경우 "등록된 국가가 없습니다."를 노출해봤었다. 리액트에서는 삼항연산자를 통해 사용한다.
```jsx
function Test({ items }){
  return (
      {
        items.length === 0 
        ? (<p>등록된 국가가 없습니다.</p>) 
        : (로직 수행)
      }   
  )
}
```

#### :pushpin: 5-3) PropTypes
튜터님의 코드를 보다 알게된 `PropTypes`! 컴포넌트가 받을 Props의 타입을 정의하고, 올바른 타입의 props가 전달되었는지 검증할 수 있게 해준다. 협업할 때 코드를 만든사람의 의도를 파악하여 실수를 줄이고, 안전성을 높일 수 있음!
- `isRequired` : 반드시 전달해야한다고 명시함
```jsx
import PropTypes from 'prop-types';

function ChildComponent({ name, age }) {
  return (
    <div>
      <p>Name : {name}</p>
      <p>Age  : {age}</p>
    </div>
  );
}

// PropTypes
ChildComponent.propTypes = {
  name: PropTypes.string.isRequired, // 문자열로만!
  age: PropTypes.number.isRequired, // 숫자여야함!
};
```

#### :pushpin: 5-4) 컴포넌트와 Props가 많아지면?
컴포넌트의 트리가 깊어질수록 Props를 전달하는 과정이 복잡해질 수 있다고하는데 이것을 `props drilling`이라고 하며, 이 문제를 해결하기 위해 `컨텍스트 API`를 사용하거나 `상태 관리 라이브러리(예: Redux)`를 도입한다고한다. 아직 배우지 못했지만 왜 배우는지는 이해할 수 있을 것 같다.

### :fire: 마무리
리액트로 CRUD 만들어보기를 하면서 정말 많이 사용했던 Component와 Props에 대해 오늘 하루 공부를 해보았는데, 알던 부분은 조금 확실히 이해할 수 있었고 몰랐던 부분에 대해서도 알 수 있는 시간이었다. 리액트를 공부하면서 가장 처음에 배웠던 것들이고 기본적인 것이라 생각이 들지만 어떻게 활용하느냐에 따라 효율성은 극과 극이라 생각이 든다. (나와 튜터의 코드를 비교 후..) 앞으로도 파이팅 해야겠다.