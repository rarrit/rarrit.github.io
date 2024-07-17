---
title: "배열의 생성, 접근, 수정, 추가, 삭제 방법"
date: 2024-07-05
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - js
tags:
  - array
author_profile: true
sidebar_main: true
---

## :ledger: 배열의 생성, 접근, 수정, 추가, 삭제 방법

### :one: 베열의 접근 및 수정

#### :pushpin: 1-1) 배열의 생성 방법
배열의 생성 방법은 이전 글에 작성했듯이, 아래의 3가지가 있다.
   - 배열 리터럴
   - 배열 생성자
   - 빈배열

```javascript
// 배열 리터럴
let literalArray = ['kimm', 'min', 'kyu'], // 배열 리터럴
    constructorArray = new Array(1994,1), // 배열 생성자
    emptyArray = []; // 빈 배열
```

#### :pushpin: 1-2) 배열의 접근 방법
배열의 특정 요소에 접근하려면 배열의 이름과 대괄호[] 안에 인덱스를 사용하면 된다.

```javascript
let literalArray = ['kimm', 'min', 'kyu'];

// 인덱스 0번째 요소에 접근
console.log(literalArray[0]); // kimm

// 인덱스 1번째 요소에 접근
console.log(literalArray[1]); // min

// 인덱스 2번째 요소에 접근
console.log(literalArray[2]); // kyu
```

#### :pushpin: 1-3) 배열 수정 방법
배열의 요소를 수정하려면 해당 배열 접근 후 변경할 인덱스에 새로운 값을 할당하면 된다.

```javascript
let literalArray = ['kimm', 'min', 'kyu'];

// 인덱스 1번째 요소 변경 kimm -> song
literalArray[0] = 'song';

// 인덱스 2번째 요소 변경 min -> ji
literalArray[1] = 'ji';

// 인덱스 3번째 요소 변경 kyu -> yong
literalArray[2] = 'yong';

console.log(literalArray); // ['song', 'ji', 'young']
```

#### :pushpin: 1-4) 배열의 길이
배열의 길이는 `length` 속성을 사용해서 배열에 포함된 요소의 갯수를 확인할 수 있다.

```javascript
let literalArray = ['kimm', 'min', 'kyu'];

console.log(literalArray.length); // 3
```

### :two: 배열 추가 방법
배열을 추가하는 방법은 `push()`메서드와 `unshift()`메서드가 있다.

   - `push()` : 배열의 끝에 요소를 추가함
   - `unshift()` : 배열의 앞에 요소를 추가함

#### :pushpin: 2-1) `push()`메서드 사용 예시
`push()`메서드는 배열의 끝에 요소를 추가하는데 사용되며 주로 동적으로 데이터를 저장할 때 사용되는 것 같다.<br/>
예) '회원가입 버튼 클릭' -> '회원 DB에 정보 저장'

```javascript
let pushMethod = ['today', 'i', 'learned']
pushMethod.push("success");
console.log('02-1. push 메서드 사용 :', pushMethod); // ['today', 'i', 'learned', 'success']
```

#### :pushpin: 2-2) `unshift()` 메서드 사용 예시
`unshift()`메서드는 `push()`와 반대로 배열의 맨 앞에 요소를 추가하는데 사용되며 반대로 생각해보면 최근 게시물이나 채팅에 사용 될 것 같다.
예) '게시글 등록' -> '게시판 리스트 가장 첫 상단에 노출'

```javascript
let unshiftMethod = ['월','화','수','목','금','토'];
unshiftMethod.unshift('일');
console.log('02-2. unshift 메서드 사용해서 배옆 앞에 다중 요소를 추가 :', unshiftMethod); // ['일','월','화','수','목','금','토']
```

### :three: 배열을 여러개 추가하고 싶다면? `apply()`메소드와 `spread` 연산자를 사용하자.
#### :pushpin: 3-1) `apply()`메소드 
배열의 추가 메소드를 공부하면서 문득 여러개를 추가하려면 어떤 방법이 있을까 해서 알게 되었다.<br/>
추가뿐만 아니라 병합할 때도 사용할 수 있어서 배열을 합칠 때 유용히 사용될 것 같다.

   - `push()`, `unshift()` 메소드 둘 다 동일하게 사용이 가능하다.


