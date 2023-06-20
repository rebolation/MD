---
date: 2020.06.26
title: Elasticsearch 검색
tag: Elasticsearch
---

# Elasticsearch 검색

### 검색

검색은 GET <인덱스명>/_search 형식으로 할 수 있다. 크게 두 가지 방법이 있다.

- 요청 URI에 검색매개변수(q) 사용 : 루씬 [Query String Syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)를 사용하며, q 파라미터 값으로 검색할 내용을 보내면 된다. 주로 빠른 테스트 용도로 사용.
- 요청 본문에 JSON 사용 : [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/5.4/query-dsl.html)를 사용하며, JSON 형식의 body로 검색할 내용을 보내면 된다. 보다 복잡한 검색이 가능하다. 

```javascript
fetch("http://localhost:9200/bank/_search?q=*&sort=account_number:asc&pretty").then(res => res.text()).then(console.log)
```

```javascript
fetch("http://localhost:9200/bank/_search", {
	method: 'POST', // 브라우저에서는 POST를 사용했음
	body: JSON.stringify(
		{
			"query": { "match_all": {} },
			"sort": [
				{ "account_number": "asc" }
			]
		}),
	headers: { 'Content-Type': 'application/json', },
}).then(res => res.text()).then(console.log)
```

응답 결과가 의미하는 바를 알아보자.

