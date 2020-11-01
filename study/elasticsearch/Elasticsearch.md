## Basic for elasticsearch



내가 생각하는 4차 산업혁명

```
3차 산업혁명에 의해 쌓아온 데이터를 통해 사물들이 기기와 기기를 통해 연결되는 초연결성 환경 속에서 사용자가 원하는 형태의 제품과 서비스가 빠르게 제공되는 환경이 바로 4차 산업혁명이라고 생각합니다. 4차 산업혁명으로 인해서 초연결성과 지능화로 인한 개인맞춤형 제품 및 서비스가 제공되고, 신산업과 전통산업이 융합되면서 수많은 직업이 생겨나거나 없어질 수 있습니다. 특히 기술 발전이 빠르게 이뤄지는 환경 속에서 데이터 분석과 AI 에 대한 학습은 이제는 선택이 아닌 필수라고 생각합니다.
```



Summary

* 데이터 사이언스 - 정형 + 비정형 등 다양한 데이터를 통해 지식 및 인사이트 추출하기 위한 과학적 방법론, 프로세스, 알고리즘, 시스템 등 동원하는 융합 분야
* 데이터 분석 - 의사결정을 위해 데이터 수집, 변환, 분류, 특징 찾아 한눈 파악을 위해 시각적 표현하기 위한 모든 행위
* 빅데이터 관리 - 빅데이터 수집, 공유, 저장, 처리, 분석, 시각화 메커니즘



## Apache Lucene, Elasticsearch



> Lucene 은 전문 검색 색인 및 검색 기능을 위해 모든 응용프로그램 및 웹 검색 엔진, 로컬 단일 사이트 검색 구현에도 유용함.

> Logstash 는 실시간 파이프라인 기능을 가진 오픈소스 데이터 수집 엔진. 다른 타입 데이터를 탄력적으로 통합하고 사용자가 원하는 형태의 데이터로 정규화하는 역할



**Apache Lucene**

* Java 언어로 된 `정보 검색` 라이브러리 오픈소스
* 전문 검색(Full Text) 색인 및 검색 기능 필요로 하는 프로그램 적합
* 웹 검색 엔진 및 로컬 단일 사이트 검색 구현
* **Core Architecture : 텍스트를 가진 필드를 포함하는 문서 개념**
* 색인 및 검색 제공 라이브러리



**Apache Lucene Project**

* 아파치 너치 : 웹 크롤러 및 HTML 구문 분석 제공
* 아파치 솔라 : Enterprise 검색 서버
* Compass : Elasticsearch 전신
* CrateDB : Lucene 기반 분산 SQL DB
* DocFetcher : 크로스 플랫폼 데스크톱 환경 검색 App
* Elasticsearch : 2010 년 Enterprise 서버



**ELK**

* **Elasticsearch, Logstash, Kibana 오픈소스**
* Elasticsearch : 검색 및 분석 엔진
* Logstash : 실시간 데이터 파이프라인, Elasticsearch 로 전송하기 위한 데이터 처리 파이프라인
* Kibana : Elasitcsearch 데이터를 활용하여 차트, 그래프를 통한 시각화 툴



**ELK 장점**

* Free Schema : Key value 형식
* 인덱스 와일드카드
* Scale-Out Database Clustering
* 실시간 데이터 처리 : 메시지 큐(MQ) 와 결합하면 강력한 실시간 데이터 처리 수집 및 처리 시스템



**Elasticsearch**

* **JSON 기반 분산형 오픈소스 RESTful 검색 엔진**
* 텍스트, 숫자, 위치 기반 정보, 정형 및 비정형 데이터 등 모든 유형 데이터를 위한 분산형 오픈소스 검색 엔진
* **Lucene 기반 분산 검색엔진**, Logstash 를 통해 수신된 데이터를 저장소에 저장하는 역할
* 예상되는 항목 검색 및 예상치 못한 항목 검색
* 정형, 비정형, 위치정보, 메트릭 등 원하는 방법으로 다양한 유형 검색 및 수행 결합 가능



