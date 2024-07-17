---
title: "JavaScript 메뉴 랜더링 제작 및 풀이 과정"
date: 2024-07-13
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - mini
  - til
  - js
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: JavaScript으로 메뉴 랜더링 기능을 만들어 보자!

### :one: 요구 사항

1. 주어진 데이터를 확인한다.<br/>

```javascript
const menuItems = [
  {
    name: "비빔밥",
    description: "밥 위에 나물, 고기, 고추장 등을 얹고 비벼 먹는 한국 요리",
  },
  {
    name: "김치찌개",
    description: "김치와 돼지고기 등을 넣고 끓인 한국의 찌개 요리",
  },
  { name: "불고기", description: "양념한 고기를 구워서 먹는 한국 요리" },
  {
    name: "떡볶이",
    description: "떡과 어묵을 고추장 양념에 볶아 만든 한국의 간식",
  },
  { name: "잡채", description: "당면과 여러 채소, 고기를 볶아 만든 한국 요리" },
];
```

2. 배열 메서드로 데이터를 화면에 표시합니다.
3. (선택 사항) CSS로 간단한 스타일을 적용합니다.
4. (선택 사항) 객체의 속성을 추가 해봅니다.(e.g. 가격, 카테고리 등)
5. (선택 사항) filter를 사용하여 설명에 “고기”가 포함된 메뉴 항목만 화면에 노출시킵니다.

#### :pushpin: 직접 기능 추가한 사항

1. 요구사항 5번까지는 완료하 되, 첫 노출은 전체 메뉴가 노출된다.
2. 버튼을 생성하여 해당 버튼을 클릭할 때 마다 각 카테고리에 맞게 랜더링된다.
3. 추후 카테고리가 추가되어도 쉽게 추가될 수 있게 코드를 작성한다.

### :two: 사용 언어 및 폴더 구조

- HTML5, CSS3, JavaScript
- 📦menuRendering<br/>
  ┣ 📂assets<br/>
  ┃ ┣ 📂css<br/>
  ┃ ┃ ┗ 📜menuRendering.css<br/>
  ┃ ┗ 📂js<br/>
  ┃ ┃ ┗ 📜menuRendering.js<br/>
  ┗ 📜menuRendering.html

### :three: 작성 코드

- HTML 파일

  ```html
  <div class="container">
    <div class="menuBox">
      <h1>한식 메뉴</h1>
      <div class="sortBox">
        <button type="button" id="btnAll">전체보기</button>
        <button type="button" id="btnMeatOn">고기좋아</button>
        <button type="button" id="btnRice">밥좋아</button>
        <button type="button" id="btnNoodle">면좋아</button>
      </div>
      <ul id="menuList">
        <!-- 데이터 랜더링 위치 -->
      </ul>
    </div>
  </div>
  ```