- `took` – 검색 시간(밀리초)
- `timed_out` – 시간 초과 여부
- `_shards` – 검색한 샤드 수 및 검색에 성공/실패한 샤드 수
- `hits` – 검색 결과
  - `hits.total` – 검색 조건과 일치하는 문서의 총 개수
  - `hits.hits` – 검색 결과 배열(기본 설정은 처음 10개 문서)
    - `hits.hits._score` - 스코어 ([정확도](https://esbook.kimjmin.net/05-search/5.3-relevancy))
  - `hits.sort` - 정렬 키(점수 기준 정렬일 경우 표시되지 않음)

검색 결과를 몇 개나 반환할지 조절하려면 size 매개변수를 사용한다. 디폴트는 10개다. 다음은 한 개의 문서를 리턴한다.

```javascript
fetch("http://localhost:9200/bank/_search?q=*&size=1&pretty").then(res => res.text()).then(console.log)
```

페이지네이션을 하려면 from과 size 매개변수를 사용한다. 다음은 10 ~ 19 번 문서를 리턴한다.

```javascript
fetch("http://localhost:9200/bank/_search?q=*&sort=account_number:asc&from=10&size=10&pretty").then(res => res.text()).then(console.log)
```

특정 필드만 조회하고 싶다면 _source 매개변수를 사용한다. 다음은 account_number와 balance만 조회한다.

```javascript
fetch("http://localhost:9200/bank/_search?q=*&_source=account_number,balance&size=10&pretty").then(res => res.text()).then(console.log)
```

### Query String Syntax

Query String Syntax를 여러가지 사용해보자.

```javascript
fetch("http://localhost:9200/bank/_search?q=account_number:1")
fetch("http://localhost:9200/bank/_search?q=firstname:Que*")
fetch("http://localhost:9200/bank/_search?q=firstname:(Que* OR Veg*)")
fetch('http://localhost:9200/bank/_search?q=address:"madison street"')
fetch('http://localhost:9200/bank/_search?q=address:"street madison"~2')
fetch('http://localhost:9200/bank/_search?q=account_number:[1 TO 3]')
fetch('http://localhost:9200/bank/_search?q=account_number:[1 TO 3}')
fetch('http://localhost:9200/bank/_search?q=address:(vi* -place -street -road)')
fetch('http://localhost:9200/bank/_search?q=address:place^2 madison street')
fetch('http://localhost:9200/bank/_search?q=balance:>49900')
```

### Query DSL

Query DSL을 사용해서 쿼리를 해보자.

```javascript
let query = { "query": { "match_all": {} },
  "from": 10, "size": 5, "sort": {"balance":{"order":"desc"}}, "_source": ["balance"],
} // 디폴트는 match_all
let query = { "query": { "match": { "address": "mill lane" } } }; // 디폴트는 OR
let query = { "query": { "match": { "address" : { "query": "mill lane", "operator": "and" }}}}
let query = { "query": { "match_phrase": { "address": "mill lane" } } };
let query = { "query": { "match_phrase": { "address": { "query": "mill lane", "slop": 1 }}}} // 검색어 사이에 1 단어가 끼어드는 것을 허용
let query = { "query": { 
  "query_string": {
    "default_field": "address",
    "query": '(mill AND lane) OR "street"' }}}
let query = {
  "query": {
    "bool": {
      "must": [{"match":{"address":"mill"}},{"match":{"address":"lane"}}] }}}
let query = {
  "query": {
    "bool": {
      "should": [{"match":{/*...*/}},{"match":{/*...*/}}] }}}
let query = {
  "query": {
    "bool": {
      "must": [{"match":{"age":"40"}}],
      "must_not": [{"match":{"state":"ID"}}] }}}
let query = {
  "query": {
    "bool": {
      "must": {"match_all":{}},
      "filter": {"range":{"balance":{"gte":20000,"lte":30000}}} }}}

fetch("http://localhost:9200/bank/_search", {
	method: 'POST', headers: { 'Content-Type': 'application/json', },
	body: JSON.stringify( query );
});
```

### 알아두면 좋은 것

- 검색시 정렬을 별도로 지정하지 않으면 스코어 순으로 정렬된다.
- should를 사용해 스코어를 조절할 수 있다.
- query context와 filter context를 구분한다고 하니 잘 기억해두자.
- 검색은 기본적으로 하나의 인덱스를 대상으로 하지만, 여러 개의 인덱스를 대상으로 할 수도 있다. `GET <인덱스1>,<인덱스2>/_search`  또는 `GET <인덱*>/_search` 와 같은 형식을 사용한다. `GET _all/_search` 방법도 가능하지만 부하가 크다고 한다.
- 엘라스틱서치의 인덱싱 과정에서 데이터는 검색어 토큰인 Term으로 나뉘어진다. 

### 효율적인 검색을 위해 인덱싱 단계에서부터 준비하자

analyzer 를 잘 활용하면 보다 세세하게 검색할 수 있는 것 같다. 엘라스틱 커뮤니티 엔지니어가 작성한 [Elasticsearch 에서 한글 형태소 분석 잘 해보기](http://kimjmin.net/2019/08/2019-08-how-to-analyze-korean/)를 잘 읽어보도록 하자.

[ElasticSearch 에서 wildcard 쿼리 대신 ngram을 활용하는 방법](https://findstar.pe.kr/2018/07/14/elasticsearch-wildcard-to-ngram/)에 따르면 RDB의 LIKE '%...%' 와 같은 결과를 얻기 위해 [n-gram](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-ngram-tokenizer.html)을 사용하면 효과적이라고 한다. 검색 속도는 빨라지고, 인덱스 크기는 커지는 것 같다. 유사한 토크나이저인 [edge n-gram]([ngram](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-edgengram-tokenizer.html))은 단어의 첫 글자 위주로 토큰화하는 것 같다.

### 집계(애그리게이션)

_search에는 집계(aggs) 기능이 있으며, 시각화 등에 사용할 수 있다.

- metrics : min, max, sum, avg, stats, cardinality, percentiles, percentile_ranks
- bucket : range, histogram, date_range, date_histogram, terms
- sub : 버킷 내부에 다시 `"aggs" : { }` 를 선언
- pipeline : min_bucket, max_bucket, avg_bucket, sum_bucket, stats_bucket, moving_avg, derivative, cumulative_sum

### 참고

- [검색 API](https://www.elastic.co/guide/kr/elasticsearch/reference/current/gs-search-api.html)
- [filter, must_not, must](https://stackoverflow.com/a/58515343)
- [정확도](https://esbook.kimjmin.net/05-search/5.3-relevancy)
- [Elastic 가이드북](https://esbook.kimjmin.net/)
- [query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)
- [Elasticsearch 에서 한글 형태소 분석 잘 해보기](http://kimjmin.net/2019/08/2019-08-how-to-analyze-korean/)
- [ElasticSearch 에서 wildcard 쿼리 대신 ngram을 활용하는 방법](https://findstar.pe.kr/2018/07/14/elasticsearch-wildcard-to-ngram/)
- [n-gram](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-ngram-tokenizer.html)
- [edge n-gram]()