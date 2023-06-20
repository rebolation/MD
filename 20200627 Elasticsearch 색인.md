---
date: 2020.07.07
title: Elasticsearch 색인
tag: Elasticsearch, 색인, 역인덱스, 텍스트분석, 애널라이저, 캐릭터필터, 토크나이저, 토큰필터, 형태소분석
---

# Elasticsearch 색인

### 색인(Index)

- 색인 : 데이터를 저장하면서 역인덱스를 만드는 것
  - 역인덱스(찾아보기) : 텀별로 텀을 포함하는 문서번호들을 정리해둔 것 
  - 텀 : 데이터로부터 추출한 각각의 키워드

### 텍스트분석

- 텍스트분석(Analysis) : 색인 중에서 특히 문자열 필드를 색인하는 과정. 분석기(애널라이저)를 사용한다.
- 애널라이저(Analyzer) : 텍스트분석을 담당한다. 캐릭터필터(문자치환), 토크나이저(문장분할), 토큰필터(토큰후처리)의 세 가지 요소로 이루어진다. 세 가지 요소로 애널라이저를 구성하는 방법은 커스텀 또는 프리셋 방식이 있다.
  - 캐릭터필터 : 특정 문자를 다른 문자로 바꾸거나 제거
    - html_strip : HTML 태그를 제거
    - mapping : 단어를 다른 단어로 바꿈
    - pattern_replace : 정규식을 이용해 바꿈
  - 토크나이저 : 문장을 토큰으로 분리
    - standard : 공백 기준으로 문장을 분리하고 @ 등의 특수문자와 단어 끝의 특수문자를 제거
    - letter : 공백,숫자,특수문자 기준으로 문장을 분리
    - whitespace : 공백 기준으로 문장을 분리
    - uax_url_email : standard와 유사하나 이메일과 웹사이트주소는 보존
    - nori_tokenizer : 
      - user_dictionary
      - user_dictionary_rules
      - decompound_mode : none, discard(디폴트), mixed
  - 토큰필터 : 각각의 토큰에 대한 가공 (필터들의 적용 순서에 주의)
    - lowercase : 토큰을 소문자로 변환
    - uppercase : 토큰을 대문자로 변환
    - stop : a, the 등의 토큰을 색인에서 제외
    - snowball : jumps, jumping 등 문법적으로 유사한 토큰을 jump로 뭉침
    - synonym : Amazon=AWS 등 유사한 토큰을 하나로 지정
    - nGram: 지정한 길이만큼 단어를 쪼개 텀으로 저장
    - edgeNGram : nGram과 유사하나 단어의 시작부분을 기준으로 쪼갬
    - shingle : nGram의 단어 단위 버전
    - unique : 중복된 텀을 제거
    - nori_part_of_speech : 지정한 품사를 제거 [nori pos tag summary](https://joonable.tistory.com/33)
    - nori_readingform : 한자를 한글로 변환
  - 토큰 : 텍스트분석과정에서 추출된 키워드이며 색인 과정에서 텀으로 저장됨

### 형태소분석

- 형태소분석 : 텍스트분석으로 텀을 추출할 때 기본형(어간)을 분석하여 추출하는 것. 대표적인 형태소분석기는 아리랑, 은전한닢, Open Korean Text, 노리 등이 있다.

- 노리(Analyzer)
  - 엘라스틱서치 공식 한글 형태소 분석기 (설치 : elasticsearch-plugin install analysis-nori)
  - 제공 토크나이저
    - nori_tokenizer
      - user_dictionary : 사용자 사전 저장 경로
      - user_dictionary_rules : 사용자 사전 배열
      - decompound_mode : 합성어 저장 방식
  - 제공 토큰필터
    - nori_part_of_speech : 제거할 품사를 지정
    - nori_readingform : 한자로 된 단어를 한글로

```
PUT <index>
{
  "settings": {
    "analysis": {
      "analyzer": {
        "nori": {
          "tokenizer": "nori_tokenizer"
        }
      }
    }
  },
  ...
}
```

### 인덱스 설정

- settings : 인덱스를 만들 때 적용할 설정
  - number_of_shards, number_of_shards
  - refresh_interval
  - analysis
    - analyzer, char_filter, tokenizer, filter
- mappings : 인덱스에 데이터를 입력할 때 적용할 설정
  - 데이터를 색인할 때 필드의 타입 등을 지정
  - 데이터를 색인할 때 적용할 애널라이저를 지정

### 테스트 API

- _analyze API : 임의의 데이터를 임의의 분석기로 테스트해볼 수 있다.

```
GET _analyze
{
  "text": "sample text for analyze API test",
  "tokenizer": "whitespace",
  "filter": [
    "lowercase",
    "stop"
  ]
}
```

- _termvectors API : 도큐먼트의 역인덱스를 확인할 수 있다.

```
GET <index>/_termvectors/<docId>?fields=<field>
```

### 참고

- [Elastic 가이드북](https://esbook.kimjmin.net/)
- [nori pos tag summary](https://joonable.tistory.com/33)