- JS 파일

  ```javascript
  document.addEventListener("DOMContentLoaded", () => {
    const menuList = document.getElementById("menuList"); // 메뉴 리스트
    const btnAll = document.getElementById("btnAll"); // 전체 버튼
    const btnMeatOn = document.getElementById("btnMeatOn"); // 고기좋아 버튼
    const btnRice = document.getElementById("btnRice"); // 밥좋아 버튼
    const btnNoodle = document.getElementById("btnNoodle"); // 면좋아 버튼
    const menuItems = [
      {
        name: "비빔밥",
        description: "밥 위에 나물, 고기, 고추장 등을 얹고 비벼 먹는 한국 요리",
      },
      {
        name: "김치찌개",
        description: "김치와 돼지고기 등을 넣고 끓인 한국의 찌개 요리",
      },
      { name: "불고기", description: "양념한 고기를 구워서 먹는 한국 요리" },
      {
        name: "떡볶이",
        description: "떡과 어묵을 고추장 양념에 볶아 만든 한국의 간식",
      },
      {
        name: "잡채",
        description: "당면과 여러 채소, 고기를 볶아 만든 한국 요리",
      },
    ];

    // 카테고리를 추가한 배열을 생성함
    const menuItemsData = menuItems.map((menuItem) => ({
      ...menuItem, // 이전 객체 복사
      categories: {
        // 카테고리 추가시 아래에 추가
        meat: menuItem.description.includes("고기"),
        soup: menuItem.description.includes("찌개"),
        rice: menuItem.description.includes("밥"),
        noodle: menuItem.description.includes("면"),
      },
    }));
    console.log(menuItemsData);

    // 랜더링 될 메뉴들
    const renderMenus = (menuItems) => {
      const menuHtml = menuItems
        .map(
          (menuData) => `
        <li>
          <strong class='menuTitle'>${menuData.name}</strong>
          <p class='menuDesc'>${menuData.description}</p>
        </li>
      `
        )
        .join("");
      menuList.innerHTML = menuHtml;
    };

    // 로딩시 전체 노출
    renderMenus(menuItemsData);

    // 카테고리 필터링
    const menuCategoryFilter = (category) => {
      category === "all"
        ? // 카테고리가 전체면 전체 데이터 노출
          renderMenus(menuItemsData)
        : // 전체가 아니라면 전체 데이터 categories의 카테고리명에 따라 노출
          renderMenus(
            menuItemsData.filter((items) => items.categories[category])
          );
    };

    // 전체보기 버튼 클릭
    btnAll.addEventListener("click", () => menuCategoryFilter("all"));
    // 고기좋아 버튼 클릭
    btnMeatOn.addEventListener("click", () => menuCategoryFilter("meat"));
    // 밥좋아 버튼 클릭
    btnRice.addEventListener("click", () => menuCategoryFilter("rice"));
    // 면좋아 버튼 클릭
    btnNoodle.addEventListener("click", () => menuCategoryFilter("noodle"));
  });
  ```

### :four: 풀이 과정

1. 변수 선언<br/>
   마지막에 랜더링 되어야 할 요소가 필요하여 `menuList`를 선언<br/>
   각 카테고리 버튼 클릭 시 이벤트 처리를 위한 버튼들도 변수로 담았다.

2. `menuItemsData` 카테고리 추가할 수 있도록 함수를 만들었다.<br/>
   카테고리를 추가하더라도 기존 원본 배열은 변경되지 않게 `map()`메서드를 사용함

3. `renderMenus` 랜더링 되어야할 HTML 요소를 만들었다. <br/>
   매개변수 `menuItems`는 초기 데이터 `menuItems`를 전달 받아 li에 데이터바인딩 후 새롭게 배열을 생성하여 `menuList`에 뿌려줌

4. `renderMenus(menuItemsData);` 페이지 로딩 시 전체 메뉴 노출<br/>
   위의 과정을 거친 후 첫 페이지 로드시에 전체 메뉴를 보여준다.

5. `menuCategoryFilter` 카테고리 필터링 함수<br/>
   여기서 삼항연산자를 사용하여 전체 메뉴의 카테고리인 `all`이면 기존 원본데이터를 노출시키고, 아니라면 `filter()`메서드를 사용하여 카테고리의 값을 반환한다.

6. `addEventListener('click', () => menuCategoryFilter('카테고리명'));`
   마지막으로 버튼을 클릭할 때 `menuCategoryFilter(카테고리명)`의 카테고리명을 함수에서 전달받아 삼항연산자를 통해 그에 맞는 카테고리 배열이 랜더링된다.

#### :pushpin: 적용한 코드

**깃헙 링크**: [메뉴 랜더링](https://github.com/rarrit/TIL/tree/main/Project/menuRendering)

### :fire: 마무리

처음 요구사항에 맞춰 작업했을 때, 카테고리가 앞으로도 계속 추가될 수 있는 재사용 가능한 기능으로 만들고 싶어져서 추가로 기능을 넣어 만들었다.<br/>
카테고리 필터링 부분에서 시간이 정말 많이 걸렸었고, 원래는 `renderMenus`에서 if문으로 처리하여 코드를 제작했는데 추후 유지보수에서 if의 조건을 계속 넣어야 하는 불편함이 있어서 `menuCategoryFilter`함수를 추가했다.<br/>
역시나 시간이 정말 많이 소요되었지만 앞으로도 열심히 경험을 해봐야겠다!
