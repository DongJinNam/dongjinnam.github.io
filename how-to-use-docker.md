# Windows 환경 Docker 사용하기

* Windows Container
* 개발환경 구축
* 컨테이너 생성 어려움 및 해결 전략
* What's next?

### Our Story
* Best player experiences excellent content, service, technology!
* 게임 서버 개발 및 테스트 자동화 -> **Kubernetes**
* 2019년 8월부터 Window 에서 Kubernetes 클러스터 및 컨테이너 지원 

### Windows Container 개발하기
* App 단 뿐만 아니라 OS, 업데이트, 커널 버전 등에 따라 시스템 구성이 달라질 수 있다
* **Container 는 철저히 Application 에 Focus**

### 컨테이너
* 애플리케이션 -> 웹 사이트
* IIS -> 프레임워크 이미지
* Windows Server -> 기본 이미지

### 도커파일
* 리눅스와 크게 다르지 않으나, 이미지 loading 시 windows 명시 필요

### Windows 컨테이너 버전 선택
* Process vs Hyper-v
* **Process 방식 사용 권장**
* Win 10 에서 Container 실행 시 OS 버전과 컨테이너 버전 일치 필요

### Windows 컨테이너 이미지 선택
* Nano Server - WIndows 종속성이 없는 애플리케이션 개발 최적화(작은)
* Server Core - 호환성 중시, 대부분 Windows Backend 앱 개발에 적합
* Windows - GUI 어플리케이션 자동화에 적합

### Moby Project
* Docker의 Backend

### 개발환경 구축
* Docker Toolbox for Windows
* Docker Desktop for Windows
* Docker Daemon 설정
  * [참고](https://github.com/moby/moby/pull/38000)
  
### 데이터베이스 관련
* MSSQL Server 2019 버전은 리눅스에 최적화됨
* 현재 상용에서 사용 가능한 컨테이너 기반 MSSQL Server는 리눅스 버전이 유일

### Docker on Windows
* 책 참고

### DevTech
* [DevTech](https://tech.devsisters.com)
* Windows Container
* Windows Kubernetes
* 관련 글 연재중
