---
title: "JavaScript의 일급 객체"
date: 2024-07-27
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

## :ledger: 자바스크립트의 일급 객체를 알아보자!

### :one: 일급 객체란?
일급 객체는 아래과 같은 속성을 가진 객체이다.
#### :pushpin: 1-1) 변수에 할당할 수 있음
  - 함수가 마치 값으로 취급되며, 함수가 나중에 사용될 수 있도록 조치가 됨

```javascript
const sayHello = function(){
  console.log(' Hello ! ');
}
```


#### :pushpin: 1-2) 다른 함수의 인자로 전달할 수 있음
  - 콜백함수 : 매개변수로서 쓰이는 함수
  - 고차함수 : 수를 인자로 받거나 return 하는 함수

```javascript
const sayHello = function(){
  console.log(' Hello ! ');
}
function callFunction(func) {
  // 매개변수로 받은 변수가 사실, 함수다.
  func(); // sayHello();
}
callFunction(sayHello); // "Hello"
```

#### :pushpin: 1-3) 다른 함수의 반환 값으로 사용할 수 있음
```javascript
function createAdder(num) {
  return function (x) {
    return x + num;
  }
}
const addFive = createAdder(5); // addFive가 createAdder 함수가되어 매개변수 5를 받음
console.log(addFive(10)); // x = 10 
```

#### :pushpin: 1-4) 객체안의 함수를 할당할 수 있음
```javascript
const person = {
  name: 'John',
  age: 31,
  isMarried: true,
  sayHello: function () {
    console.log(`hello, my name is ${this.name}`); // 화살표 함수의 경우 호이스팅이 안되어서 undefined 됌
  }
}
person.sayHello(); // hello, my name is John
```

#### :pushpin: 1-5) 데이터 구조에 저장할 수 있음
- 함수를 배열이나 객체와 같은 데이터 구조에 저장할 수 있음
```javascript
const sayHello = function(){
  console.log(' Hello ! ');
}
const func = [sayHello, function(){console.log(' hello ~ '); }];
func[0](); // Hello !
func[1](); // hello ~
```

#### :pushpin: 1-6) 배열의 요소로 함수를 할당할 수 있음
```javascript
const myArr = [
  function (a, b) {
    return a + b;
  },
  function (a, b) {
    return a - b;
  }
]
console.log(myArr[0](1, 3)); // 4
console.log(myArr[1](10, 7)); // 17
```

#### :pushpin: 1-7) 여러개의 함수로 사용
```javascript
// `multiplyBy` 이 함수는 고차 함수로, 인자로 받은 num을 기억하는 클로저를 반환함. 
// 반환된 함수는 인자로 받은 x에 num을 곱하는 역할을 합니다.
function multiplyBy(num) {
  return function (x) {
    return x * num;
  }
}
// `add` 함수는 매개변수로 전달받은 x, y 인자 값을 더하고 리턴해줌
function add(x, y) {
  return x + y;
}
const multiplyByTwo = multiplyBy(2); // num 을 2로 설정한 클로저 함수라 할 수 있음 인자로 받은 값에 2를 곱함
const multiplyByThree = multiplyBy(3); // num 을 2로 설정한 클로저 함수라 할 수 있음 인자로 받은 값에 3를 곱함

console.log(multiplyByTwo(10)); // 20
console.log(multiplyByThree(10)); // 30

const result = add(multiplyByTwo(5), multiplyByThree(10));
console.log('result =>', result ); // 40
```

### :two: 일급 객체로서의 함수 

#### :pushpin: 2-1) 고차 함수
함수가 다른 함수를 인자로 받거나 변환할 수 있어 고차 함수를 쉽게 작성할 수 있음
```javascript
function add(a, b) {
  return a + b;
}
function operate(a, b, operation) {
  return operation(a, b);
}
console.log(operate(3,4, add)); // 7
// 1. operate 의 a = 3, b = 4;
// 2. return operation(3, 4);
// 3. add(3, 4) return a +b 
// operate(3,4, add); // 7
```

#### :pushpin: 2-2) 콜백 함수
비동기 프로그래밍에서 콜백 함수를 사용하여 비동기 작업을 처리할 수 있음
```javascript
setTimeout(function(){
  console.log('2초 뒤 실행');
}, 2000);
```

