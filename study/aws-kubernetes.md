# AWS Kubernetes

* 쉬운 쿠버네티스 구축을 위하여 AWS 활용



* 기존 VM
  * 각 VM들은 OS 마다 개발환경이 다름
* Container
  * 다른 컨테이너와 커널을 공유하는 특징
  * 도커 엔진 위에 동작함
  * 각 App 별로 의존성을 가지기 때문에, 환경에 얽매일 필요 X
* Docker
  * build & deploy container 엔진
  * Question
    * 컨테이너 간 통신
    * 컨테이너 스케줄링
    * 컨테이너 인스턴스 확장
      * **Answer is Kubernetes**
      * Open source Container Orchestration Tool
      * Made by **Google**
* 쿠버네티스 기초
  * 여러 클러스터를 묶어 하나의 유닛 처럼 동작하는 고가용성을 가짐
  * 컨테이너 App을 클러스터에 배포 가능
* 쿠버네티스 클러스터 구성 요소
  * Master : 클러스터 관리 주체
    * 어플리케이션 스케줄링
    * 어플리케이션 확장
  * Nodes : Container App 기동 주체
    * 워커 머신으로 동작하는 VM 혹은 Physical Computer
    * 각 노드는 kubelet 을 가짐(노드를 관리하고 마스터와 커뮤니케이션함)
    * 하나의 마스터 당 최소 3개의 노드를 가짐
    * 컨테이너 앱 배포 방식 -> **kubernetes API 사용**
    * 
* Amazon Elastic Container Service(EKS)
  * 관리하기 위한 컨트롤 플레인 구축할 필요 X
  * 기본 Secure 설정 제공
  * 표준 Kubernetes 호환
  * 쿠버네티스 커뮤니티와 통신 가능
* To Do
  * Amazon EKS 클러스터 구축
  * 워커 노드 배포
  * EKS 연결(kubectl)
  * EKS Cluster 위 Kubernetes App 기동

