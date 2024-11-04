---
title: "[Next.js] 공통 페이지네이션 만들기 1차"
date: 2024-10-31
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
tags:
  - pagination
author_profile: true
sidebar_main: true
---

## :ledger: [Next.js] 공통 페이지네이션 만들기 1차

최종 프로젝트에서 나의 파트가 조금 일찍 끝나서 페이징 처리하는 기능도 적용해보기로 했다. 페이징 처리하는 방법은 페이지네이션과 무한스크롤 기능이 있는데, 페이지네이션으로 진행하게로 했고 공통으로 사용할 수 있도록 작업해봤다.

### :one: 기능 목표

1차 작업은 공통으로 사용할 수 있게 제작하고 테스트는 특정 리스트 페이지에서만 적용했다. 기능 목표는 아래와 같다.

1. 범용적으로 사용할 수 있어야함
2. 사용하는 위치에서 추가로 커스텀이 가능해야함
3. 사용 방법이 어렵지 않아야함

### :two: 1차 구현

이전 공통 모달, 공통 슬라이드를 제작하고 이번에 페이지네이션 또한 공통으로 제작하면서 느낀건 확실히 처음 만들 때 미처 생각하지 못한 부분들이 너무 많고 고려해야할 상황도 정말 많다는 것이었다. 코드는 아래와 같다.

<details>
<summary>1차 구현 코드 보기</summary>
<div markdown="1">

```ts
// CampList.tsx
const CampList = ({ camps, itemsPerPage }: CampListProps) => {
  const [page, setPage] = useState<number>(1);

  const firstItems = (page - 1) * itemsPerPage;
  const lastItems = page * itemsPerPage;
  const currentItems = camps.slice(firstItems, lastItems);
  const totalPages = Math.round(camps.length / itemsPerPage);

  const handleMovePagePrev = () => {
    if (page <= 1) return false;
    setPage((prev) => prev - 1);
  };

  const handleMovePageNext = () => {
    if (page == totalPages) return false;
    setPage((prev) => prev + 1);
  };

  return (
    <div className="camp_list">
      <ul className="list_box flex flex-wrap gap-[30px]">
        {currentItems.map((camp) => (
          <li key={camp.contentId} className="w-[calc(25%-30px)]">
            <CampCard camp={camp} />
          </li>
        ))}
      </ul>
      <Pagination
        page={page}
        totalPages={totalPages}
        onMovePagePrev={handleMovePagePrev}
        onMovePageNext={handleMovePageNext}
      />
    </div>
  );
};

// Pagination.tsx
type PaginationProps = {
  page: number;
  totalPages: number;
  onMovePagePrev: () => void;
  onMovePageNext: () => void;
};

const Pagination = ({
  page,
  totalPages,
  onMovePagePrev,
  onMovePageNext,
}: PaginationProps) => {
  return (
    <div className="pagination">
      <button onClick={onMovePagePrev}>이전</button>
      {page} / {totalPages}
      <button onClick={onMovePageNext}>다음</button>
    </div>
  );
};

export default Pagination;
```

</div>
</details>

1차 구현 완료 후 들었던 생각은 "ㅋㅋ 조졋네" 였고 **문제점**은 아래와 같아서 리팩토링을 진행하기로 했다.

- 현재 클라이언트 컴포넌트 내에서 직접 작성해줘야 하는 것이 많음 (예: 페이지 상태값, 데이터 리스트 인덱싱, 토탈 값 등등..)
- 작성해줘야 하는게 많다는 건 사용하기 어려운 것 같아서 해당 부분을 훅으로 분리해서 사용할 수 있도록 적용 필요하다 생각함.
- 커스텀 훅으로 변경했을 때 값만 전달받아 자동으로 페이징 처리를 할 수 있어야 될 것 같음.

### :three: 2차 구현

2차 구현에서는 1차 구현의 문제를 해결하기 위해 페이지네이션을 사용하는 컴포넌트안에 작성된 로직을 커스텀 훅으로 분리하여, 필요한 데이터와 함수만 컴포넌트에 전달하는 구조로 변경했다. 페이지네이션 이벤트만 전달받아서 기능이 동작하도록 구현해 보려 했다. 작업한 코드는 아래와 같다.