```javascript
// 배열 뒤쪽에 요소를 여러개 추가
let pushApplyMethod = ['a','b','c']
pushApplyMethod.push('d'); // ['a','b','c','d']
pushApplyMethod.push.apply(pushApplyMethod, ['e', 'f']);
console.log('02-1-1. push + apply 메서드 사용 :', pushApplyMethod); // ['a','b','c','d','e','f']

// 배열 앞쪽에 요소를 여러개 추가
let unshiftApplyMethod = ['a', 'b', 'c', 'd'];
unshiftApplyMethod.unshift('b'); // ['b', 'a', 'b', 'c', 'd']
unshiftApplyMethod.unshift.apply(unshiftApplyMethod, ['d', 'c']);
console.log('02-2-1. unshift + apply 메서드 사용해서 배열 앞에 다중 요소를 추가 :', unshiftApplyMethod);

// 뒤쪽의 배열 합치기
let pushApplyMethod01 = ['a','b','c'],
    pushApplyMethod02 = ['d','e','f'];
Array.prototype.push.apply(pushApplyMethod01, pushApplyMethod02); // 기존 데이터 뒤쪽에 새 데이터를 추가
console.log(pushApplyMethod01) // ['a','b','c','d,'e','f']

// 앞쪽의 배열 합치기
let unshiftApplyMethod01 = [4,5,6],
    unshiftApplyMethod02 = [1,2,3];
Array.prototype.push.apply(unshiftApplyMethod01, unshiftApplyMethod02); //  기존 데이터 앞쪽에 새 데이터를 추가
console.log(unshiftApplyMethod01) //  [1,2,3,4,5,6]
```

#### :pushpin: 3-2) `spread` 연산자
`spread` 연산자는 `apply()`메소드에 비해 문법이 더 간결하며, 가독성이 좋다. (쓰임은 비슷하다)

```javascript
// 배열 뒤쪽에 요소를 여러개 추가
let spreadOperator = [1,2,3]
spreadOperator.push(4); // [1,2,3,4]
spreadOperator.push(...[5,6,7]);
console.log('02-1-2. push + spread 연산자 사용해서 배열 뒤에 다중 요소를 추가 :', spreadOperator); // [1,2,3,4,5,6,7]

// 배열 앞쪽에 요소를 여러개 추가
let unshiftSpreadOperator = [1,2,3];
unshiftSpreadOperator.unshift(-1);
unshiftSpreadOperator.unshift(...[-3, -2]);
console.log('02-2-1. unshift + spread 메서드 사용해서 배열 앞에 다중 요소를 추가 :', unshiftSpreadOperator);
```

#### :pushpin: 3-3) `apply()`메서드와 `spread`연산자의 차이 
현재는 공부를 하고 있는 입장이라 실무에서 어떻게 쓰이는지 gpt를 통해 정리해봤다.
   - `apply()` 메서드는 주로 `this` 값을 설정하거나, 기존 배열 메서드를 유사 배열 객체에 적용할 때 사용된다.
   - `spread` 연산자: 배열을 개별 인수로 간편하게 전달할 때 사용된다.

##### `apply()`메서드 `spread`연산자 실무에서 사용시 
   - 함수 호출 시 `this` 값을 설정할 필요가 없고 단순 배열을 개별 인수로 전달할 때 `spread`연산자를 사용하자.
   - `this`값을 설정하거나 기존 배열 메서드를 유사 배열 객체에 적용해야 하는 경우에는 `apply()`메서드를 사용하자.


### :four: 배열 제거 방법
배열을 제거하는 방법은 `pop()`메서드와 `shift()`메서드가 있다.

   - `pop()` : 배열의 끝에 요소를 제거함
   - `shift()` : 배열의 앞에 요소를 제거함

#### :pushpin: 4-1) `pop()`메서드
`pop()`메서드는 배열의 마지막 요소를 제거하니 어디에 쓰일까 고민이 정말 많았다.<br/>
결국 데이터를 삭제하는 것으로 생각이 정해지는데, 게임으로 생각해보면 능력치를 찍었다가 다시 되돌릴 때 쓰이지 않을까 싶다.<br/>
아니면 게시글의 총 갯수가 정해져있을 때, 최신글을 올릴 경우 마지막 글이 삭제되는 용도로 쓰일 것 같다!

```javascript
let popMethod = [1,2,3,4,5];
popMethod.pop();
console.log('03-1. pop 메서드 사용해서 배열 끝 요소를 제거 :', popMethod); // [1,2,3,4]
```