**Logstash**

* Backend 오픈 소스 데이터 처리 파이프라인
* 다양한 소스에서 동시 데이터 수집, 변환하여 stash 보관소 저장
* 데이터 집계, 처리, 결과물을 Elasticsearch 에 전송
* 다양한 고급 다운스트림 분석 및 시각화 활용 사례를 위해 모든 데이터 정리, 대중화(democratization)
* 데이터 수집, 강화, 변환 + Elasticsearch 에서 색인 가능하도록 전송하는 역할 담당
* 어떤 유형의 이벤트도 다양한 입력, 필터, 출력 플러그인을 통해 강화, 전환 가능하며 기본 제공되는 여러 코덱으로 수집 프로세스 간소화



**Logstash**

* 수평 확장 가능
* 플러그형 파이프라인 Architecture : 다양한 입력, 필터, 출력
* 확장 가능하고 개발자에게 편리한 플러그인 에코 시스템



**Beats**

* 서버 Agent 형식, 오픈소스 경량 데이터 수집기
* 수백 수천개 장비와 시스템으로부터 Logstash 나 Elasticsearch 에 데이터 전송
* 다양한 종류 데이터들을 서버에서 다른 곳으로 전송하는 프로그램



**ELK Example**

* App 검색
* 웹 사이트 검색
* 로깅 및 로그 분석
* Infra Metrics and Container Metrics
* App 성능 Metrics
* 위치 기반 정보 데이터 분석 및 시각화



## Elasticsearch Details

**Goal**

* 기본 개념
* 아키텍처, 클러스터 구성 요소
* REST API



**기본 개념**

* Elasticsearch 는 HTTP RESTful API 지원한다.
* Index 는 관계형DB 에서 Database 를 의미한다. 여러 노드에 분산 저장 및 관리되고, 내부에는 색인된 데이터들이 물리적 공간에 여러 부분들로 존재함. 이 부분들이 Shard
* Near Realtime
* Cluster : 전체 데이터 함께 보유하는 모든 노드에서 연합 인덱싱 및 검색 기능을 제공하는 하나 이상의 노드 모음
* Node : Cluster 의 일부, 데이터 저장하고 클러스터의 인덱싱 및 검색 기능을 하는 단일 Server
* Index : 유사한 특성을 갖는 문서들의 집합
* Type : Index 내 하나 이상의 Type 정의
* Document : Index 를 생성 할 수 있는 기본 정보 단위, JSON 표현
* Shards : Index 를 여러 조각으로 분할 가능, 기본 4개 제공
* Replication : 복제본 개념이며, 장애 발생할 경우 고가용성 제공, 기본 1개 제공



**관계형 DB 와의 비교**

* index vs database
* type vs table
* document vs row
* field vs column
* mapping vs schema
* everything is indexed vs index
* query dsl vs sql



**Node 구성 요소**

* Primary Shard
  * Index
  * Type(Table)
  * Mapping(Schema)
  * Document(Row)
    * Field(Column)
    * name value
* Replica Shard
* Gateway(Cluster 상태 관리)



**RESTful API 기본 구조**

* Methods
  * POST : 등록
  * PUT : 수정 or 등록
  * DELETE : 삭제
  * GET : 조회
* URI(Uniform Resource Identifier)
  * http://localhost:9200/index/type/id?parameters
  * index : database
  * type : table
  * id : 레코드에 해당되는 document id
  * index, type, id 를 여러 개 저장할 경우 "," 를 사용하여 구분
  * `*` 모두 지정 가능
* Action
  * 상세 관련 내용은 Google참고
* Action 2
  * _bulk : bulk 색인 작업
  * _count : 문서 count 작업
  * _settings : elasticsearch.yml 설정된 global settings 조회
* Document API
  * index API : -XPUT 등록, -XPOST : 등록
  * get API : -XGET
  * delete API : -XDELETE
  * update API : -XPUT
