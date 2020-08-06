# AWS ECS 튜토리얼



### Goal

Fargate 를 활용하여 ECS 클러스터에 컨테이너 배포



### Definition

* Container Definition
* Task Definition
* Service
* Cluster



### Tutorial

* AWS 로그인 후, Fargate 를 사용하여 ECS 시작 
* https://ap-northeast-2.console.aws.amazon.com/ecs/home?region=ap-northeast-2#/firstRun
  * 컨테이너 및 작업
  * 서비스
  * 클러스터
  * 검토
* 컨테이너 정의
  * nginx
  * memory : 0.5gb
  * cpu : 0.25vCPU
* 작업 정의
  * name : first-run-task-definition
  * network : awsvpc
  * 작업 실행 역할 : 새로 생성
  * 호환성 : fargate
  * 작업 메모리 : 0.5gb
  * 작업 CPU : 0.25vCPU
* 서비스 정의
  * nginx-service
  * 작업 개수 1개
  * 보안 그룹 자동 생성
  * 로드 밸런서 없음
* 