<details>
<summary>2차 구현 코드 보기</summary>
<div markdown="1">

```ts
// CampList.tsx
const CampList = ({ camps, itemsPerPage, paramsId }: CampListProps) => {
  const { currentItems, page, totalPages, movePagePrev, movePageNext } =
    usePagination({ items: camps, itemsPerPage, paramsId });
  return (
    <div className="camp_list">
      <Pagination
        page={page}
        totalPages={totalPages}
        onMovePagePrev={movePagePrev}
        onMovePageNext={movePageNext}
      />
    </div>
  );
}

// Pagination.ts
const Pagination = ({
  page,
  totalPages,
  onMovePagePrev,
  onMovePageNext
}: PaginationProps) => {
  return (
    <div className="pagination">
      <button onClick={onMovePagePrev} disabled={page === 1}>
        이전
      </button>
      {page} / {totalPages}
      <button onClick={onMovePageNext} disabled={page === totalPages}>
        다음
      </button>
    </div>
  );
};

export default Pagination;


// pagination.ts
import { useState } from "react";

// 제네릭 타입 사용해서 다양한 타입의 아이템 배열을 받을 수 있도록해줌
type UsePaginationProps<T> = {
  items: T[]; // 페이지네이션할 아이템 배열
  itemsPerPage: number; // 한 페이지에 보여줄 수 있는 아이템 수
  paramsId: string; // 리스트 페이지 params 값
};

const usePagination = <T>({
  items,
  itemsPerPage,
  paramsId
}: UsePaginationProps<T>) => {
  // 현재 페이지 상태를 관리하는 state
  const [page, setPage] = useState<number>(Number(paramsId));
  // 총 페이지 수를 계산 (데이터 총 갯수 / 설정한 페이지 갯수)
  const totalPages = Math.ceil(items.length / itemsPerPage);

  // 현재 페이지에 해당하는 데이터를 계산해줌
  const currentItems = items.slice(
    (page - 1) * itemsPerPage, // 시작 인덱스 => (page - 1)은 배열이 0부터 시작해서 빼줘야함
    page * itemsPerPage // 종료 인덱스 => 페이지가 2면 인덱스 번호 끝은 16임 (itemsPerPage = 8)
  );

  const movePagePrev = () => {
    if (page > 1) {
      setPage((prev) => prev - 1);
    } else {
      // 사카모토 써야함
      alert("첫번째 페이집니다.");
    }
  };

  const movePageNext = () => {
    if (page < totalPages) {
      setPage((prev) => prev + 1);
    } else {
      // 사카모토 써야함
      alert("마지막 페이집니다.");
    }
  };

  return {
    page, // 현재 페이지
    totalPages, // 총 페이지 갯수
    currentItems, // 현재 페이지에 해당하는 아이템
    movePagePrev, // 이전 페이지로 이동하는 함수
    movePageNext // 다음 페이지로 이동하는 함수
  };
};

export default usePagination;
```

</div>
</details>

커스텀 훅을 생성하여, 컴포넌트에서 직접 작성해줘야 했던 로직을 분리해줬다. 위와 같이 작성해줌으로써 사용하는 컴포넌트내에서 적용해줘야할 것은 이동하는 함수내에서 직접 커스텀이 가능할 수 있게 적용해주었다. 개선된점은 아래와 같다.

- 페이지네이션 로직이 커스텀 훅으로 분리되어 컴포넌트에서 더 깔금하게 관리할 수 있음
- 다른 컴포넌트에서도 재사용이 가능함 (테스트 후 수정이 필요하겠지만..)
- 필요에 따라 훅 내부 로직을 확장할 수 있고, 추가 기능 또한 적용할 수 있음

### :fire: 마무리

이번 페이지네이션 구현은 처음에는 다소 복잡하고 생각지 못했던 부분이 많았지만, 실제 구현을 통해 문제를 마주하고 해결해 나가는 과정에서 많은 것을 돌아볼 수 있었다. 1차 구현은 기본적인 기능을 구현하는 것에 초점을 맞췄다면 2차 구현에서는 공통으로 사용할 수 있게 노력했던 것 같다.