* Search API
  * q : Query String Query
  * analyzer
  * default_operator : OR, AND
  * _source=false
  * df : 디폴트 필드
* Example APIs
  * Google 참고
* Bulk API
  * 여러 문서 등록, 수정, 삭제
  * Google 참고
* REST
  * Representational State Transfer 의 약자, HTTP URI 를 통해 자원을 명시, HTTP Method 를 통해 해당 자원에 대한 CRUD Operation 적용하는 것.
* 반전된 인덱스
  * 문서에 나타나는 모든 고유 단어 목록을 만들고, 각 단어가 발생하는 모든 문서 식별

**Summary**

* 클러스터, 노드, 인덱스, 샤드와 같은 개념
* 클러스터는 여러 개 노드로 구성, 노드 내부에는 인덱스가 있음
* 인덱스 내부는 여러 개 샤드로 쪼개진 데이터들이 있고, 샤드 내부에는 문서가 있고, 문서는 필드와 Mapping 을 갖고 있음.
* 자원별로 고유 URL 로 접근 가능, HTTP Method(GET, POST, PUT, DELETE)로 처리 가능



## Logstash Details

**Goal**

* 실시간 데이터 파이프라인

* 구성 요소

* Configuration 파일 및 데이터 파이프라인 설정

  

**Contents**

* Logstash 기본 개념
* Logstash 구성 요소 이해
* Config file
* 데이터 파이프라인 설정



**Basics**

* 오픈소스 데이터 수집 엔진(Data Flow Engine)
* 다양한 플러그인이 있어 다양한 아키텍처에서 손쉽게 데이터 수집, 처리, 전달 가능
* 프로세스는 하나 이상의 파이프라인
* 각 파이프라인에서 하나 이상 입력 플러그인이 내부 대기열에 배치된 데이터를 수신하거나 수집.
* 기본적으로 작은 메모리에 보관되지만, 안정성 + 복원력을 위해서 디스크에서 영구 구성 가능
* 다양한 데이터 통합하고 정규화해서 필요한 부분 데이터를 filtering and streaming
* **포맷과 스키마** 에 관계없이, 모든 데이터 수집, 강화, 통합하기 위한 Elastic Stack의 중앙 데이터 Flow Engine
* **INPUTS -> FILTERS -> OUTPUTS**
* Beats -> Logstash Pipeline -> Elasticsearch



**Pipeline**

* 하나 이상 구성 파일 기반 생성
* 단일 구성 파일을 사용한 파이프라인 (-f 파라미터 사용)
* 여러 구성 파일을 사용한 파이프라인 (logstash.yml 파일 사용)
* 여러 파이프라인 (pipelines.yml 파일 사용)
* 데이터 처리를 위한 logstash 설정
* 3가지 processor(Input, Filter, Output)으로 구성되고 순차적 실행됨



**Plugin**

* 각 단계가 Input, Filter, Output 순으로 실행됨
* 예시로, Input 은 File Plugin, Filter 는 JSON Plugin, Output 은 Elasticsearch plugin
* 파일로부터 데이터를 가져와서 Elasticsearch 에 저장 가능한 Json 형태로 변환 후 Elaticsearch 로 indexing 하게됨
* 각 단계를 조합하는 Plugin 방법은 유사



**Input**

* 특정 소스 이벤트(데이터)를 가져옴
* Logs & Files, Metrics, Wire Data, Web Apps, Data Stores 등 수집 지원
* Beats 와 결합해서 쓰는 것도 가능



**Filter**

* log parsing, 데이터 확장, 태그 추가 등 데이터 변형 작업
* 필수는 아님
* **Example Plugins**
  * grok : 문자열 데이터에서 필요 데이터 정규식 처리
  * mutate : 데이터 필드 단위 변형, 이벤트 필드에서 일반적 변환 수행
  * date : String 을 Date 타입으로 변환(날짜)
  * clone : 이벤트 복사본 생성
  * json : input 의 JSON 데이터 구조 유지
  * geoip : ip 주소 지리적 위치



