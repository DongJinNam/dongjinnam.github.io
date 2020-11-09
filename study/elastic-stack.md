# Elastic Stack



본 튜토리얼은 Elastic Stack 을 활용하여 운영 중인 nginx 웹 서버의 로그를 filebeat 와 logstash, elasticsearch 를 통해 수집하고,

이를 분석하여 kibana 를 통해 시각화 하는 과정까지 진행할 예정이다.


**참고 링크**

* https://www.guru99.com/elk-stack-tutorial.html
* https://medium.com/@vitalypanukhin/elasticsearch-elk-stack-for-log-management-8f4e61a60239

우선, 아래 튜토리얼을 진행하기 앞서 배경 지식을 먼저 살펴보자.

기존 application 로그 저장 방식은 아래와 같은 **한계**를 지님.

* 로그를 확인하기 위해 서버에 접속해야 함

* 여러 서버에 접속하여 grep 명령어 사용하는 것은 비효율적
* 로그 데이터가 많아질 경우, 이를 일일이 관리하기가 어려움.


Elastic stack 은 서버와 어플리케이션 로그를 하나로 모아서 중앙 집중형태로 관리한다.

특정 타임 주기별로 여러 서버들로 부터 로그를 수집한다.


### Elastic Stack Architecture

**구성 요소**

* log : 수집 및 분석 대상
* beats : 로그 수집기
* logstash : 데이터 전처리
* elasticsearch : 데이터 저장소
* kibana : 데이터 시각화



**기본 원리**

* log -> beats -> logstash -> elasticsearch -> kibana



데이터 양이 방대해져서 Message Queue 를 사용할 경우, 아래 구조도 가능

* log -> beats -> mq(rabbit mq or kafka) -> logstash -> elasticsearch



### Basics of Elasticsearch



**What is Elastcsearch?**

> NoSQL DB, 루씬 기반 검색 엔진, RESTful API 지원
>
> 큰 사이즈의 데이터를 저장하고 검색하고 분석하는데 용이함



**features of elasticsearch**

* server 자바로 구성됨
* 다양한 유형의 데이터를 indexing
* RESTful API 인터페이스
* Full Text 검색엔진
* 실시간 검색
* Sharded, Replication
* Schema Free
* 다국어 처리



**advantages of elasticsearch**

* 비정형 데이터를 기록할 수 있음
* RESTful API 를 통해 데이터 제어가 가능



**terms in elaticsearch**

* cluster : 노드들의 집합
* node : elasticsearch 인스턴스
* index : documents 들의 집합
* document : 인덱싱되는 기본 데이터 단위
* shard : index 의 한 부분



### Basics of Logstash

* 데이터 수집 및 처리 파이프라인 툴
* 다양한 소스들로부터 데이터를 정제하여 elasticsearch 로 데이터를 보내는 역할



**구성요소**

* input : 특정 형식의 프로세스에 로그를 넘기는 역할
* filters : 특정 액션 혹은 이벤트 조건들의 집합
* output : 이벤트 혹은 로그의 결정







