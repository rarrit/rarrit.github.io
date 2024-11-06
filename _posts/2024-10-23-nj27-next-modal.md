---
title: "[Next.js] 공통 Modal(모달) 만들기"
date: 2024-10-23
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
tags:
  - modal
author_profile: true
sidebar_main: true
---

## :ledger: [Next.js] 공통 Modal(모달) 만들기

스파르타 마지막 최종 프로젝트에서 기능을 공통 컴포넌트부터 분배했다. 공통 컴포넌트 중 모달을 제작하게 되었으며, 어떻게 구현했는지 작성해봤다.

### :one: 디렉토리 구조

공통 모달로 사용하기 위해 컴포넌트를 따로 분리했다.

📦_components<br/>
┣ 📂modal<br/>
┃ ┣ 📜Modal.tsx<br/>
┃ ┗ 📜ModalButton.tsx<br/>

### :two: Modal UI 컴포넌트

`Modal.tsx`에서 전달받는 props는 아래와 같다.

- `modalClass`: 모달 타입을 확인하기 위해 적용
- `wid`: 넓이
- `children`: 모달안의 컨텐츠
- `isOpen`: 모달의 상태값
- `onClose`: 모달 닫기 함수

처음 구상했을 때 와이어 프레임이 없어서, 어떤 타입이 들어올 지 몰라서 `modalClass`를 통해 다른 ui를 보여줄 수 있게 생각했고, `children`을 사용해서 모달의 컨텐츠 또한 자유롭게 적용될 수 있게 해주었다. 코드는 아래와 같다.

```tsx
import React from "react";

type ModalProps = {
  modalClass?: string;
  wid: string;
  isOpen: boolean;
  children: React.ReactNode;
  onClose: () => void;
};

const Modal = ({ modalClass, wid, children, isOpen, onClose }: ModalProps) => {
  if (!isOpen) return null;
  return (
    <div
      className={`modal ${modalClass ? modalClass : ""} w-full max-w-[${wid}]`}
    >
      <div className="modal_cont">
        <div className="modal_inner">{children}</div>
        <button type="button" className="modal_close_btn" onClick={onClose}>
          닫기
        </button>
      </div>
    </div>
  );
};

export default Modal;
```

### :two: Modal Button 컴포넌트

`ModalButton.tsx` 에서 전달 받는 props는 아래와 같다.

- `onClick:`: 모달의 상태값을 true 변경할 함수 (근데 지금 글을 작성하면서 느낀건데 함수명이 이상해서 open으로 변경해야겠다.)
- `buttonText`: 버튼 텍스트가 열기인지, open인지 모르겠어서 텍스트 또한 props로 전달받게 적용함

코드는 아래와 같다.

```tsx
"use client";

type ModalButtonProps = {
  onClick: () => void;
  buttonText: string;
};

const ModalButton = ({ onClick, buttonText }: ModalButtonProps) => {
  return (
    <>
      <button type="button" className="modal_btn" onClick={onClick}>
        {buttonText}
      </button>
    </>
  );
};

export default ModalButton;
```

### :three: Modal Test 페이지

생성한 컴포넌트를 사용하여 클라이언트 서버에서 테스트를 진행해봤다.

- `isOpen`: 모달의 상태 (true / false)
- `handleModalOpen`: setState를 사용해서, 모달의 상태를 true 변경하는 함수
- `handleModalClose`: setState를 사용해서, 모달의 상태를 false 변경하는 함수

아래와 같이 적용했을 때, 기능은 정상적으로 적용되었고 이후 디자인을 통해 모달이 어떤 type이 있는지 확인해서 `props`를 추가해줘야겠다.

```tsx
"use client";

import Modal from "@/_components/modal/Modal";
import ModalButton from "@/_components/modal/ModalButton";
import { useState } from "react";

const ModalTestPage = () => {
  const [isOpen, setIsOpen] = useState(false);

  const handleModalOpen = () => setIsOpen(true);
  const handleModalClose = () => setIsOpen(false);

  return (
    <div>
      <h1>모달 테스트</h1>
      <ModalButton
        buttonText="모달 열기"
        onClick={handleModalOpen} // 버튼 클릭 시 모달 열기
      />

      <Modal
        modalClass={""}
        wid={"500"}
        isOpen={isOpen}
        onClose={handleModalClose}
      >
        <h2>모달 내용</h2>
        <p>모달 내의 텍스트입니다.</p>
        <button onClick={handleModalClose}>닫기</button>
      </Modal>
    </div>
  );
};

export default ModalTestPage;
```

### :fire: 마무리

모달을 처음 만들어 보면서 공통으로 사용하기 위해 어떤 것들이 필요하닞 고민을 많이 해보게 되었다. 이후에 디자인을 통해 적용했을 때 모달 타입과 어떤 부분을 고려해야하는지 좀 더 자세히 알 수 있을 것 같다. 이후에 모달 작업을 진행 후 글을 업데이트 할 예정이다.