**Output**

* 데이터 전송 목적지, 가공 데이터를 저장소에 적재할 경우 Output Plugin 설정
  * Elasticsearch : 이벤트 데이터를 전송
  * file : 디스크 파일에 쓰기
  * graphite : graphite 전송(metric 을 저장하고 그래프로 작성하는데 사용되는 오픈소스 도구)
  * statsd : 카운터 및 타이머와 같은 통계 수신, UDP 를 통해 전송, 백엔드 서비스에 집계를 보내는 서비스



**Codec**

* 데이터 인코딩 & 디코딩, 메시지를 쉽게 구분하고 전송 가능하도록 Input, Output 단계에서 사용되는 Stream Filter
* json : 데이터 인코딩, 디코딩
* multiline : 여러 줄 텍스트 이벤트를 단일 이벤트로 병합



**Config file**

* 2가지 종류
  * 파이프라인 정의
  * logstash 시작 및 실행 관련 옵션 설정 파일
* logstash.yml : logstash 실행 관련 설정 파일
* pipelines.yml : 단일 logstash 인스턴스에서 여러 개 파이프라인 실행 시 사용

```yml
- pipeline.id: my-pipeline_1
  path.config: "/etc/path/to/p1.config"
  pipeline.workers: 3
  ...
```





* jvm.options : JVM 설정. 힙 사이즈 조절 및 GC 관련 옵션
* log4j2.properteis : log4j 관련 설정 파일



* config/test.conf
  * 필터는 선택 사항

```yml
input {
  file {
    path => {"/home/logstash/testdata.log"}
    sincedb_path => "/dev/null" -> 각 입력 파일 내 데이터 추적
    start_position => "beginning" -> 새 파일 발견 시, 처음부터 파일 읽도록
  }
}
filter {

}
output {
  stdout {
    codec => rubydebug -> 디버깅
  }
}
```



* csv file 분석 예시
  * csv 파일을 읽어서 elasticsearch 에 전송

```yml
input {
  file {
    path => {"/path/to/stock-data.csv"}
    sincedb_path => "/dev/null" -> 각 입력 파일 내 데이터 추적
    start_position => "beginning" -> 새 파일 발견 시, 처음부터 파일 읽도록
  }
}
filter {
  csv {
    separator => ","
    columns => ["Date","Open","High","Low","Close","Volume","Adj Close"]
  }
  mutate {
    convert => {
      "Open" => "float"
      "High" => "float"
      "Low" => "float"
      "Close" => "float"
      "Volume" => "float"
      "Adj Close" => "float"      
    }
  }
}
output {
  elasticsearch {
    hosts => "127.0.0.1:9200"
    index => "stock-%{+YYYY.MM.dd}"
  }
  stdout {
  
  }
}
```



## Kibana Details

* 모니터링
* 디스커버
* 비주얼라이즈
* 대시보드



**monitoring**

* elasticsearch 에 있는 index 데이터를 검색하고 확인하는 상호 작용 수행
* 손쉽게 고급 데이터 분석 수행, 다양한 차트 + 테이블 + 지도 형태로 데이터 시각화
* 간단한 브라우저 기반 인터페이스, Elasticsearch Query 변경 사항을 실시간 표시 가능한 대시보드 
* 사용자 인터페이스 행위 발생
* Elasticsearch 가 분석 및 처리하고, kibana 가 이를 렌더링하는 웹 어플리케이션
* Elasticsearch 집계에 시각 능력을 부여해서 시계열 데이터 집합, 데이터 필드 일부분을 쉽게 원형 차트 생성
* index document 를 통해 데이터 발굴
* elastic stack 모니터링도 가능.
* document 간 관계 생성도 가능.
* metric 분석도 가능
* 핵심 기능 : discover, visualize, dashboard, console
* 플러그인 : timelion, graph, monitoring, elastic, logout



**discover**

