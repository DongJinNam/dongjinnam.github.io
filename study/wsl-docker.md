# WSL Docker 사용

- 개발환경
    - windows 10 Pro 이상
    - windows 10 Home 불가
  
- 참고
    - [Setting Up Docker for Windows and WSL to Work Flawlessly](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly#install-docker-and-docker-compose-within-wsl)
    
- 선행 작업
    - WSL Ubuntu 설치
    - Python 3 기본 버전 설정
    ```sh
    sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 1
    ```
    - pip3 설치
    ```sh
    sudo apt install python3-pip
    ```

- Docker install on WSL ubuntu

    1. Configure Docker for windows
        - [Download](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
        - 설치 중, docker use windows container instead of linux **체크 해제**
        - 설치 후, Expose Daemon on tcp://localhost:2375 without TLS **체크**
        
    2. WSL Ubuntu Docker 설치
        ```sh
        sudo apt-get update -y
        sudo apt-get install -y \
            apt-transport-https \
            ca-certificates \
            curl \
            software-properties-common
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        sudo apt-key fingerprint 0EBFCD88
        sudo add-apt-repository \
            "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
            $(lsb_release -cs) \
            stable"
        sudo apt-get update -y
        sudo apt-get install -y docker-ce
        sudo usermod -aG docker $USER
        ```
        
    3. Docker Compose 설치
        ```sh        
        sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)"  -o /usr/local/bin/docker-compose
        sudo mv /usr/local/bin/docker-compose /usr/bin/docker-compose
        sudo chmod +x /usr/bin/docker-compose
        ```
        
    4. Connect to a remote Docker daemon
        ```sh
        echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
        ```
    
    5. 작동 여부 확인
        ```sh
        docker info
        docker-compose --version
        ```
