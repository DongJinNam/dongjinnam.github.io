# AWS VPC 튜토리얼



참고 : [https://brunch.co.kr/@topasvga/384](https://brunch.co.kr/@topasvga/384)

참고 : [https://www.44bits.io/ko/post/understanding_aws_vpc](https://www.44bits.io/ko/post/understanding_aws_vpc)



**VPC**

* AWS 를 통해 상용 웹 서비스 운영 및 깊게 이해하고자 하는 경우 이해해야 됨.
* 입문자 경우에는 당장은 알 필요 없으나, 알아두면 나중에 도움이 됨.
* AWS 클라우드 논리적 격리된 공간을 프로비저닝하여 고객이 정의하는 AWS 리소스 시작 가능



**VPN**

* VPC 와 직접 관련은 없으나, VPC 라는 개념을 설명하기 예시로 들기 좋은 개념

* 모든 망은 공개되어 있지만, 사내망과 같이 Private 망을 구축하고자 할 경우 가상의 사설망을 만듬



**Components**

* 1 VPC 
    * Private Cloud 구성하는 기본 리소스
    * CIDR Block : IP 범위 Definition(ex. 192.168.0.0/24 - 192.168.0.0 ~ 192.168.0.255)
* n subnet
    * AZ(가용존)
    * mask range : 16 ~ 65535
* 1 routing table
    * subnet 에 연결된 리소스
    * 다수의 subnet 에서 사용 가능
* 1 Network ACL (ex. 업무PC -> 상용 웹 서버 방화벽 신청)
    * 가상 방화벽(V-firewall)
    * 다수의 서브넷에서 재사용 가능
    * 서브넷 앞단 관련 방화벽
* 1 Security Group(ex. 특정 IP VM -> 서버 간 방화벽 신청)
    * EC2 인스턴스 관련 방화벽
* 1 internet Gateway(=internet gateway)
    * 기본적으로 VPC는 논리적 격리된 공간, 인터넷 사용 불가
    * 인터넷 사용을 위해서는 외부로 나갈 Gateway 가 필요
    * 다만, 서브넷과 Gateway 만으로는 인터넷 연결이 불가 -> `라우팅 테이블 설정 필수`
    * 인터넷 연결하기 위해서는 Public IP 소유 필수
* 1 DHCP Optionset



**How to install VPC**

* <https://ap-northeast-2.console.aws.amazon.com/vpc/home?region=ap-northeast-2#wizardSelector>:
* Console - VPC - VPC 생성
    * ipv4 cidr block : 192.168.0.0/16
    * name : vpc-apne2-test
    * public subnet ipv4 CIDR : 192.168.0.0/24
    * public subnet name : subnet-apne2-test-pub
    * az : ap-northeast-2c (d는 신설존이라 안정성 불확실)
    * others : default
* VPC - Routing table
    * 위 생성한 VPC ID 에 맞는 Routing Table 을 찾아 name 변경
    * name : rtb-apne2-test
* Security - Network ACL
    * 위 생성한 VPC ID 에 맞는 Network ACL 을 찾아 name 변경
    * name : acl-apne2-test
* Security - Security Group
    * 위 생성한 VPC ID 에 맞는 Security Group 을 찾아 name 변경
    
    * name : sg_apne2_test
    
      

**How to install subnet**

* VPC 위에 직접 EC2 Instance 를 올릴 수가 없으며, 올릴려면 우선 서브넷 부터 생성해야함.
* Console - VPC - Subnet 생성
    * name : subnet-apne2-test
    * vpc : vpc-apne2-test
    * AZ : ap-northeast-2c
    * vpc cidr : 192.168.0.0/16
    * ipv4 cidr block : 192.168.0.0/24
* Console - VPC - Subnet 생성
    * name : subnet-apne2-test2
    * vpc : vpc-apne2-test
    * AZ : ap-northeast-2d
    * vpc cidr : 192.168.0.0/16
    * ipv4 cidr block : 192.168.1.0/24
* 네트워크 설계 시, 하나의 AZ에서 장애가 나더라도 서비스는 운영되어야 하기 때문에 2개의 AZ를 전제로 네트워크를 설계하는 것을 권장 -> `서브넷 2개 이상을 두어야 하는 이유`



**How to install internet gateway**

* Console - vpc - internet gateway
    * igw-apne2-test
    * 생성 후, 작업 - VPC에 연결



**Routing table 설정**

* VPC - routing table - rtb-apne2-test 선택 후 라우팅 편집
* 라우팅 추가
    * 0.0.0.0/0 igw-apne2-test



**How to install EC2 instance**

* Console - ec2 - create
    * Amazon Linux AMI
    * t2.micro
    * network : vpc-apne2-test
    * subnet : `subnet-apne2-test`
    * 외부 접속 허용할 경우, public IP 할당 필요 (public 자동 할당 활성화)
    * 8gb
    * 6단계 보안 그룹 구성(sg_apne2_test)
      * name : sg_apne2_test
      * description : sg_apne2_test for test vpc
      * ssh, tcp, 22, Source(0.0.0.0/0)
    * keypair-apne2-test 생성 후, 로컬 보관 필수!!(이 때만 다운로드 가능)
* 가용영역 `ap-northeast-2d` 에 instance 설치 여부는 확인 후 진행 부탁 드립니다.
    * 테스트 시점 기준(08.01), 아래와 같이 시작 실패 메시지 발생

```
시작 실패
Your requested instance type (t2.micro) is not supported in your requested Availability Zone (ap-northeast-2d). Please retry your request by not specifying an Availability Zone or choosing ap-northeast-2a, ap-northeast-2c.
시작 로그 숨기기
```



* **ec2 instance 연결하기 전 아래 내용 확인 필요**
    * **VPC에 Internet GW 연결**
      * 연결되어 있지 않으면,해당 ec2 instance 는 직접 접근이 불가합니다.
    * **라우팅 테이블이 Internet GW 향하는지**
      * 라우팅 테이블에 internet gateway 미설정 되어 있어도 접근 불가.
    * **서브넷 인스턴스에 고유 IP 주소가 있는지**



* bastion host
  * private subnet 에 구성된 VM 에 경우에는 internet gateway 를 통해서 접근 불가함.
  * VPN 을 이용하는 방법이 있으나, 비용 문제로 인하여 bastion host 방식 을 이용하는 것이 좋음
  * **To be continued**
  * ec2 instance 생성
    * vpc-apne2-test
    * subnet-apne2-test
    * public 자동 할당 비활성화
    * 



### WEB, WAS, DB



**Components**

* WEB : Presentation Layer (ex. Apache, Nginx)
* WAS : Business Layer (ex. Spring, Django and so on)
* DB : Data Layer (ex. RDBMS, NoSQL)



위 3가지 기준, VM 3개 생성 후 아래와 같이 테스트 합니다.

* WEB : Public Subnet
* WAS : Private Subnet
* DB : Private Subnet