* 어떤 데이터를 색인하면 데이터 탐색을 위해 가장 먼저 확인하는 메뉴
* 도큐먼트 상세 가능
* 선택된 index pattern 과 일치하는 time picker 기간 내 모든 데이터 조회 가능
* query bar 를 통한 검색, `add filter`를 통한 데이터 필터링
* 조회된 데이터는 Histogram 과 Document Table 에서 확인 가능



**visualize**

* 시간시각화 : 막대그래프, 누적막대그래프, 점그래프
* 분포시각화 : 파이차트, 도우넛파트, 트리맵, 누적연속그래프
* 관계시각화 : 스캐터플롯, 버블차트, 히스토그램
* 비교시각화 : 히트맵, 스타차트, 평행좌표계, 다차원척도법
* 공간시각화 : 지도 매핑



**dashboard**

* 사용자가 배치하고 공유 가능한 여러 시각화의 모음



**Quiz**

* elasticsearch 쿼리 변경사항을 실시간으로 표시하는 동적 대시보드 빠르게 생성 및 공유 가능
* kibana 색인 패턴 : elasticsearch 에 이미 데이터가 있다면, 사용자는 색인 패턴 생성 필요
* kibana는 데이터 계층 추상화하는 용도로만 사용. 데이터 계층에 영향 주지 않으며 사용자가 데이터를 가지고 있지 않으면 CSV Importer 를 사용해 입력 가능하다.



### Kibana Details 2

**Goal**

* Aggregation(집계)
* Time Lion(시계열 데이터 분석기 - 다양한 visualizing 제공)
* Dev Tools(웹 기반 Console 기능 제공)



**Aggregation(집계)**

* Elasticsearch 에 있는 데이터를 그룹화하고 통계치를 얻는 기능

* 검색 쿼리 기반 데이터 제공 - 집계 프레임워크

* 문서 세트에 대한 분석 정보를 구축하는 작업 단위

* RDBMS 의 Group by 와 유사

* Metric 집계, Bucket 집계, 파이프라인 집계, 누적 카디널리티 집계

  

Metric 집계

* 도큐먼트 계산해서 처리한 값
* SQL의 `Select avg(score) FROM results;` 와 비슷
* 사용 계산식은 평균, 최소, 최대값 등
* **Stats 집계** : 단일 요청에서 **Document 합계, 평균, 최대, 최소 개수 계산**
* **Extended Stats 집계** : stats 결과에 추가로 통계 정보를 더해 반환
* **Cardinality(카디널리티) 집계** : 특정 요소 개수를 찾는데 유용



Metric 집계 구조 example

```
GET /{index_name}/_search?pretty
{
	"size": 0,
	"aggs": {
		"{metric_args_type}"
	}
}
```



Bucket 집계

* 조건에 해당되는 도큐먼트들을 Bucket 이라는 저장소 단위로 구분해 담아 새로운 집합 생성
* SQL Group by 와 유사
* ex. 특정 요일 별 시간 간격 변경하기



Bucket 집계 구조

```
GET /{index_name}/_search?pretty
{
	"size": 0,
	"aggs": {
		"{bucket_args_type}" : {
			"field": "{file_name}",
			"order": {
				"{field_name}": "desc"
			}
		}
	}
}
```



Pipeline 집계

* 다른 집계 결과를 그대로 다시 집계하는 방법
* 다른 집계 버킷을 참조해서 집계 수행
* 집계 혹은 중첩된 집계를 통해 생성된 버킷으로 추가 계산 수행 가능
* buckets_path 지정 필수
* 부모 집계는 집계를 통해 생성된 버킷을 사용하여 계산 수행, 그 결과를 기존 집계 결과에 반영
* 형제 집계는 동일 선상 위치에서 수행되는 새 집계



Pipeline 집계 구조 