#### :pushpin: 2-3) 클로저 생성
함수가 외부 변수에 접근할 수 있는 클로저를 생성
```javascript
function add(){
  let num = 0;
  return function(){
    count++;
    return count;
  }
}
const addNum = add();
console.log(addNum()); // 1
console.log(addNum()); // 2
```


### :three: 유의할 점
- 고차 함수와 클로저를 많이 사용하면 디버깅이 어려워지며 코드의 흐름을 파악하기 어려워 질 수 있으므로 적절한 주석과 코드 구조를 유지하는 것이 중요함
- 너무 많은 고차 함수나 중첩된 함수 호출은 코드의 가독성을 오히려 떨어뜨릴 수 있으며 코드가 복잡해지지 않게 주의해야함

### :four: 비유를 통한 고차 함수
고차 함수를 기본 함수를 포함하여 요리법으로 비유해본다.
1. 고차 함수는 요리법
  - 고차 함수는 요리법처럼 다른 요리법(함수)을 만들거나 사용할 수 있으며 요리법이 다른 요리법을 참고하거나 특정 재료를 준비하는 방법을 정의하는 것처럼 고차 함수도 다른 함수를 인자로 받거나 반환할 수 있음
2. 기본 함수는 요리 재료
  - 기본 함수는 요리 재료로써 여러 요리법(고차 함수)에 사용될 수 있고 요리법에 따라 다르게 조합될 수 있음
3. 예시
  - 기본 함수 : 재료
    - 재료: 프라이팬 = function, 재료 = add(), multiply()
    - 함수: `add`, `multiply`
    - 코드 설명<br/>
    ```javascript
      // 재료 (기본 함수)
      function add(x, y) { // 간계밥
        return x + y;
      }
      function multiply(x, y) { // 김볶밥
        return x * y;
      }
    ```
    
  - 고차 함수 : 요리법
    - 요리법: 간계밥 요리법, 김볶밥 요리법
    - 고차 함수: 다른 함수를 받아서 새로운 함수를 만들어내는 함수
    - 코드 설명<br/>
    ```javascript
    function handleMakeRecipe(operation) {
      return function(x, y) {
        return operation(x,y);
      }
    }
    ```
  
  - 요리법 사용 (고차 함수 사용)
    - 간계밥 요리법처럼 `add`함수를 이용한 고차 함수
    - 김볶밥 요리법처럼 `multiply`함수를 이용한 고차 함수<br/>
    
    ```javascript
    const addRecipe = handleMakeRecipe(add);
    const multiplyRecipe = handleMakeRecipe(multiply);

    console.log(addRecipe(3, 4)); // 7 - 간계밥 요리법
    console.log(multiplyRecipe(3, 4)); // 12 - 김볶밥 요리법
    ```

  - 내용 정리
    - 고차 함수는 요리법처럼 다른 함수(재료)를 조합하여 새로운 기능(요리)을 만듬
    - 고차 함수는 다른 함수를 인자로 받거나, 다른 함수를 반환하여 보다 유연하게 재사용 가능한 코드를 작성하게 함
    - 다양한 요리법이 서로 다른 재료를 사용하듯, 고차 함수는 다양한 기본 함수를 사용하여 원하는 결과 값을 도출함


### :fire: 마무리
일급 객체와 일급 객체로서의 함수를 알아보았다. 처음에 쉽게 생각하기 위해서 함수를 변수에 할당하는 것 그리고 그 변수를 다른 함수의 인자로 전달하거나 반환하는 것으로 생각했었고, 비유를 통해 알아보니 어느정도 이해가 되는 것 같다. 이런 방법 또한 실무에서 사용할 수 있어야 하는데, 어떻게 활용해야 하는지를 고민을 해보았을 때 명확히 그려지지 않았다. 한 가지 생각해보면 `click` 이란 `이벤트 핸들러(요리법)`를 통해 `다양한 함수(재료)`를 실행하는 것과 최근에 자주 사용하게 되는 배열 메서드인 `map`,`filter`,`reduce`또한 요리법으로 생각하고 글을 마무리 해본다.