---
date: 2020.06.24
title: Elasticsearch 시작
tag: Elasticsearch
---

# Elasticsearch 시작

### 기본개념은 알고 가자

##### NRT(near realtime)

> 문서를 색인화하는 시점부터 문서가 검색 가능해지는 시점까지 약간의 대기 시간(대개 1초)이 있습니다.

##### 클러스터(cluster)

> 클러스터는 하나 이상의 노드(서버)가 모인 것이며, 이를 통해 전체 데이터를 저장하고 모든 노드를 포괄하는 통합 색인화 및 검색 기능을 제공합니다.

##### 노드(node)

> 노드는 클러스터에 포함된 단일 서버로서 데이터를 저장하고 클러스터의 색인화 및 검색 기능에 참여합니다.
>
> ...클러스터 이름을 통해 어떤 클러스터의 일부로 구성될 수 있습니다.

##### 인덱스(index)

> 비슷한 특성을 가진 문서(도큐먼트)의 모음. 인덱스 이름을 사용하여 인덱스에 포함된 문서에 대한 인덱싱, 검색, 업데이트, 삭제 작업 수행.

##### 인덱싱(indexing)

> 문서(도큐먼트)를 검색어 토큰들로 전환하여 검색이 가능한 상태로 만드는 것

##### 검색(search)

> 검색어 토큰을 포함하는 문서를 찾는 것

##### 도큐먼트(document)

> 인덱싱할 수 있는 기본 정보 단위(JSON)

##### 샤드(shards) & 레플리카

> 단일 노드에 방대한 데이터 저장 시 저장장치 용량을 초과하거나 속도가 저하되는 문제를 해결하고자 인덱스를 여러개의 조각으로 나눈 것. 그 자체가 온전한 기능을 가진 독립된 인덱스. 샤딩을 통해 볼륨의 수평 분할/확장이 가능. 성능/처리

##### 레플리카(replicas)

> 샤드의 복사본. 노드가 깨졌을 때를 대비할 수 있고,  볼륨 스케일업을 할 수 있다.

### 윈도우에서 시작하기 - 키바나 없이 시작

윈도우 환경에서 엘라스틱서치 서버를 실행해보자. 기본적으로는 9200 포트에서 구동된다.

```powershell
elasticsearch-7.7.1-windows-x86_64.zip 다운로드 및 압축 해제
elasticsearch-7.7.1/bin/elasticsearch.bat 실행
```

엘라스틱서치는 기본적으로 REST API 를 사용하여 데이터를 다룬다. PUT, POST, GET, DELETE 메소드를 사용한다.

우선, 서버가 잘 구동하는지 브라우저에서 확인해보자. 클러스터이름(기본값인 elasticsearch)과 노드이름(컴퓨터이름)을 확인할 수 있다. 단일 클러스터 내에서 단일 노드를 시작했다는 것을 알 수 있다.

```powershell
http://localhost:9200/
```

크롬 콘솔에서 클러스터 상태를 확인해보자. 클러스터의 상태가 green인 것을 알 수 있다.

```javascript
fetch("http://localhost:9200/_cat/health?v").then(res => res.text()).then(console.log)
```

클러스터의 노드들을 조회해보자. 한 개의 노드가 확인된다.

```javascript
fetch("http://localhost:9200/_cat/nodes?v").then(res => res.text()).then(console.log)
```

클러스터에 존재하는 모든 인덱스를 나열해보자. 아직 아무런 인덱스가 없다.

```javascript
fetch("http://localhost:9200/_cat/indices?v").then(res => res.text()).then(console.log)
```

인덱스를 만들어 보자. REST API를 본격적으로 사용해본다.

```javascript
fetch("http://localhost:9200/customer", { method: 'PUT' })
```

만든 인덱스를 삭제해보자.

```javascript
fetch("http://localhost:9200/customer", {method: 'DELETE',})
```

인덱스에 문서를 추가해보자. 

- 존재하지 않는 인덱스는 자동 생성된다. (customer)
- 문서의 타입을 지정해주어야 한다. (external)
- ID의 경우, POST를 사용하면 자동 생성, PUT을 사용하면 사용자 지정이다.

```javascript
fetch("http://localhost:9200/customer/external/1", {
    method: 'PUT',
    body: JSON.stringify({'name':'코코몽'}),
    headers: { 'Content-Type': 'application/json', },
}).then(res => res.text()).then(console.log)
```

인덱스의 모든 문서를 조회해보자.

```javascript
fetch("http://localhost:9200/customer/_search").then(res=>res.text()).then(console.log)
```

추가한 문서를 검색해보자. 문서를 찾았음을 found 값 true 를 통해 알 수 있다.

```javascript
fetch("http://localhost:9200/customer/external/1").then(res=>res.text()).then(console.log)
```

기존 문서를 새로운 문서로 대체해보자. (인덱스가 read-only 모드인 경우 실패한다)