```
GET /{index_name}/_search?pretty
{
	"size": 0,
	"aggs": {
		"{parent_aggs_type}" : {
			"{bucket_type_name}" : {
				"field": "{file_name}",
			},
		}
		"{aggs_name}": { // 집계 이름 사용자 정의
			"{metric_aggs_name}" : { // 매트릭 집계 타입
				"field": "{file_name}", // 매트릭 집계 대상 필드
			},		
		}
		"{pipe_aggs_name}": { // 파이프라인 집계 이름
            "{pipe_args_type}" : { // 파이프라인 집계 타입
				"buckets_path": "{parent_aggs_name}>{aggs_name}" // 경로
            }		
		}		
	}
}
```



Time Lion

* 서로 다른 데이터를 결합하여 하나의 시계열에서 시각화할 수 있는 도구



Dev Tools

* Elasticsearch - http 프로토콜로 접근 가능한 RESTful API 지원
* kibana 에서 elasticsearch 의 RESTful API 호출 가능
* 자원 별로 http 메소드 GET, POST, PUT, DELETE 를 이용하여 자원 처리
* RESTful 시스템 데이터 처리
  * 입력 : PUT 
  * 조회 : GET
  * 삭제 : DELETE
* kibana 에서 install x-pack 하면 search profiler 가 자동 활성화





### Beats

* 데이터 수집 플로우
* Beats 역할
* X-Pack



Beats는 수백, 수천 개 장비와 시스템으로부터 Logstash or Elasticsearch 에 데이터 전송

Beats : Elasticsearch Service 로 전송할 수 있게 해주는 경량 데이터 수집기

예시로 IoT 장치와 같이 하드웨어 리소스가 제한되어 있는 장치에서 사용



### Beats 제품군

**Filebeat**

* 가장 많이 사용
* 파일 형식으로 제공되는 소스로부터 데이터 읽고 전처리 수집
* Non Binary 파일 형식 지원
* Apache, MySQL, Kafka 등 일반 애플리케이션을 위한 로그 형식 수집 및 구문 분석 가능

**MetricBeat**

* 서비스 메트릭을 수집하고 전처리
* CPU, Memory, Disk, Network 에 대한 정보 포함
* 모듈은 Kafka, Redis 외에도 여러 다른 서비스로부터 데이터 수집하기 위해 제공

**PacketBeat**

* App 모니터링, 보안, 네트워크 성능 분석 활성화

**WinlogBeat**

* App 이벤트, 하드웨어 이벤트, 보안 및 시스템 이벤트 등 이벤트 로그 캡처 수행
* 활용도 높은 편

**AuditBeat**

* 중요 파일 변경사항 탐색, Linux Audit Framework 로부터 이벤트 수집
* 보안 분석 파트에서 많이 사용

**HeartBeat**

* 검색 사용해서 시스템, 서비스 가용성 모니터링
* 인프라 모니터링, 보안 분석 등 수많은 시나리오에서 유용
* 지원되는 프로토콜 ICMP, TCP, HTTP

**FunctionBeat**

* AWS Lambda 와 같은 서버리스 환경 내 로그 및 메트릭 수집



### X-Pack(Extension For the Elastic Stack)

* 보안, 알림, 모니터링, 보고, 그래프, 머신러닝 기능 활용을 위한 편리 단일 패키지 Bundle
* Elastic Stack(Elasticsearch, Kibana) 과 쉽게 통합 가능

* Security : 사용자, 권한 기반 인증 및 통신 암호화 제공

* Alerting : 쿼리 기반 자동 알림

* Monitoring : ES 클러스터 상태 모니터링

* Graph : 관계도 분석 제공

* Reporting : Kibana 대시보드를 PDF 로 내려받거나 CSV 파일로 저장

* Machine Learning : 시계열 데이터 기반 실시간 이상징후 탐지 기능

  

**용어 정리**

* Log Aggregator : 웹 서버 로그, 웹 로그 등 각종 로그 데이터 수집
* RDB Aggregator : RDBMS 정형 데이터를 HDFS 나 NoSQL 전송
* Filebeat : 모든 종류의 넌바이너리 파일 형식 지원됨
* Packetbeat : 라이브 네트워크 데이터 수집 및 전처리



### 클러스터

