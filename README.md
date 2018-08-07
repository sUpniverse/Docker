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

  ```bash
  $ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```

  - run 명령어를 사용하면, 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드를 하고 컨테이너를 생성하고 시작한다.

  - 하지만 run을 하면 컨테이너가 정상적으로 실행되지만, 명령어를 전달하지 않았기에 컨테이너는 생성되자마자 종료된다. 그래서 명령어를 주어서 실행해본다.

    ```bash
    $ docker run --rm -it ubuntu:latest /bin/bash
    ```

- 관련 명령어

  ```bash
  # 실행중인 도커 머신 정보 보기
  
  $ docker ps
  
  # 실행중이 않은 머신을 포함한 정보 보기
  
  $ docker ps -a
  
  # 도커 머신 삭제 하기
  
  $ docker rm -f [container name]
  
  # 도커 이미지 확인 하기
  
  $ docker images
  
  # 도커 이미지 삭제 하기
  
  $ docker rmi [image name]
  
  # 도커 머신 중지하기
  
  $ docker stop [container name]
  
  # 도커 머신 시작하기
  
  $ docker start [container name]
  
  # 도커 머신으로 파일 복사하기
  
  $ docker cp [소스파일] [머신이름:디렉토리]
  
  # 도커 머신으로 부터 파일 가져오기
  
  $ docker cp [머신이름:소스파일] [디렉토리]
  
  # 도커 머신으로 재접속하기
  
  $ docker attach [머신이름]
  ```

- Dockerfile을 이용하여 Langage-pack-ko 설치하기

  ```dockerfile
  FROM ubuntu:latest
  
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
    $ docker build --tag ko_ubuntu:latest ./
    ```

  - 머신 생성하고 접속하기

    ```shell
    $ docker run -dit --name my-sup2 ko_ubuntu:latest
    $ docker exec -it my-sup2 /bin/bash
    ```

- 자바 설치해보기

  - 수동으로 설치하기

    ```bash
    $ apt-get install wget
    $ wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" + "다운로드 주소"
    
    $ tar -xvf "파일명"
    
    # 편한 접근을 위해 심볼릭 링크
    $ ln -s "파일명" java
    ```

  - 환경 설정하기

    ```bash
    #.bash_profile
    JAVA_HOME=/usr/apps/java
    PATH=$PATH:$JAVA_HOME/bin
    ```

- Dockerfile 을 이용해 나만의 Docker 이미지 만들기

  - docker search "찾고 싶은 것"

  - 위의 ko-언어팩 설치 부분을 이용 / [docker hub](https://hub.docker.com/)를 이용해서 찾은 후

    ```dockerfile
    FROM pull accelad/docker-maven-java10-oracle
    # FROM 밑 부분을 모두 붙여 넣는다
    # ko_locale + apt-get install vim 등
    ```

    - 밑 부분에 자신이 원하는 것들을 넣어 놓는다.

  - 그 후 위 처럼 build 를 하면 된다.

- 소스코드 배포하기

  - docker에 접속할때 port 설정하기

  - ```bash
    $ docker run -dit --name my-sup2 -p 7000:8080 my-dev
    ```

  - git 설치 후 배포

    ```bash
    $ git --version
    
    # 배포할 git 저장소 받아오기
    $ git clone "저장소"
    
    # git 폴더 내의 target으로 이동후
    $ java -jar "프로젝트.jar" &
    ```

    - [또는 쉘 스크립트를 활용하여 배포 자동화를 통해 관리](https://github.com/sUpniverse/worm_springboot)
    - 후에  docker_ipAddress:7000 으로 접속