```javascript
fetch("http://localhost:9200/customer/external/1", {
    method: 'PUT',
    body: JSON.stringify({'name':'뽀로로'}),
    headers: { 'Content-Type': 'application/json', },
}).then(res => res.text()).then(console.log)
```

참고로, 만약 디스크 공간이 부족해서 인덱스가 read-only 모드가 되었다면 다음과 같이 해제해주어야 한다.

```javascript
fetch("http://localhost:9200/_settings", {
	method: 'PUT',
	body: JSON.stringify({ 'index.blocks.read_only_allow_delete': null }),
	headers: { 'Content-Type': 'application/json', },
}).then(res => res.text()).then(console.log)
```

문서를 삭제해보자.

```javascript
fetch("http://localhost:9200/customer/external/1", { method: 'DELETE' })
```

벌크작업을 할 수도 있다. 오버헤드를 줄일 수 있다. 일단 브라우저에서 한 번 해보자. 잘 된다.

```javascript
fetch("http://localhost:9200/customer/external/_bulk", {
    method: 'POST',
    body: '{"index":{"_id":"1"}}\n{"name": "John Doe" }\n{"index":{"_id":"2"}}\n{"name": "Jane Doe" }\n',
    headers: { 'Content-Type': 'application/json', },
}).then(res => res.text()).then(console.log)
```

이번엔 [curl for windows](https://curl.haxx.se/windows/)와 [샘플데이터](https://github.com/elastic/elasticsearch/blob/master/docs/src/test/resources/accounts.json?raw=true)를 다운받아 벌크작업에 사용해보자. 윈도우에서는 큰 따옴표를 쓰자.

```powershell
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/account/_bulk?pretty&refresh" --data-binary "@accounts.json"
curl "localhost:9200/_cat/indices?v"
```

### 리눅스에서 엘라스틱서치

Centos 기반에서 설치해보자.

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.8.0-x86_64.rpm
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.8.0-x86_64.rpm.sha512
(shasum 명령어 없으면) yum install -y perl-Digest-SHA
shasum -a 512 -c elasticsearch-7.8.0-x86_64.rpm.sha512 
sudo rpm --install elasticsearch-7.8.0-x86_64.rpm
```

서버를 실행해보자.

```bash
# 부팅시 자동시작 등록
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service

# 수동시작/중지
sudo systemctl start elasticsearch.service
sudo systemctl stop elasticsearch.service
```

잘 시작되었는지 확인해보자.

```bash
curl "localhost:9200/"
```

노리 플러그인은 아래와 같이 설치하면 된다.

```
/usr/share/elasticsearch/bin/elasticsearch-plugin install analysis-nori
```

아래와 같이 엘라스틱서치를 제거할 수 있다.

```
sudo rpm -e elasticsearch
```

아래와 같이 노리 플러그인을 제거할 수 있다.

```
sudo /usr/share/elasticsearch/bin/elasticsearch-plugin remove analysis-nori
```

### Node Client

node client 를 설치하고 사용해보자

```bash
npm install @elastic/elasticsearch
```

```js
const { Client } = require('@elastic/elasticsearch');
const client = new Client({ node: 'http://localhost:9200' });

const result = await client.search({
	index: 'customer',
	body: {
		"query": { 
			"match": { "_id": 1 }
		}
	}
})
```

인덱스를 해보자. 추가 또는 갱신 중 적절한 것이 수행된다.

```javascript
await client.index({
  index: 'ppost',
  id: id,
  body: {
    mdtitle : frontmatter.title,
    mdcontent : md.content,
    mdfile : name,
  }
})  
```

### 요약

엘라스틱서치는 클러스터 단위로 운영되는데 클러스터는 여러대의 노드(검색서버)로 이루어진다. 노드에는 인덱싱이라는 과정을 통해 문서들이 검색하기 쉬운 형태로 저장되는데, 이렇게 저장된 문서의 그룹을 인덱스라고 부른다. 인덱스는 샤드와 레플리카를 통해 분할/확장이 가능하다. 인덱스를 생성/삭제하거나 문서를 추가/검색/갱신/삭제하는 것은 REST API를 사용한다. 이러한 API를 손쉽게 사용할 수 있도록 Node Client 등을 제공한다.

### 참고

- [Elastic 공식사이트 - 시작하기](https://www.elastic.co/kr/start)
- [RPM 설치](https://github.com/itmare/es)
- [Elastic 가이드북](https://esbook.kimjmin.net/)
- [Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#docs-index-api-request)
- [Get API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)
- [Search API](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html)
- [Query String Syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)
- [공식사이트 한국어문서](https://www.elastic.co/guide/kr/index.html)
- [공식 Node.js client](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/introduction.html)
- [공식 Node.js client 2](https://expressjs.com/ko/guide/database-integration.html#elasticsearch)
- [크몽 검색 기능 개선기](https://brunch.co.kr/@kmongdev/7)