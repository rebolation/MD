---
date: 2020.01.02
title: changelog
tag: changelog
---



# CHANGELOG

### 요구사항

- SEO가 가능해야 한다. → SAPPER

- URL이 단순해야 한다. → SAPPER

- 콘텐츠를 간편하게 관리할 수 있어야 한다. → 마크다운, 깃
  - 타이포라에서 마크다운 문서를 작성하고 깃푸시
  - 서버단에서 마크다운 폴더를 감시(node-watch) -> 변경 감지 -> 마크다운-HTML 변환 수행
  - 브라우저에서 HTML 문서를 조회하고 원본 마크다운 파일을 다운로드 또는 마크다운 에디터로 수정

- 검색 기능이 강력해야 한다. → 엘라스틱서치

### 패키지

+ (base) sapper-template-hot 
+ markdown : marked, prismjs, gray-matter, postcss-import, autoprefixer, node-watch, markdown-toc
+ session : express-session, session-file-store, body-parser
+ tailwindcss : tailwindcss, svelte-preprocess (tailwind in *.svelte file), postcss-cli
+ cache : etag
+ authentication : passport, passport-google-oauth20, passport-facebook, passport-twitter
+ search : @elastic/elasticsearch
+ test : cypress

### 마크다운

- gray-matter : date, title, tag, desc 등을 관리
- markdown-toc(수정) : table of content 생성

```javascript
if(opts.prefix) tok.content = utils.mdlink(text, opts.prefix + '/#' + slug);
```

### 캐시

- 앱쉘(service-worker.js) 리소스 : cache-first

  - 서비스워커 install 단계에서 리소스를 브라우저의 cache storage에 저장
  - 매번 cache storage에서 꺼내서 빠르게 응답

- 목록

  - 

- 본문([slug].json.js) : etag 와 if-none-match 비교

  - 서비스워커 cache-first를 적용할 경우 본문을 수정하더라도 수정 내용을 볼 수가 없으므로 etag 방식 활용
  - 요청 -> 응답 헤더에 etag 추가 -> 2번째 요청 헤더에 if-none-match 자동추가 -> 비교하여 304 또는 200
  - 본문이 한 글자라도 수정되면 즉시 새로운 정보로 대체함

- 본문 이미지(service-worker.js) : postimg 요청은 cache-first

  - 본문과 마찬가지로 etag를 사용하기 위해 sirv etag:true 설정하였으나 2번째 요청 헤더에 if-none-match가 자동추가 되지 않음(Provisional headers 경고). 서비스워커로 캐시하기로 함.
  - postimg의 모든 이미지는 매번 서비스워커의 fetch 단계를 거치게 됨.
  - fetch 단계에서 브라우저의 cache storage에 이미지가 있으면 해당 이미지를 사용하여 빠르게 응답
  - cache storage에 이미지가 없으면 네트워크(서버)로 요청 후 응답을 받아 응답
  - 다른 대안으로 sirv의 maxAge옵션을 사용하는 방법
  - ![1](/postimg/1.png)

### 인증

- passport

### 검색

- elasticsearch

