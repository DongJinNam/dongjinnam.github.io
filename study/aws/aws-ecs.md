# AWS ECS 튜토리얼



### Referenced From

* [Docker Basic](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

* [AWS IAM User 생성](https://www.44bits.io/ko/post/publishing_and_managing_aws_user_access_key)
* [ECS 의 모든것](https://www.44bits.io/ko/post/container-orchestration-101-with-docker-and-aws-elastic-container-service)



### Prerequisites

* docker
* docker-compose



### Basic

* Docker
  * 컨테이너 기반 가상화 플랫폼
  * 컨테이너 가장 잘 사용하는 기업? Google (매주 20억개 이상 컨테이너를 기동한다는..)
  * 격리된 공간에서 프로세스로 동작하지만, 이전의 OS 가상화와는 달리 성능 문제는 크게 없음
    * 프로세스가 OS 에 종속되지 않으며, 각 OS에 따라 돌아가는 프로그램이 달라서 생기는 문제
  * Image
    * 도커 위에 동작하는 컨테이너에 필요한 파일 혹은 설정값을 포함
    * 다만 프로세스에 동작하는 모든 파일 및 설정 정보를 관리하기에 용량은 꽤 크다.
    * How to make? `Dockerfile`
* ECS
  * AWS 에서 제공하는 Container Orchestration Tool
  * EC2 뿐만 아니라 Fargate 를 통해서 Serverless 형태로 배포도 가능
* why we need to learn?
  * 여러 컨테이너를 동시에 기동하면, 컨테이너 오케스트레이션이 필요
  * 컨테이너마다 CPU 사용량, 메모리 사용량을 한정지어야 할 경우가 필요
    * ex. Gitlab, Xwiki 등



* 클러스터
  * 도커 컨테이너들을 실행할 수 있는 가상 공간
  * `ECS의 가장 기본단위`
* Task
  * ECS에 컨테이너를 실행하는 최소 단위
  * 1개 이상 컨테이너로 구성
  * How to run? -> `Task Definition`
    * 컨테이너 오케스트레이션에 필요한 정보
      * Image
      * Command
      * CPU limit
      * Memory Limit
* Service
  * Task 관리 단위
  * 클러스터에서 2가지 방식으로 테스크 실행
    * Task Definition
      * 실행 이후에는 더 이상 관리가 되지 않음
    * Service
      * 1개의 Task Definition 과 연결됨.

* ECR
  * Elastic Container Registry (= Docker Hub)



### To Do

* ECS 클러스터 구성
* nginx docker container 배포
* 로드 밸런서 연결 및 Multi Task 실행
* 동적 포트 설정 및 Task Definition and Service



### Nginx 배포 in ECS

**Prerequisites**

* https://www.44bits.io/ko/page/aws_command_line_interface_basic

* https://www.44bits.io/ko/page/publishing_and_managing_aws_user_access_key

* 필자는 admin 계정 생성 후, `aws configure` 설정 후 아래 튜토리얼 진행



**Setting aws configure**

```sh
$ pip install awscli --upgrade
$ aws configure
AWS Access Key ID [None]: (생성한 계정의 access key id)
AWS Secret Access Key [None]: (생성한 계정의 access key secret)
Default region name [None]: ap-northeast-2
Default output format [None]:
```



**Create ECS Cluster**

Request

```sh
$ aws ecs create-cluster --cluster-name awesome-ecs-cluster
```

Response Example

```json
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:ap-northeast-2:628762111948:cluster/awesome-ecs-cluster",
        "clusterName": "awesome-ecs-cluster",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": [],
        "tags": [],
        "settings": [
            {
                "name": "containerInsights",
                "value": "disabled"
            }
        ],
        "capacityProviders": [],
        "defaultCapacityProviderStrategy": []
    }
}
```



**List of ecs cluster**

Request

```sh
$ aws ecs list-clusters | jq '.clusterArns[]'
```

Response

```json
"arn:aws:ecs:ap-northeast-2:628762111948:cluster/awesome-ecs-cluster"
```



**Add Container instance on cluster**

* VPC 는 이전 VPC 생성 가이드 참고하여 생성
* EC2 인스턴스 시작
  * Amazon ECS-Optimized Amazon Linux 2 AMI
  * Free Tier 선택
  * Instance 세부 정보
    * network : vpc-apne2-test
    * subnet : subnet-apne2-test
    * public ip enabled : active
    * storage : 30GB (기본 ec2 프리티어는 8gb 이지만, ecs에 최적화된 OS 이미지)
    * sg_apne2_test
      * HTTP, TCP, 80, 0.0.0.0/0
      * SSH, TCP, 22, 0.0.0.0/0
    * 

* 기본  VPC 에 직접 인스턴스 생성 후 클러스터에 컨테이너 인스턴스 추가



**Run ec2 instances**

* ecs 에 최적화된 ami id : `ami-029df54b84ab8240f`

```sh
$ aws ec2 run-instances \
  --image-id ami-029df54b84ab8240f \
  --count 2 \
  --instance-type t2.micro \
  --security-group-ids sg-06b6318ca10edd623 \
  --subnet-id subnet-035357a35bd4dc561 \
  --user-data ./userdata.sh \
  --key-name keypair-apne2-test-ecs \
  --associate-public-ip-address
```



Response

```
{
    "Groups": [],
    "Instances": [
        {
            "AmiLaunchIndex": 1,
            "ImageId": "ami-029df54b84ab8240f",
            "InstanceId": "i-0c80835de479ff6fe",
            "InstanceType": "t2.micro",
            "KeyName": "keypair-apne2-test-ecs",
            "LaunchTime": "2020-08-06T16:43:10.000Z",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "ap-northeast-2c",
                "GroupName": "",
                "Tenancy": "default"
            },
            "PrivateDnsName": "ip-192-168-0-74.ap-northeast-2.compute.internal",
            "PrivateIpAddress": "192.168.0.74",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "StateTransitionReason": "",
            "SubnetId": "subnet-035357a35bd4dc561",
            "VpcId": "vpc-01412cfd51b76a134",
            "Architecture": "x86_64",
            "BlockDeviceMappings": [],
            "ClientToken": "65999a96-eb95-475c-a705-b44f4e281f67",
            "EbsOptimized": false,
            "Hypervisor": "xen",
            "NetworkInterfaces": [
                {
                    "Attachment": {
                        "AttachTime": "2020-08-06T16:43:10.000Z",
                        "AttachmentId": "eni-attach-03026f25d83808e50",
                        "DeleteOnTermination": true,
                        "DeviceIndex": 0,
                        "Status": "attaching"
                    },
                    "Description": "",
                    "Groups": [
                        {
                            "GroupName": "sg_apne2_test",
                            "GroupId": "sg-06b6318ca10edd623"
                        }
                    ],
                    "Ipv6Addresses": [],
                    "MacAddress": "0a:b0:8d:fc:f3:5e",
                    "NetworkInterfaceId": "eni-0ea258673c219a284",
                    "OwnerId": "628762111948",
                    "PrivateDnsName": "ip-192-168-0-74.ap-northeast-2.compute.internal",
                    "PrivateIpAddress": "192.168.0.74",
                    "PrivateIpAddresses": [
                        {
                            "Primary": true,
                            "PrivateDnsName": "ip-192-168-0-74.ap-northeast-2.compute.internal",
                            "PrivateIpAddress": "192.168.0.74"
                        }
                    ],
                    "SourceDestCheck": true,
                    "Status": "in-use",
                    "SubnetId": "subnet-035357a35bd4dc561",
                    "VpcId": "vpc-01412cfd51b76a134",
                    "InterfaceType": "interface"
                }
            ],
            "RootDeviceName": "/dev/xvda",
            "RootDeviceType": "ebs",
            "SecurityGroups": [
                {
                    "GroupName": "sg_apne2_test",
                    "GroupId": "sg-06b6318ca10edd623"
                }
            ],
            "SourceDestCheck": true,
            "StateReason": {
                "Code": "pending",
                "Message": "pending"
            },
            "VirtualizationType": "hvm",
            "CpuOptions": {
                "CoreCount": 1,
                "ThreadsPerCore": 1
            },
            "CapacityReservationSpecification": {
                "CapacityReservationPreference": "open"
            },
            "MetadataOptions": {
                "State": "pending",
                "HttpTokens": "optional",
                "HttpPutResponseHopLimit": 1,
                "HttpEndpoint": "enabled"
            }
        },
        {
            "AmiLaunchIndex": 0,
            "ImageId": "ami-029df54b84ab8240f",
            "InstanceId": "i-065b8684cb583293e",
            "InstanceType": "t2.micro",
            "KeyName": "keypair-apne2-test-ecs",
            "LaunchTime": "2020-08-06T16:43:10.000Z",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "ap-northeast-2c",
                "GroupName": "",
                "Tenancy": "default"
            },
            "PrivateDnsName": "ip-192-168-0-109.ap-northeast-2.compute.internal",
            "PrivateIpAddress": "192.168.0.109",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "StateTransitionReason": "",
            "SubnetId": "subnet-035357a35bd4dc561",
            "VpcId": "vpc-01412cfd51b76a134",
            "Architecture": "x86_64",
            "BlockDeviceMappings": [],
            "ClientToken": "65999a96-eb95-475c-a705-b44f4e281f67",
            "EbsOptimized": false,
            "Hypervisor": "xen",
            "NetworkInterfaces": [
                {
                    "Attachment": {
                        "AttachTime": "2020-08-06T16:43:10.000Z",
                        "AttachmentId": "eni-attach-0f6e22ed8eeb62660",
                        "DeleteOnTermination": true,
                        "DeviceIndex": 0,
                        "Status": "attaching"
                    },
                    "Description": "",
                    "Groups": [
                        {
                            "GroupName": "sg_apne2_test",
                            "GroupId": "sg-06b6318ca10edd623"
                        }
                    ],
                    "Ipv6Addresses": [],
                    "MacAddress": "0a:86:f6:be:d8:0c",
                    "NetworkInterfaceId": "eni-0979644e8d989f0ad",
                    "OwnerId": "628762111948",
                    "PrivateDnsName": "ip-192-168-0-109.ap-northeast-2.compute.internal",
                    "PrivateIpAddress": "192.168.0.109",
                    "PrivateIpAddresses": [
                        {
                            "Primary": true,
                            "PrivateDnsName": "ip-192-168-0-109.ap-northeast-2.compute.internal",
                            "PrivateIpAddress": "192.168.0.109"
                        }
                    ],
                    "SourceDestCheck": true,
                    "Status": "in-use",
                    "SubnetId": "subnet-035357a35bd4dc561",
                    "VpcId": "vpc-01412cfd51b76a134",
                    "InterfaceType": "interface"
                }
            ],
            "RootDeviceName": "/dev/xvda",
            "RootDeviceType": "ebs",
            "SecurityGroups": [
                {
                    "GroupName": "sg_apne2_test",
                    "GroupId": "sg-06b6318ca10edd623"
                }
            ],
            "SourceDestCheck": true,
            "StateReason": {
                "Code": "pending",
                "Message": "pending"
            },
            "VirtualizationType": "hvm",
            "CpuOptions": {
                "CoreCount": 1,
                "ThreadsPerCore": 1
            },
            "CapacityReservationSpecification": {
                "CapacityReservationPreference": "open"
            },
            "MetadataOptions": {
                "State": "pending",
                "HttpTokens": "optional",
                "HttpPutResponseHopLimit": 1,
                "HttpEndpoint": "enabled"
            }
        }
    ],
    "OwnerId": "628762111948",
    "ReservationId": "r-0395d3a3aa2d00853"
}
```

