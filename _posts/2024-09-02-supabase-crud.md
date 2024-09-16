---
title: "React + Supabase 시작하기 (3), POST CRUD 구현"
date: 2024-09-02
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - supabase
  - til
tags:
  - supabase
  - database
author_profile: true
sidebar_main: true
---

## :ledger: 리액트와 Supabase를 사용하여 POST CRUD를 구현해보자.

로그인, 회원가입 후 적용해볼 것은 Post CRUD이다. 마침 이번 팀 프로젝트에서 게시물 등록, 수정 역할이어서 정리해봤다.

### :one: Post 테이블 생성

게시물 데이터를 저장하기 위해 public 스키마에 `Post` 테이블을 생성했다. 간략히 소개하자면, 이번 프로젝트는 stack overflow처럼 사용자가 질문(코드 포함)을 작성하면, 다른 사용자가 댓글을 달고 작성자가 해결되면 채택해주는 기능이다. Columns는 아래와 같다.

- `id`: 게시물의 아이디 값
- `created_at`: 게시물 생성 시간
- `title`: 게시물 타이틀
- `description`: 게시물 내용
- `code`: 코드블럭 내용
- `userID`: 작성한 사람의 ID, public의 userInfo.id 참조함
- `solve`: 게시물 댓글 채택 여부

| Name | Type | default Value |
| id | uuid | gen_random_uuid() |
| created_at | timestampltz | now() |
| title | varchar | NULL |
| description | varchar | NULL |
| code | varchar | NULL |
| userID | uuid | NULL |
| solve | bool | NULL |

### :two: Toast Editor 사용

