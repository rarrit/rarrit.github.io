---
title: "JavaScript `filter` 사용 방법"
date: 2024-07-12
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

## :ledger: `filter`메서드란 무엇인가?
`filter`메서드는 JavaScript의 배열 메서드이며, 주어진 조건을 만족하는 배열 요소들만을 추출하여 새로운 배열을 반환할 때 사용된다.<br/>
이 메서드는 원본 배열을 변경하지 않고, 조건에 맞는 요소들로 구성된 새로운 배열을 생성한다.<br/>
즉, 주어진 조건 `true`,`false`을 확인하는 용도로 자주 사용된다.


### :one: `filter`메서드의 문법

```javascript
배열.filter((요소,인덱스,원본배열) => {
  // 콜백함수 로직
})
```
   - 요소(`element`) : 처리할 현재 요소 
   - 인덱스(선택 사항) : 처리할 현재 요소의 인덱스
   - 원본배열(선택 사항) : `filter`를 호출할 배열

### :two: `filter`메서드의 특성
   - 원본 배열은 변경되지 않고 새로운 배열을 반환함
   - `callback`함수는 각 요소에 대해 한 번씩 호출함
   - 주로 ***참/거짓을 평가하여 요소를 필터링***하는데 사용됨
   - 배열의 모든 요소를 순회하면서 특정 작업을 하는데는 `forEach`가 적합함
   - 새로운 배열의 길이와 순서를 바꾸는 작업은 `map`이나 `sort`가 적합함

### :three: `filter`메서드 예제 

#### :pushpin: 3-1) 배열의 짝수를 찾아라.
```javascript
const numbers = [1,2,3,4,5,6,7,8,9,10];
const newNumbers = numbers.filter(number => number % 2 === 0);

console.log(newNumbers); // [2,4,6,8,10];
```

#### :pushpin: 3-2) 나이가 5살 이상인 고양이를 찾아라.
```javascript
const cats = [
  {name: 'Ruby', age: 5},
  {name: 'RaRa', age: 2},
  {name: 'Miu', age: 8}
];
const oldCats = cats.filter(cat => cat >= 5);

console.log(oldCats); // {name:'Ruby', age: 5}, {name: 'Miu', age: 8}
```

#### :pushpin: 3-3) 이름이 5글자 이상인 사람을 찾아라.
```javascript
const names = ["rarrit", "ruru", "rubby", "rara", "ririro"];
const longNames = names.filter( name => name.length >= 5);

console.log(longNames); // {"rarrit", "rubby", 'ririro'};
```

### :four: `filter`메서드는 실무에서 언제 사용될까?

#### :pushpin: 4-1) 이커머스 상품 카테고리 필터링
```javascript
const products = [
  {id: 1, name: 'banana', category: 'fruit'},
  {id: 2, name: 'hambuger', category: 'food'},
  {id: 3, name: 'apple', category: 'fruit'},
  {id: 4, name: 'pizza', category: 'food'}
]
const fruitProducts = products.filter(product => product.category === 'fruit')
console.log(fruitProducts); // {id: 1, name: 'banana', category: 'fruit'}, {id: 3, name: 'apple', category: 'fruit'}
```

#### :pushpin: 4-2) 휴먼 회원 필터링
```javascript
const members = [
  {id: 1, name: 'rarrit', human: false},
  {id: 2, name: 'tirrar', human: true},
  {id: 3, name: 'rororo', human: false},
  {id: 4, name: 'rarara', human: true}
]
const humanMembers = members.filter(member => member.human === true);
console.log(humanMembers); {id: 2, name: 'tirrar', human: true}, {id: 4, name: 'rarara', human: true}
```


### :fire: 마무리
오늘은 `filter`메서드에 대해 알아보았다.<br/>
이전 프로그래머스 문제 풀이에서 정말 많이 보았던 메서드인데 공부해보니 배열에서 특정 조건을 만족하는 요소들을 쉽게 추출할 수 있어 다양하게 활용이 가능해보인다.<br/>
오늘 배운 내용을 바탕으로 `filter`메서드를 다양한 프로젝트에서 적극 활용해볼 수 있길 바라며 더 나은 코드 작성을 위해 꾸준히 연습해야겠다.<br/>
