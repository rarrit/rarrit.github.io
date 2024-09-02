---
title: "React Toast UI Editor Library 사용법"
date: 2024-09-02
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - lib
  - react
tags:
  - lib
  - editor
author_profile: true
sidebar_main: true
---

## :ledger: 리액트에서 text editor 라이브러리인 Toast UI Editor를 사용해보자!

팀 과제를 진행하던 중 게시물 등록,수정 페이지를 맡게 되었다. 게시물 본문의 글을 작성하는데에 textarea를 사용하려다가 이전에 사용했던 toast ui library가 생각나 찾아보게 되었고 Toast UI Editor를 알게되어 선택했다!

### :one: 설치

[공식문서 - install guide](https://nhn.github.io/tui.editor/latest/#-install)를 참고해도 되고 터미널에 아래의 명령어를 입력한다.

```bash
// npm
npm i --save @toast-ui/editor @toast-ui/react-editor

// yarn
yarn add @toast-ui/editor @toast-ui/react-editor
```

### :two: 컴포넌트 생성

등록,수정 페이지 둘 다 사용하기에 재사용 가능하게 컴포넌트로 생성하여 사용했다.

#### :pushpin: 2-1) 작성 코드

```jsx
// TuiEditor.jsx
import React, { useRef } from "react";
import { Editor } from "@toast-ui/react-editor";
import "@toast-ui/editor/dist/toastui-editor.css";
import { useEffect } from "react";

// description : 초기 마크다운 내용
// onChange : 콜백 함수
const TuiEditor = ({ description, onChange }) => {
  const editorRef = useRef();

  useEffect(() => {
    if (editorRef.current) { // 해당 에디터에 작성되었는 지 확인
      // setMarkdown 메서드를 통해 에디터 내용을 description으로 설정함
      editorRef.current.getInstance().setMarkdown(description || ''); // 빈 값일 경우 '' 초기화
    }
  }, [description]); // 게시글이 값이 변경될 때마다 실행

  const handleChange = () => {
    const data = editorRef.current.getInstance().getMarkdown(); // 여기서 에디터 내용 가져옴
    onChange(data); // 부모 컴포넌트로 에디터 내용을 전달
  };


  return (
    <Editor
      initialValue={description ?? ""}
      previewStyle="vertical"
      height="400px"
      initialEditType="markdown"
      useCommandShortcut={true}
      ref={editorRef}
      onChange={handleChange} // 에디터 변경 시 handleChange 호출
    />
    {/* <button onClick={handleClick}>Get Markdown</button> */}
  );
};

export default TuiEditor;
```

#### :pushpin: 2-2) 옵션 설명

더 다양한 옵션을 확이하려면 [공식문서](https://nhn.github.io/tui.editor/latest/ToastUIEditorCore)를 참고!

| 옵션 | 타입 | 설명 |
| initialValue | string | 에디터의 초기값. 수정 기능 구현시 유용 |
| initialEditType | string | 에디터 초기 타입 설정. markdown, wysiwyg 중 선택 가능 |
| autofocus | boolean = true | true일 경우 자동으로 에디터에 포커스 설정 |
| toolbarItems | array | 툴바 아이템 |
| hideModeSwitch | boolean = false | markdown / wysiwyg 스위치 탭 숨김 |
| height | string = '300px' | 에디터의 높이 값. ex) '300px', '100%', 'auto' |
| hooks | Object | addImageBlobHook - 이미지 업로드에 사용되는 훅 설정 |

### :three: 에디터 사용

1. `tuiDeitor` 컴포넌트를 사용할 컴포넌트로 Import
2. `useState`로 초기 값이 빈 문자열로 상태를 정의함
3. `handleSubmit` 함수에서 Post 테이블 description column에 `postDesc` 즉, 작성한 글을 삽입함
4. `handleEditorChange` 함수에서 글 내용이 변경될 때 매개변수로 값을 전달받고 set함수를 통해 `postDesc` 상태를 업데이트함

```jsx
import TuiEditor from "../components/TuiEditor";

const postRegister = () => {
  const [postDesc, setPostDesc] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();

    await supabase.from("Post").insert([
      {
        description: postDesc,
      },
    ]);

    alert("글이 등록되었습니다!");
    navigate("/");
  };
  const handleEditorChange = (newDescription) => {
    setPostDesc(newDescription);
  };
  return (
    <>
      <TuiEditor description={postDesc} onChange={handleEditorChange} />
      <StRegiBtn onClick={handleSubmit}>등록</StRegiBtn>
    </>
  );
};
```

![toast ui editor img](https://github.com/user-attachments/assets/b307505c-23a6-4577-a0eb-2a2869610667)

### :fire: 마무리

처음 사용해보는 에디터 라이브러리인 `Toast UI Editor`는 마크다운 문법과 실시간으로 어떻게 보일 지 확인이 가능하고, 하이라이트, 정렬, 이미지, 파일 등 다양한 기능이 제공되어 편리했다.
