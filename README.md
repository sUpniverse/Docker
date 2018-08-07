# Docker

- 초보 프로그래머의 도커 사용기_ #얼마 안할테지만 #그래도 잘 정리해 놓아보자

- 도커.Araboja

  - 도커란? VMware, VirtualBox같은 가상화 도구인데 아주 가볍기 때문에 많이 쓴다.
  - 그래서.. 다양한 OS도 마음껏 쓰고, 내 PC를 더럽히지 않으면서 테스트가 가능하다.

- 도커를 설치하는 방법

  - 나의 경우엔 MAC이므로 Docker for Mac을 설치
  - 하지만.. Toolbox를 많이 깐다. 
  - 또한 처음 사용자는 어려우므로,, Docker for 사용을 권장하기보다 Docker machine을 추천한다.
    - Virtual Box 나 VMware같은 가상머신에 리눅스를 설치하여 사용을 추천한다.

- 가상머신 생성

  ```shell
  docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```

  - run 명령어를 사용하면, 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드를 하고 컨테이너를 생성하고 시작한다.

  - 하지만 run을 하면 컨테이너가 정상적으로 실행되지만, 명령어를 전달하지 않았기에 컨테이너는 생성되자마자 종료된다. 그래서 명령어를 주어서 실행해본다.

    ```shell
    docker run --rm -it ubuntu:16.04 /bin/bash
    ```

- 관련 명령어

  ```shell
  # 실행중인 도커 머신 정보 보기
  
  >> docker ps
  
  # 실행중이 않은 머신을 포함한 정보 보기
  
  >> docker ps -a
  
  # 도커 머신 삭제 하기
  
  >> docker rm -f [container name]
  
  # 도커 이미지 삭제 하기
  
  >> docker rmi [image name]
  
  # 도커 머신 중지하기
  
  >> docker stop [container name]
  
  # 도커 머신 시작하기
  
  >> docker start [container name]
  
  # 도커 머신으로 파일 복사하기
  
  >> docker cp [소스파일] [머신이름:디렉토리]
  
  # 도커 머신으로 부터 파일 가져오기
  
  >> docker cp [머신이름:소스파일] [디렉토리]
  
  # 도커 머신으로 재접속하기
  
  >> docker attach [머신이름]
  ```

- Dockerfile을 이용하여 Langage-pack-ko 설치하기

  ```dockerfile
  FROM ubuntu:16.04
  
  RUN apt-get update
  
  RUN apt-get install -y language-pack-ko
  
  #set locale ko_KR
  RUN locale-gen ko_KR.UTF-8
  
  ENV LANG ko_KR.UTF-8
  ENV LANGUAGE ko_KR.UTF-8
  ENV LC_ALL ko_KR.UTF-8
  
  CMD /bin/bash
  ```

  - 빌드하기

    ```shell
    docker build --tag ko_ubuntu:16.04 ./
    ```

  - 머신 생성하고 접속하기

    ```shell
    docker run -dit --name my-sup2 ko_ubuntu:16.04
    docker exec -it my-sup2 /bin/bash
    ```

- 자바 설치해보기

  - 그냥 설치하기

    ```shell
    apt-get install java
    ```

  - 수동으로 설치하기

    ```
    apt-get install wget
    
    ```

  - 환경 설정하기

    ```shell
    #.bash_profile
    JAVA_HOME=/usr/apps/java
    PATH=
    ```

    