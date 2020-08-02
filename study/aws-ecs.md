# AWS ECS 튜토리얼



Goal : ECS에 컨테이너 이미지 배포



Application, Service, Batch Process - Docker Container 배포 및 관리 규모 조정

ECS 리소스 요구사항에 따른 클러스터 전반에 컨테이너 배치 및 ELB, EC2 보안 그룹 등 여러개와 통합



시작하기 : [Click](https://ap-northeast-2.console.aws.amazon.com/ecs/home?region=ap-northeast-2#/getStarted)

ECS : AWS 기반 클러스터 내 컨테이너 관리 서비스

Fargate 를 통해 서비스 및 테스크 런칭 가능

컨테이너 이미지 를 ECR 에서 관리, Task Definition 에서 Task 생성 후 ECR 로부터 이미지를 가져와서 클러스터에 배포하는 방식

컨테이너는 이미지라는 템플릿을 통해서 만들어짐.

이미지는 Dockerfile 을 기반으로 만들어짐.

ECS 를 하기 앞서 Docker 에 대해서 이해하고 오는 것이 좋음 [Docker Basic](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)



EC2에 도커 설치

```sh
$ sudo yum update -y
$ sudo amazon-linux-extras install docker
$ sudo yum install docker
$ sudo service docker start
$ sudo usermod -a -G docker ec2-user
..logout and login..
$ docker info
```



### EC2 - django gunicorn nginx

[Previous Post](https://medium.com/analytics-vidhya/setting-up-django-with-nginx-gunicorn-and-docker-352f7656f869)

[Click](https://medium.com/analytics-vidhya/setting-up-django-with-nginx-gunicorn-and-aws-ecs-e1b279c7ae8)



ECS - **확장 가능한, 고성능 컨테이너 관리 서비스(EC2 기반 도커 지원)**



Prerequisite

- Public, Private Subnets
- Application Load Balancer
- IAM roles, polices 생성 권한



Architecture Diagram

* ALB : 80 포트 Listen
* ECS Container 들은 nginx 로 80 포트로 동작중, 각 호스트에는 랜덤 포트로 매핑
* 네트워크는 브릿지 모드



Create ECS Cluster

* EC2 Linux + Networking Template
* Click on Next
* Cluster name : `harsh-ecs`
* Provisioning Model : default
* Instance Type : `t3.medium`
* Instance No : default
* AMI-ID : default
* EBS Storage : default
* VPC : new VPC
* Private Subnet : default
* Security Group : default
* Inbound Group : default
* Container Instance IAM role : default
* Click on `Create`



Getting image to docker hub

* [Previous Post](https://medium.com/analytics-vidhya/setting-up-django-with-nginx-gunicorn-and-docker-352f7656f869) 에서 생성한 아래 2개의 컨테이너 이미지
  * hello-django
  * hello-nginx



Create Task Definition

* ECS 에서 컨테이너 기동 명령어
* 아래 포함
  * 테스크에 포함된 각 컨테이너에 사용되는 이미지
  * 각 테스크에 포함되는 컨테이너에 사용되는 CPU, 메모리 사용량
* 