* 무중단 운영 롤링 리스타트
* 안정적 성능 제공을 위한 샤드 분배
* index setting
* 미리 정의된 template 인덱싱



**롤링 리스타트**

* 무중단 운영을 위한 작업 방법
* 시스템 작업 혹은 Elasticsearch version upgrade 해야 되는 상황에서 사용
* replica 가 있는 cluster 경우, 할당되지 않은 샤드들이 기본 라우팅 설정에 따라 복구를 위해 자동 재분배
* 많은 노드 작업 시, 샤드들이 재분배되기를 기다렸다가 cluster 가 green 상태가 될 때까지는 네트워크 혹은 disk i/o 등 많은 리소스 필요
* 위와 같은 작업 시, 리밸런싱이 일어나지 않도록 하는 것이 **롤링 리스타트**
* _cluster/settings 의 cluster.routing.allocation.enable 값 변경에 따른 설정 변경



**롤링 리스타트 이슈**

* 패치 및 업그레이드 시 이슈사항 예방
* 한 대 씩 업데이트 및 리스타트 시 "전체 클러스터"의 샤드 재분배 작업 발생
* 노드 재실행 시 2번의 클러스터 샤드 재분배(stop and start)로 인한 I/O 부하 발생
* shard allocation 옵션 disable 후 shutdown
* elasticsearch 실행 및 allocation 옵션 all 처리(all 처리, unassigned 된 샤드가 노드에 할당)



**Elasticsearch 데이터구조**

* doc 을 index 로 만든 뒤 샤드로 분리하여 보관
* 샤드 : 논리적,물리적 분할된 인덱스, lucene 인덱스
* lucene 은 새 doc 을 index 에 저장 시 segment 생성
* lucene 의 인덱스 조각인 segment 를 조합해서 데이터 검색
* doc : elasticsearch 기본 데이터 단위
* index : doc 의 집합
* index 는 샤드로 분리되고 각 노드에 분산되어 저장
* 샤드는 루씬의 단일 검색 인스턴스



**index setting**

* static index settings
  * 인덱스 생성 설정, 일부는 closed index 에서 설정 가능
  * number_of_shards : 샤드의 개수
* dynamic index settings
  * live index 에서 update-index-settings API 로 변경 가능
  * 운영 중 인덱스 세팅 변경
  * RESTful API 변경사항 요청
  * number_of_replicas : 운영 중 리플리카 샤드 개수 변경
  * refresh_interval : 세그먼트에 저장된 데이터를 검색할 수 있도록 commit point 생성하는 주기
  * routing.allocation.enable : 데이터 노드에 샤드를 어떤 방식으로 할당
  * routing.rebalance.enable : 데이터 노드에 샤드를 어떤 방식으로 재배치할지
* mapping
  * doc 이 indexing 될 때, doc과 doc 에 포함된 field 를 어떻게 저장하는 지를 결정하는 과정
  * dynamic mapping : doc 을 보고, 알아서 타입을 찾아 mapping
  * static mapping : 사용자 정의 scheme 기준 mapping



**미리 정의된 template indexing**

* 새 index 가 생성 될 때, index 설정 또는 특정 필드 데이터 타입 정의
* index 별 shard 개수 및 특정 필드 개별 관리 가능



**로그 관리 in elastic stack**

* 수많은 서버 환경에서 모든 서비스 정상 동작 확인을 위해서 로그 수집이 필요
* beats 제품군
* filebeat : 로그 데이터를 전달하고 중앙화하기 위햔 경량 producer, 서버 에이전트 형식으로 설치됨



### Data Dashboard Workflow

* Beats 활용 로그 관제 시스템 구축 실습
  * 로그 -> filebeat -> logstash -> elasticsearch <- kibana
  * elasticsearch : lucene 기반 실시간 분산형 RESTful 검색 및 분석 엔진
  * logstash : 각종 로그를 JSON 형태로 만들어서 elasticsearch 에 데이터 전송
  * kibana : elasticsearch data 시각화 솔루션
  * filebeat : 로그 파일 경량화를 통해 elasticsearch 혹은 logstash 전달 역할 수행.
    * logstash 가 과부하면 전송하는 속도 조절, 시스템 가동 중단 혹은 재부팅되면 중단점 기억 후 다시 전송
