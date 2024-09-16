---
title: "[2차] 팩트폭행 MBTI 테스트 프로젝트 - MBTI 테스트 기능 적용"
date: 2024-09-10
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
  - mini
  - til
tags:
  - mini project
author_profile: true
sidebar_main: true
---

## :ledger: REACT MBTI TEST PROJECT

![1  mbti-test-main](https://github.com/user-attachments/assets/8d85374b-7b5e-4db3-a19a-35bd9fb1f9f5)

## :rocket: 베포 링크

- vercel
  - [https://fact-mbti-test.vercel.app/](https://fact-mbti-test.vercel.app/)
- github
  - [https://github.com/rarrit/react-mbti-test](https://github.com/rarrit/react-mbti-test)

## :one: 프로젝트 설명

팩트로 폭행하는 MBTI TEST 입니다. F는 상처받을 수 있으니 테스트 진행 시 주의 요망

## :two: STACK

![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white) ![Context-API](https://img.shields.io/badge/Context--Api-000000?style=for-the-badge&logo=react) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens) ![React Query](https://img.shields.io/badge/-React%20Query-FF4154?style=for-the-badge&logo=react%20query&logoColor=white) ![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

## :three: 프로젝트 구조

- 2차에선 MBTI 테스트, 결과, 리스트 기능을 적용했습니다.
- 깃에서 `feat/mbti` 브랜치를 통해 확인 가능

<details>
<summary>전체 프로젝트 구조</summary>
📦src<br/>
 ┣ 📂api<br/>
 ┃ ┣ 📜authAPI.js<br/>
 ┃ ┗ 📜mbtiAPI.js<br/>
 ┣ 📂assets<br/>
 ┃ ┣ 📂img<br/>
 ┃ ┃ ┣ 📂mbti<br/>
 ┃ ┗ 📜react.svg<br/>
 ┣ 📂components<br/>
 ┃ ┣ 📂footer<br/>
 ┃ ┣ 📂header<br/>
 ┃ ┣ 📜GlobalStyle.jsx<br/>
 ┃ ┣ 📜Layout.jsx<br/>
 ┃ ┣ 📜ProtectedRoute.jsx<br/>
 ┃ ┣ 📜TestForm.jsx<br/>
 ┃ ┣ 📜TestResultItem.jsx<br/>
 ┃ ┗ 📜TestResultList.jsx<br/>
 ┣ 📂constants<br/>
 ┃ ┗ 📜queryKeys.js<br/>
 ┣ 📂context<br/>
 ┃ ┗ 📜AuthContext.jsx<br/>
 ┣ 📂data<br/>
 ┃ ┗ 📜questions.js<br/>
 ┣ 📂hooks<br/>
 ┃ ┣ 📜mutations.jsx<br/>
 ┃ ┗ 📜queries.jsx<br/>
 ┣ 📂instance<br/>
 ┃ ┗ 📜baseInstance.js<br/>
 ┣ 📂pages<br/>
 ┃ ┣ 📜Join.jsx<br/>
 ┃ ┣ 📜Login.jsx<br/>
 ┃ ┣ 📜Main.jsx<br/>
 ┃ ┣ 📜Mypage.jsx<br/>
 ┃ ┣ 📜TestPage.jsx<br/>
 ┃ ┗ 📜TestResultPage.jsx<br/>
 ┣ 📂shared<br/>
 ┃ ┗ 📜Router.jsx<br/>
 ┣ 📂utils<br/>
 ┃ ┣ 📜dateResult.js<br/>
 ┃ ┣ 📜mbtiCalculator.js<br/>
 ┃ ┣ 📜mbtiImg.js<br/>
 ┃ ┗ 📜mbtiResult.js<br/>
 ┣ 📂zustand<br/>
 ┃ ┗ 📜userStore.js<br/>
 ┣ 📜App.css<br/>
 ┣ 📜App.jsx<br/>
 ┣ 📜index.css<br/>
 ┗ 📜main.jsx<br/>
</details>

## :four: 프로젝트 세팅 및 기능 설명
### :pushpin: 4-1) 설치 패키지

1. Json-server
2. Tanstack query
3. zustand
4. Axios
5. Tailwind
6. styled-component
7. react-router-dom
8. the-new-css-reset
9. lucide-react

### :pushpin: 4-2) 주요 기능 소개
1. 메인 페이지 (1차에서 완료)
  - 약간의 어그로성 문구와 `테스트하기` 버튼을 통해 로그인 페이지로 이동합니다.
2. 회원가입 페이지
  - 닉네임, 아이디, 비밀번호 입력 후 회원가입 버튼을 클릭하면 "회원가입이 완료되었습니다." 문구와 함께 `로그인` 페이지로 이동합니다.
3. 로그인 페이지
  - 아이디와, 비밀번호를 입력 후 로그인 버튼을 클릭하면 "로그인이 완료되었습니다." 문구와 함께 `메인` 페이지로 이동합니다.
4. 프로필 페이지
  - 아이디, 이전 닉네임을 확인할 수 있으며, 변경 닉네임에 값을 입력 후 닉네임을 변경 버튼을 클릭하면 변경된 닉네임으로 확인이 가능합니다.
5. 테스트 페이지
  - 12개의 문항에 대해 체크 후 저장 버튼을 클릭하면, MBTI 테스트 결과가 화면에 출력됩니다.
6. 결과보기 페이지
  - MBTI 테스트 결과 리스트가 있는 페이지입니다.
    - 게시물은 본인이 쓴 게시물만 공개/비공개, 삭제 처리가 가능합니다.
      - 비공개 선택 시 해당 글은 결과보기 페이지에서 본인만 확인이 가능합니다.

## :five: 작업 목록

### :pushpin: 5-1) MBTI TEST PAGE 

![5  mbti-test-test](https://github.com/user-attachments/assets/6125ffa1-ed27-45c3-9bae-71af51103b2b)

```jsx
// components/TestForm.jsx
const navigate = useNavigate();
const { isUserInfo } = useContext(AuthContext);
const [resultInfo, setResultInfo] = useState(null);

const handleTestSubmit = async (answers) => {
  const result = calculateMBTI(answers);    
  console.log(result);
  setResultInfo(result); 
  const resultData = {
    userId: isUserInfo.id,
    nickname: isUserInfo.nickname,
    result,
    // answers,      
    date: new Date().toISOString(),
    visibility: true,
  };    
  console.log("resultData =====>", resultData);
  await createTestResult(resultData);    
  
};

const mbtiImg = getMbtiImg(resultInfo);
return (    
  {resultInfo
  ? (
    <StResultForm>
      <h2>{resultInfo}</h2>
      <StImg className="mbtiImg" src={mbtiImg} />
      <p className="minSans" dangerouslySetInnerHTML={{__html: mbtiResult(resultInfo)}}/>

      <div className="btnArea">
        <button onClick={() => setResultInfo(null)}>다시하기</button>
        <button onClick={() => navigate('/results')}>결과 리스트 보기</button>
      </div>          
    </StResultForm>
  ) 
  : ( 
    <>
      <TestForm onSubmit={handleTestSubmit}/>        
    </>   
  )}  
)
```

- `TestPage`
  - 사용자가 12개의 질문에 대한 답변을 선택하고 제출할 수 있는 설문 폼을 제공한다.
  - MBTI 테스트 결과를 계산하고 저장하는 역할을 수행한다.
  - **상태관리**
    - useState를 사용해 resultInfo 상태를 생성하여 MBTI 결과를 저장한다.
    - AuthContext의 isUserInfo를 통해 사용자 정보를 받아와, 사용자 ID와 닉네임을 사용한다.
  - **handleTestSubmit** 함수
    - 설문 제출 시 호출되는 함수로, 사용자가 선택한 answers 배열을 calculateMBTI 함수를 통해 MBTI 결과로 변환한다.
    - MBTI 결과를 setResultInfo로 상태에 저장한 뒤, resultData 객체를 생성해 사용자 ID, 닉네임, 결과, 날짜, 공개 여부 등의 정보를 포함한다.
    - createTestResult 함수를 호출하여 해당 데이터를 서버에 저장한다.
  - **결과 화면**
    - MBTI 테스트 결과가 있으면, getMbtiImg로 이미지를 가져오고, mbtiResult 함수로 결과 설명을 가져와 화면에 표시한다.
    - 결과가 없을 때는 TestForm 컴포넌트를 렌더링하여 사용자가 테스트를 진행하도록 한다.


---

```jsx
// components/TestForm.jsx
const TestForm = ({ onSubmit }) => {
  const [answers, setAnswers] = useState(Array(questions.length).fill(null));

  // 체크된 input의 값을 전달 (순서, yes or no)
  const handleChange = (index, answer) => {
    // 기존의 answers 배열을 복사 (불변성을 유지하기 위해)
    const newAnswers = [...answers];
    // 특정 질문(index)에 해당하는 답변을 새로운 값(answer)으로 변경
    newAnswers[index] = answer;
     // 업데이트된 배열을 상태로 설정
    setAnswers(newAnswers);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(answers);
    window.scrollTo({
      top: 0,
      behavior: "smooth",
    });
  };
}  
```


- `TestForm`
  - **상태관리**
    - `useState`를 사용해 `answers` 상태를 생성 및 `questions` 배열의 길이만큼 `null`로 채워진 배열을 초기값으로 설정하며, 각 질문에 대한 답변이 `answers` 배열의 특정 인덱스에 저장된다.
  - **handleChange 함수**
    - 사용자가 특정 질문에 대해 답변을 선택할 때 호출되는 함수.
    - `answers` 배열을 복사하여 선택된 질문(`index`)의 답변을 업데이트하고, 업데이트된 배열을 다시 상태로 설정한다.
  - **handleSubmit 함수**
    - 사용자가 제출 버튼을 클릭했을 때 호출되는 함수로, 제출을 막기 위해 `e.preventDefault()`를 사용.
    - `onSubmit` prop으로 `answers` 배열을 전달하고, 페이지를 최상단으로 스크롤하는 동작을 추가.  




### :pushpin: 5-2) MBTI TEST RESULT
- `TestResultList` 컴포넌트는 서버에서 데이터를 불러와 최신순으로 정렬하고, 각각의 결과에 대해 삭제 및 공개 상태를 변경할 수 있도록 구성된 기능을 제공한다.
- `TestResultItem` 컴포넌트는 MBTI 결과를 개별적으로 보여주며, 사용자가 해당 글의 작성자인 경우 삭제 및 공개 여부를 조정할 수 있는 버튼을 제공한다.

![6  mbti-test-result](https://github.com/user-attachments/assets/fb96422b-b977-4936-856d-f4dbabd3bb0a)


```jsx
// components/TestResultList
const TestResultList = () => {  
  const { data, isLoading, isError } = useMbti();

  // custom hooks
  const { mutate: onDeleteMbti } = useDeleteMbti();
  const { mutate: onUpdateMbti } = useVisibilityMbti();


  if(isLoading) return <div>로딩중입니다.</div>
  if(isError) return <div>에러가 발견되었습니다.</div>

  const sortData = data.sort((a, b) => new Date(b.date) - new Date(a.date));   
  
  return (
    <StResultItemWrap>
      {
        sortData.map(list => {
          return (
            <TestResultItem 
              key={list.id}
              data={list}
              onDelete={onDeleteMbti}
              onUpdate={onUpdateMbti}
            />
          )
        })  
      }   
    </StResultItemWrap>    
  )
}
```

- `TestResultList`
  - MBTI 테스트 결과 목록을 화면에 표시하는 컴포넌트이다.
  - API로부터 가져온 테스트 결과를 로딩, 에러 처리와 함께 출력하며, 결과 정렬 및 삭제, 공개 여부 업데이트 기능을 제공한다.
  - ***상태관리 및 데이터 처리***
    - `useMbti` 커스텀 훅을 사용하여 API에서 MBTI 테스트 결과 데이터를 가져온다.
      - `data`: 서버에서 받아온 MBTI 테스트 결과 목록.
      - `isLoading`: 데이터가 로딩 중인 상태를 나타냄.
      - `isError`: 에러 발생 여부를 나타냄.
    - `isLoading`일 때는 "로딩중입니다." 메시지를, isError일 때는 "에러가 발견되었습니다." 메시지를 출력한다.
  - ***정렬***
    - 데이터를 최신순으로 정렬하기 위해 sort 메서드를 사용하여 결과의 date 값을 기준으로 내림차순 정렬한다.
  - ***커스텀 훅 사용***
    - `useDeleteMbti`: 특정 MBTI 테스트 결과를 삭제하는 데 사용하는 훅.
      - `onDeleteMbti` 함수로 데이터를 삭제할 수 있다.
    - `useVisibilityMbti`: 특정 테스트 결과의 공개 여부를 업데이트하는 훅.
      - `onUpdateMbti` 함수로 공개 여부를 변경할 수 있다.
  - ***결과 출력***
    - sortData 배열을 map 메서드로 순회하면서, 각각의 테스트 결과(list)를 TestResultItem 컴포넌트로 전달한다.

---

```jsx
// components/TestResultItem.jsx
const TestResultItem = ({ data, onDelete, onUpdate }) => {  
  const { isUserInfo } = useContext(AuthContext);

  // 공개 글 || 나의 글
  if(data.visibility || isUserInfo) {
    return (
      <StMbtiItem>
        <div className="info">
          <p>
            {data.nickname ? data.nickname : data.userId}        
            {
              isUserInfo.id === data.userId && (
                <span className={data.visibility ? `listState release` : `listState private` }>
                  {data.visibility ? '공개 글' : '비공개 글'}
                </span>
              )
            } 
          </p>          
          <p>{getFormattedTime(data.date)}</p>
        </div>       
        <div className="descBox">
          <span className="descMbti">{data.result}</span>
          <p className="descText minSans" dangerouslySetInnerHTML={{__html: mbtiResult(data.result)}} />
        </div> 
        
        
        {
          isUserInfo.id === data.userId && (
            <div className="btnArea">
              <button onClick={() => onDelete(data.id)}>삭제</button>
              <button onClick={() => onUpdate({id: data.id, vis: data.visibility})}>
                {data.visibility ? '비공개' : '공개'}
              </button>
            </div>
          )
        }      
        
        
      </StMbtiItem>
    )
  }

  return null;

}
```

- `TestResultItem` 
  - MBTI 테스트 결과 개별 항목을 표시하는 컴포넌트이다.
  - props로 `data`, `onDelete`, `onUpdate`를 전달받아 결과 데이터를 출력하고, 삭제 및 공개 여부를 변경할 수 있는 버튼을 제공한다.
  - **유저 정보 및 게시물 접근**
    - `useContext(AuthContext)`를 사용하여 현재 로그인된 사용자의 정보를 가져온다.
    - 게시물이 공개 상태(`data.visibility === true`)이거나, 현재 사용자가 해당 게시물의 작성자인 경우에만 게시물이 화면에 표시된다.
  - **게시물 정보 출력**
    - 닉네임 및 작성 시간
      - 게시물 작성자가 닉네임을 설정한 경우, 닉네임을 표시하고 그렇지 않다면 `userId`를 대신 표시한다.
      - 게시물의 작성 시간은 `getFormattedTime(data.date)` 함수를 통해 포맷팅되어 화면에 출력된다.
  - **게시물 공개 상태 표시**
    - 현재 로그인된 사용자가 해당 게시물의 작성자인 경우, 게시물의 공개 여부를 공개 글 혹은 비공개 글로 표시한다.
    - `data.visibility`에 따라 글이 공개 상태면 '`release`', 비공개 상태면 '`private`'이라는 CSS 클래스를 설정하여 스타일링한다.
  - **게시물 내용 출력**
    - MBTI 결과: `data.result`로 MBTI 결과를 출력하고, 그에 따른 설명을 `mbtiResult(data.result)` 함수로 가져와 `dangerouslySetInnerHTML`을 사용하여 HTML로 표시한다.
    - `dangerouslySetInnerHTML`은 직접적인 HTML 렌더링을 위해 사용되며, 보안에 유의해야 하지만, 여기서는 결과를 설명하는 부분에서 사용된다.
  - **버튼 영역**
    - 현재 사용자가 해당 게시물의 작성자인 경우에만 삭제 및 공개 여부 변경 버튼이 표시된다.
    - 삭제 버튼: `onDelete(data.id)`를 호출하여 게시물을 삭제한다.
    - 공개 여부 변경 버튼: `onUpdate({id: data.id, vis: data.visibility})`을 호출하여 게시물의 공개 상태를 변경한다. 현재 게시물의 공개 상태에 따라 '비공개' 혹은 '공개' 버튼이 표시된다.
  - **조건부 렌더링**
    - 게시물이 공개된 상태이거나(`data.visibility`) 현재 사용자가 해당 게시물의 작성자인 경우에만 전체 컴포넌트가 렌더링되며, 그렇지 않은 경우에는 `null`을 반환해 화면에 아무것도 표시되지 않는다.

## :joystick: Trouble Shooting
- [React에서 템플릿 리터럴 br 적용하는 방법](https://rarrit.github.io/til/react/troubleshooting/react-inner-html/)


## :fire: 회고
MBTI Test 컴포넌트에서 `Tanstack Query`의 커스텀 훅을 사용해서 기능을 구현해봤다. 이전보다 로직을 훨씬 간결하게 처리할 수 있었고 API 호출 시 데이터 처리 흐름을 더 명확하게 할 수 있었다.

### :pushpin: Keep - 현재 만족하고 있는 부분
수준별 분반 수업 때 공부한 `Tanstack Query`와 커스텀 훅을 만들어 적용해봤다.

### :pushpin: Problem - 불편하게 느끼는 부분
`TestListItem` 컴포넌트에서 조건부로 처리되는 부분이 많다. 상위 컴포넌트에서 조건부로 처리되어야 할 로직을 작성 후 해당 컴포넌트에서는 props로 전달받아서 사용하는 방식이 복잡하게 느껴졌다. 특히, 로그인한 사용자와 게시물 작성자 간의 조건 처리나, 글의 공개/비공개 여부에 따른 로직이 중첩되어 가독성이 떨어진다. 이러한 로직이 많아질수록 컴포넌트가 지나치게 복잡해지는 문제가 발생했다.

### :pushpin: Try - problem에 대한 해결책, 당장 실행 가능한 것
복잡한 조건부 로직을 상위 컴포넌트에서 처리하기보다, 관련된 조건을 별도 함수로 분리하여 관리할 수 있을 것이다. 예를 들어, '사용자가 해당 글의 작성자인가?', '공개 여부는 어떻게 되는가?' 등의 로직을 함수로 만들어, 각 컴포넌트 내에서 쉽게 재사용할 수 있도록 하는것과 전역 컨텍스트를 활용하여 `TestListItem` 컴포넌트에서 필요한 데이터만 전달받도록 로직을 수정해봐야겠다.

### :pushpin: 관련 글
- [[1차] 팩트폭행 MBTI 테스트 프로젝트 - 초기 세팅 및 회원 기능](https://rarrit.github.io/react/mini/mbti-project-1/)
- [[2차] 팩트폭행 MBTI 테스트 프로젝트 - MBTI 테스트 기능 적용](https://rarrit.github.io/react/mini/mbti-project-2/)
- [[3차] 팩트폭행 MBTI 테스트 프로젝트 - 리팩토링(API,상태 관리)](https://rarrit.github.io/react/mini/mbti-project-3/)