##### :page_facing_up: `for문`을 사용해서 배열 끝의 다중 요소를 제거
`push()`메소드를 사용할 때는 `apply()`와 `spread`연산자를 사용했지만 `pop()`메서드는 3가지 방법이 있다. <br/>
그 중 하나인 반복문을 통해 적용해봤다.
   - 제거할 요소의 개수가 변동적일 때 사용하면 좋다. (변동적인 값을 변수로 적용)

```javascript
let popFor = [1,2,3,4,5],
    forCount = 3; // 제거할 배열의 갯수

for(let i = 0; i < forCount; i ++) popFor.pop(); // count 갯수 만큼 배열 끝의 요소를 제거함.
console.log('03-1-1. 반복문 + pop() 사용해서 배열 끝 다중 요소를 제거함 :', popFor); // [1,2]
```

##### :page_facing_up: `splice` 메서드 사용해서 배열 끝에 다중 요소를 제거한다.
`splice`메서드는 처음 선언한 변수(원본)를 직접 수정해서, 원본 배열을 변경해야할 경우 유용하다. 

```javascript
let popSplice = [1,2,3,4,5],
    spliceCount = 3; // 제거할 배열의 갯수

popSplice.splice(-spliceCount, spliceCount); // 배열 끝에서 spliceCount 개수만큼 요소 제거
console.log('03-1-2. splice 사용해서 배열 끝 다중 요소를 제거함 :', popSplice); // [1,2]
```

##### :page_facing_up: `slice` 메서드 사용해서 배열 끝에 다중 요소를 제거한다.
제거할 요소의 위치나 범위가 고정적일 때 사용하면 좋다.

```javascript
let popSlice = [1,2,3,4,5],
    sliceCount = 3; // 제거할 배열의 갯수

popSlice = popSlice.slice(0, popSlice.length - sliceCount);  // 배열 끝에서 count 개수만큼 요소 제거
console.log('03-1-3. slice 메서드를 사용해서 배열 끝에 다중 요소를 제거함 :', popSlice) // [1,2]
```

#### :pushpin: 4-2) `shift()`메서드
`pop()`메서드완 반대로 배열의 앞쪽 요소를 제거하는 메서드이다.<br/>
`shift()`메서드도 어떤 상황에 쓰일지 고민하다 게임에서 채팅창으로 쳤을 때 가장 마지막에 쓴 채팅이 이전 채팅을 가리고 앞에 보여하니 이럴 경우에 쓰이지 않을까 싶다.<br/>
너무 게임쪽으로만 생각하는 것 같아서 조금 아쉽다.

```javascript
let shiftMethod = [1,2,3,4,5];
shiftMethod.shift();
console.log('03-2. shift 메서드를 사용해서 배열 앞쪽 요소를 제거함 :', shiftMethod); // [2,3,4,5]
```

##### :page_facing_up: `for문`을 사용해서 배열 앞쪽 다중 요소를 제거
`pop()`메서드를 사용했을 때와 동일하게 제거할 요소의 개수가 변동적일 때 사용하면 좋다. 

```javascript
let shiftFor = [1,2,3,4,5],
    shiftForCount = 3; // 제거할 배열의 갯수

for(let i = 0; i < shiftForCount; i ++) shiftFor.shift(); // count 갯수 만큼 배열 앞쪽 요소를 제거함.
console.log('03-2-1. 반복문 + shift() 사용해서 배열 앞쪽 다중 요소를 제거함 :', shiftFor); // [4,5]
```

##### :page_facing_up: `slice()`메서드를 사용해서 배열 앞쪽 다중 요소를 제거
제거할 요소의 위치나 범위가 고정적일 때 사용하면 좋다.

```javascript
let swiftSlice = [1,2,3,4,5],
    swiftSliceCount = 3; // 제거할 배열의 갯수

swiftSlice = swiftSlice.slice(swiftSliceCount);  // 배열 끝에서 count 개수만큼 요소 제거
console.log('03-2-3. slice 메서드를 사용해서 배열 앞쪽 다중 요소를 제거함 :', swiftSlice) // [4,5]
```

### :fire: 마무리
배열의 요소를 생성,추가,삭제하는 방법에 대해 알아보았다. <br/>
추가하는 `push()`, `unshift()` 메서드<br/>
삭제하는 `pop()`, `shift()` 메서드<br/>
다중으로 추가,삭제를 할 때는 `apply()`메서드, `spread`연산자 및 `for()`, `splice()`, `slice()`을 사용하는 방법을 알 수 있었다.


## :link: 링크
- Git: [rarrit github](https://github.com/rarrit/TIL/blob/main/Array/01.array_method.html)
