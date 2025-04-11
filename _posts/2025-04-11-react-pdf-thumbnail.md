---
title: "[React] PDF 파일을 이미지(JPG/PNG) 썸네일로 변환 후 렌더링하는 방법 (pdfjs-dist)"
date: 2025-04-11
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - react
tags:
  - react pdf
  - pdf thumbnail
  - pdfjs-dist
author_profile: true
---

## :ledger: [React] PDF 파일을 이미지(JPG/PNG) 썸네일로 변환 후 렌더링하기 (pdfjs-dist)

어드민에서 업로드한 파일을 프론트에 노출시키는 과정에서, 이미지가 노출되지 않는 이슈가 생겼다. <br/>
확인해보니 프론트에서는 `img`태그를 사용하고 있는데, 어드민에서 PDF를 업로드하면 PDF 파일은 `<img src="...">` 태그로 직접 렌더링되지 않기 때문에, **썸네일 이미지로 미리보기**를 구현해야 했고 이를 **클라이언트에서 이미지로 변환하여 보여주는 방식**으로 해결했다. `React + Vite` 환경에서 안정적으로 PDF 렌더링을 하기 위한 과정과 주요 이슈, 해결방법을 아래에 정리한다.

---

### :one: 문제 상황

- 어드민에서 업로드된 파일 목록 중 일부가 `.pdf` 확장자
- 사용자 입장에서는 썸네일 이미지가 떠야 UX가 자연스럽다.
- PDF 파일은 `<img src="...">` 태그에서 미리보기가 되지 않아, **PDF를 이미지로 변환**하는 방법이 필요했다.

---

### :two: 사용한 도구 및 기술

#### :pushpin: 2-1) 라이브러리 `pdfjs-dist`
`pdfjs-dist`는 `Mozilla`에서 만든 `PDF.js`의 배포용 NPM 패키지로, 클라이언트 사이드에서 PDF 렌더링을 가능하게 해주는 라이브러리다.<br/>
이 라이브러리는 브라우저 상에서 PDF 파일을 직접 해석하고, 특정 페이지를 `<canvas>` 위에 렌더링할 수 있다.

```bash
# npm
npm install pdfjs-dist

# yarn
yarn add pdfjs-dist
```

---

### :three: PdfThumbnail 컴포넌트 구현
React 컴포넌트 내부에서 PDF의 첫 페이지를 `<canvas>`에 렌더링하고, 해당 이미지를 Base64로 추출해 `<img>`에 삽입한다.

#### :pushpin: 3-1) Vite 환경에서의 워커 설정
`pdfjs-dist`는 내부적으로 Web Worker를 사용하는데, Vite 환경에서는 이를 명시적으로 설정해줘야 한다.
아래와 같이 워커 파일을 직접 import하고 설정해준다.

- `?worker`는 Vite가 이 파일을 Web Worker로 처리하도록 하는 특별한 쿼리 파라미터다.

```jsx
// PdfThumbnail.jsx
import * as pdfjsLib from 'pdfjs-dist';
import pdfjsWorker from 'pdfjs-dist/build/pdf.worker.min?worker';

pdfjsLib.GlobalWorkerOptions.workerPort = new pdfjsWorker();
```

#### :pushpin: 3-2) PdfThumbnail.jsx
이 컴포넌트는 다음과 같은 상황에 활용된다.

- 썸네일 이미지로 사용할 때
- PDF 파일을 즉석에서 렌더링해야 할 때
- 프리뷰 용도로 일부 페이지만 보여줄 때

```jsx
import { useEffect, useRef, useState } from "react";
import * as pdfjsLib from "pdfjs-dist";
import pdfjsWorker from "pdfjs-dist/build/pdf.worker.min?worker";

pdfjsLib.GlobalWorkerOptions.workerPort = new pdfjsWorker();

const PdfThumbnail = ({ pdfUrl, alt, className }) => {
  const canvasRef = useRef(null);
  const [imageUrl, setImageUrl] = useState(null);

  useEffect(() => {
    const renderPDF = async () => {
      try {
        const loadingTask = pdfjsLib.getDocument(pdfUrl);
        const pdf = await loadingTask.promise;
        const page = await pdf.getPage(1);
        const viewport = page.getViewport({ scale: 1.5 });

        const canvas = canvasRef.current;
        const context = canvas.getContext("2d");
        canvas.width = viewport.width;
        canvas.height = viewport.height;

        await page.render({ canvasContext: context, viewport }).promise;

        const dataUrl = canvas.toDataURL();
        setImageUrl(dataUrl);
      } catch (error) {
        console.error("PDF 렌더링 실패:", error);
      }
    };

    renderPDF();
  }, [pdfUrl]);

  return (
    <>
      {imageUrl ? (
        <img src={imageUrl} alt={alt} className={className} />
      ) : (
        <p>로딩 중...</p>
      )}
      <canvas ref={canvasRef} style={{ display: "none" }} />
    </>
  );
};

export default PdfThumbnail;
```

#### :pushpin: 3-3) img, pdf 조건 처리
기존의 이미지처리와 pdf일 경우 처리는 상위 컴포넌트에서 처리하면 된다.

```jsx
// 파일이 PDF인지 확인하는 함수
const isPdf = (filePath) => {
  return filePath?.toLowerCase().endsWith(".pdf");
};

// 사용 예시
{isPdf(selectedResource.activeFile.path) ? (
  <PdfThumbnail
    pdfUrl={convertSrcPath(selectedResource.activeFile.path)}
    alt={selectedResource.title}
    className="w-full rounded-[8px]"
  />
) : (
  <img
    src={convertSrcPath(selectedResource.activeFile.path)}
    alt={selectedResource.title}
    className="w-full rounded-[8px]"
  />
)}
```

---

### :four: 사용 후기
PDF 파일을 웹에서 이미지처럼 다루기 위해선 단순한 `<img>` 태그로는 불가능하다.<br/>
`pdfjs-dist`를 사용하여 PDF를 클라이언트에서 직접 파싱하고 이미지로 변환해보았는데, 다음과 같은 사항에 유의해야 한다.

- 무거운 PDF는 변환하는 과정에서 성능 문제가 생길 수 있다.
- Vite에서는 반드시 워커를 명시적으로 설정해야 한다.
- 외부 PDF 경로가 CORS를 만족해야 렌더링이 가능하다.

해당 방식은 리액트 프로젝트 내에서 유연하고 커스터마이징이 쉬운 썸네일 구현을 가능하게 해주며, PDF를 직접 다운로드하거나 외부 뷰어를 쓰지 않아도 된다는 장점이 있다.<br/>
해당 방법으로 이슈를 해결했으나, 가능하다면 어드민에서 등록할 때 썸네일란을 추가하거나, 서버에서 PDF파일을 이미지로 변환 후 프론트에 전달하는 방식이 좋을 것 같다. (성능 이슈)