* Elastic Stack 활용 인구분석 시스템 구축 실습
  * logstash 에서 csv 파일을 읽는다.
  * logstash 에서 filter 설정 후 separator 설정, columns 설정
  * mutate : 타입 혹은 이름 변경
  * output 에는 elasticsearch {} 를 설정 후 stdout 를 통해 로그 확인
  * 시각화 : discovery 에서 원하는 필드를 선택하고 원하는 값들을 다양하게 filter 로 확인 가능
* **CNI(Container Network Interface)** : CNCF 프로젝트 중 하나로 리눅스 컨테이너를 위한 네트워킹, 런타임과 플러그인 간 상호 작용을 정의하며 컨테이너 간 네트워킹을 제어할 수 있는 플러그인을 만들기 위한 표준 API
* 로그 생성 서버에 filebeat 설치, 로그 집계 서버에 Elastic stack 설치
* filebeat 에서 logstash 로 로그 전송, logstash 에서 filtering 된 로그들이 elasticsearch 에 저장됨
* 저장 된 로그는 kibana 를 통해 시각화 가능
* logstash 가 과부하 상태이면, 전송 속도를 조절하고 시스템 가동이 중단 혹은 재부팅되면 중단된 지점부터 다시 전송함.



### 빅데이터 모니터링 시스템 구축

* 쿠버네티스 : 컨테이너 오케스트레이션 툴

* 컨테이너는 Application 을 실제 구동 환경으로부터 추상화 할 수 있는 논리 패키징 매커니즘 제공

  * 컨테이너 간 리눅스 커널 공유
  * 프로세스 격리된 환경
  * 실행 속도가 빠르고, 성능 상 손실이 거의 없음
  * 호스트 머신에는 프로세스로 인식되지만, 컨테이너 관점에서는 독립된 환경을 가진 가상 머신

  

* 쿠버네티스 개요

  * horizontal scaling : 컨테이너 이미지 복제, 쉽게 확장 가능
  * service discovery and load balancing : msa 나 일반 app 에서도 각 서비스에 대한 app 확장 시 부하 분산하는 로드 밸런싱과 각 app 을 찾아 주는 service discovery 는 필수, replicaSet 을 통한 운영 플랫폼 자체에서 서비스 디스커버리, 로드밸런싱 기본 제공
  * rollout and rollback : 무중단 배포 및 배포 롤백, 특정 버전 돌아가는 기능은 서비스 장애 최소화를 위한 필수 사항, 배포에서 replicaSet 을 관리하여 정책에 따른 무중단 배포 및 업데이트 등 플랫폼 차원에서 제공
  * self-healing : 컨테이너 메인 프로세스가 종료되어 정지된 컨테이너를 다시 시작하고, 기 정의된 API 를 통해 컨테이너 상태를 체크하여 컨테이너 재시작하는 작업
  * secret and configuration management : DB 접속 정보나 타 시스템 인증정보 관리하는데, configmap 오브젝트를 통해서 관리함.

* 쿠버네티스

  * master
  * node
  * pod
    * 여러 컨테이너들을 포함
  * 관리자는 kubectl 을 통해 모든 리소스를 관리

* 쿠버네티스 아키텍처

  * master component
    * kube-controller-manager
    * etcd
    * kube-api-server
    * kube-scheduler
  * node component
    * kubelet
    * kube-proxy

* 쿠버네티스 로깅 시스템 구성

  * log 데이터 저장 및 조회 elasticsearch
  * 각 node에서 daemonset 기반으로 log 수집하여 elasticsearch 로 전송하는 filebeat
  * 수집한 log를 시각화하는 kibana
  * kubenetes cluster 에 beats 를 설치하고, elasticsearch, kafka 까지

  

  