이번에 글 작성 영역을 에디터 라이브러리를 사용하려 했는데, 구글링해서 다양한 라이브러리를 확인했지만 이전에 사용해본 Toast UI 라이브러리가 친숙해서 [Toast UI Editor](https://ui.toast.com/tui-editor/)를 선택했다. TuiEditor.jsx로 컴포넌트를 생성한 후 재사용 가능하게 설정해줬다.

```jsx
// TuiEditor.jsx
import React, { useRef } from "react";
import { Editor } from "@toast-ui/react-editor";
import "@toast-ui/editor/dist/toastui-editor.css";
import { useEffect } from "react";

// 게시글 내용과 onChange 이벤트를 props로 전달받음
const TuiEditor = ({ description, onChange }) => {
  // useRef를 사용하여 에디터의 인스턴스를 참조함
  const editorRef = useRef();

  useEffect(() => {
    // 렌더링 시 존재여부 확인
    if (editorRef.current) {
      editorRef.current.getInstance().setMarkdown(description || ""); // 내용이 없을 경우 빈 값으로 설정
    }
  }, [description]); // description 이 변경될 때마다 실행

  const handleChange = () => {
    const data = editorRef.current.getInstance().getMarkdown(); // 여기서 에디터 내용 가져옴
    onChange(data); // 부모 컴포넌트로 에디터 내용을 전달
  };

  return (
    <div>
      <Editor
        initialValue={description ?? ""}
        previewStyle="vertical"
        height="400px"
        initialEditType="markdown"
        useCommandShortcut={true}
        ref={editorRef}
        onChange={handleChange} // 에디터 변경 시 handleChange 호출
      />
    </div>
  );
};

export default TuiEditor;
```

### :three: Post 등록

1. 글 작성은 로그인한 회원만 작성 가능하기에, user의 정보를 관리하고있는 context에 접근해서 `loginUserInfo` 값을 가져옴
2. `loginUserInfo`(로그인 User 정보)를 useContext를 사용하여 값을 가져옴
3. DB에 저장할 값인 postTitle(타이틀), postDesc(게시글 본문)을 생성
4. 기본적으로 유저의 값이 들어갈 이름,프로필 이미지에 `loginUserInfo`의 값으로 바인딩해줌
5. 등록 버튼을 클릭하면 state에 업데이트된 값으로 supabase Post 테이블에 저장해준다.

```jsx
const PostRegister = () => {
  const { loginUserInfo } = useContext(dataContext); // 로그인한 user정보
  const [postTitle, setPostTitle] = useState("");
  const [postDesc, setPostDesc] = useState("");
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();

    await supabase.from("Post").insert([
      {
        title: postTitle,
        description: postDesc,
        userId: loginUserInfo.id,
        solve: false,
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
      {/* 타이틀 */}
      <input
        type="text"
        value={postTitle}
        onChange={(e) => setPostTitle(e.target.value)}
      />

      {/* 유저 프로필 이미지 */}
      <img src={loginUserInfo.profileImage} />
      {/* 유저 네임 */}
      <span>{loginUserInfo.username}</span>

      {/* 등록 버튼 */}
      <StBtn onClick={handleSubmit}>등록</StBtn>

      {/* 글 영역 */}
      <TuiEditor description={postDesc} onChange={handleEditorChange} />
    </>
  );
};
```

### :three: Post 상세

1. 리스트 클릭 시 post.id 값으로 url을 이동한다.
2. 상세페이지에서 supabase 메서드를 사용하여, 해당 게시물의 id값과 userId값을 가져와서 바인딩처리해준다.

```jsx
// toast ui editor에서 작성된 내용을 보여주기 위해 Viewer를 사용해야함.
import { Viewer } from "@toast-ui/react-editor";
import "@toast-ui/editor/dist/toastui-editor-viewer.css";

const PostDetail = () => {
  // postListItem.jsx
  const handleDetailMove = (post) => {
    navigate(`/detail/${post.id}`);
  };

  // PostDetail.jsx
  useEffect(() => {
    //게시글 정보
    const fetchPosts = async () => {
      const { data, error } = await supabase
        .from("Post")
        .select("*")
        .eq("id", id)
        .single();
      console.log(post);
      if (error) {
        console.log("error =>", error);
      } else {
        console.log("post data =>", data);
        setPosts(data);

        // 작성자 정보 로드
        if (data.userId) {
          fetchAuthor(data.userId);
        }
      }
    };

    //작성자 정보
    const fetchAuthor = async (userId) => {
      const { data, error } = await supabase
        .from("userinfo")
        .select("id, created_at, email, username, profileImage")
        .eq("id", userId)
        .single();
      if (error) {
        console.log("error =>", error);
      } else {
        console.log("post data =>", data);
        setUserInfo(data);
      }
    };

    fetchAuthor();
    fetchPosts();
  }, [id]);

  return (
    <>
      {post.description ? (
        <Viewer initialValue={post.description} />
      ) : (
        <p>Loading...</p>
      )}
    </>
  );
};
```

### :four: Post 수정

1. 게시물 상세페이지에서 useLocation을 사용하여 사용자 값을 전달받는다.
2. useParams를 사용하여 Post의 id값을 찾아서 set함수를 사용하여, 상태값을 저장해준다.
3. `handleModifyPost` 함수를 통해 업데이트된 상태값을 수정한 글에 업데이트해준다.

```jsx
const PostModify = () => {
  const location = useLocation();
  const navigate = useNavigate();
  const { id } = useParams();
  const [postTitle, setPostTitle] = useState("");
  const [postDesc, setPostDesc] = useState("");
  const [postCode, setPostCode] = useState("");
  const { userName, userProfileImg } = location.state || {};

  useEffect(() => {
    const fetchPosts = async () => {
      const { data, error } = await supabase
        .from("Post")
        .select("*")
        .eq("id", id)
        .single();
      if (error) {
        console.error("데이터 전달 오류:", error);
      } else {
        setPostTitle(data.title);
        setPostDesc(data.description);
        setPostCode(data.code);
      }
      console.log("aasdfasfsaa =>>>", data.code);
    };
    fetchPosts();
  }, [id]);

  const handleModifyPost = async (e) => {
    e.preventDefault();

    const { data, error } = await supabase
      .from("Post")
      .update({
        title: postTitle,
        description: postDesc,
        code: postCode,
      })
      .eq("id", id);

    if (error) {
      console.error("포스트 업데이트 오류", error);
    } else {
      console.log("포스트 업데이트 성공:", data);
      navigate(-1); // 수정 후 상세 페이지로 이동
    }
  };
  const handleEditorChange = (newDescription) => {
    setPostDesc(newDescription);
  };

  return (
    <>
      <input
        type="text"
        value={postTitle}
        onChange={(e) => setPostTitle(e.target.value)}
      />
      <img src={userProfileImg} />
      <span>{userName}</span>
      <TuiEditor description={postDesc} onChange={handleEditorChange} />
    </>
  );
};
```

### :five: Post 삭제

버튼 클릭 시 현재 Post의 id값을 supabase의 delete 메서드를 통해 삭제해준다.

```jsx
const handleDeletePost = async (postId) => {
  let reallyDelete = confirm("정말 삭제하시겠습니까?");
  if (reallyDelete === true) {
    const { error } = await supabase.from("Post").delete().eq("id", postId);

    if (error) {
      console.error("게시글 삭제 오류:", error);
    } else {
      alert("게시글이 삭제되었습니다.");
      navigate("/");
    }
  } else {
    return;
  }
};
```

### :six: supabase 메서드 요약 (CRUD)

#### :pushpin: 6-1) Create

- 데이터베이스에 새 게시물을 추가하려면 `Supabase`의 `insert` 메서드를 사용해야함.
- 이 메서드는 테이블에 새로운 레코드를 삽입하며, 성공적으로 삽입된 경우 해당 레코드가 반환됨

```jsx
await supabase.from("Post").insert([
  {
    title: postTitle,
    description: postDesc,
    userId: loginUserInfo.id,
    solve: false,
  },
]);
```

#### :pushpin: 6-2) Read

- 특정 게시물 또는 모든 게시물을 조회하려면 `Supabase`의 `select` 메서드를 사용해야함.
- 조건을 추가하여 원하는 데이터를 필터링할 수 있음.

```jsx
const { data, error } = await supabase
  .from("Post")
  .select("*")
  .eq("id", postId)
  .single();
```

#### :pushpin: 6-3) Update

- 기존 게시물의 내용을 수정하려면 `Supabase`의 `update` 메서드를 사용해야함.
- 이 메서드는 조건에 맞는 레코드의 특정 컬럼 값을 업데이트함.

```jsx
const { data, error } = await supabase
  .from("Post")
  .update({
    title: postTitle,
    description: postDesc,
    code: postCode,
  })
  .eq("id", id);
```

#### :pushpin: 6-4) Delete

- 게시물을 삭제하려면 `Supabase`의 `delete` 메서드를 사용해야함
- 이 메서드는 조건에 맞는 레코드를 삭제함.

```jsx
const { error } = await supabase.from("Post").delete().eq("id", postId);
```

### :fire: 마무리

이번 팀 프로젝트 중 맡은 역할인 등록,수정 페이지를 진행하며, 유저를 작업한 경혜님과 리스트,상세를 작업한 연주님이 고생을 많이하셨다. 내가 작업한 부분에서는 값을 전달받고 상태만 업데이트 해주면 되었는데, 앞에서 DB세팅 및 값 처리를 잘 해주어서 어려움 없이 진행한 것 같다.
