# DOCKER

## DOCKER 명령어

### DOCKER 서비스 시작/종료/재시작

- [sudo service docker restart] Docker서비스 재시작
- [systemctl restart docker] CentOS Docker 서비스 재시작

### Docker Hub 이미지 검색

> **docker search 이미지명**

```bash
docker search ansible
```

### DOCKER 이미지 받기

> **Docker pull <이미지 이름>:<태그>**
> **latest : 최신버젼**

```bash
docker pull ansible/centos7-ansible
```

### DOCKER 이미지 목록 출력

> **docker images**

```bash
docker images
```

### DOCKER 이미지 실행

> **Docker run <옵션> <이미지 이름> <실행할 파일>**
> **예제) docker run –-name hello –i –t Ubuntu /bin/bash**
> **-i : interactive**
> **-t : Pseudo-tty**
> **-name : 컨테이너의 이름을 지정, (지정하지 않으면 Docker가 자동으로 이름을 생성)**

```bash
docker run –-name hello –i –t Ubuntu /bin/bash**
```

- 이미지를 컨테이너로 생성한 뒤 Bash Shell을 실행(생성/실행)

### DOCKER 설정파일

```bash
# LINUX
/etc/sysconfig/docker
other_args=”—selinux-enabled –exec-driver=lxc”
```

### 모든 컨테이너 출력

> **[docker ps -a] 모든 컨테이너 출력**
> **-a : 정지된 컨테이너까지 출력**

### 컨테이너 시작/종료/재시작

#### 컨테이너 시작

> **docker start <컨테이너 이름 or 컨테이너 ID>**

```bash
sudo docker start hello
```

#### 컨테이너 재시작

> **docker restart <컨테이너 이름 or 컨테이너 ID>**

```bash
sudo docker restart hello
```

#### 컨테이너에 접속

> **docker attach <컨테이너 이름 or 컨테이너 ID>**
> **컨테이너 정지 : Bash Shell exit 또는 Ctrl+D**
> **컨테이너 빠져 나옴 : Ctrl+P, Ctrl+Q**

```bash
sudo docker attach hello
```

#### 컨테이너에 안의 명령을 실행

> **docker exec <컨테이너 이름 or 컨테이너 ID> 명령어**

```bash
docker exec hello echo “Hello World”
```

#### 컨테이너 정지

docker stop <컨테이너 이름 or 컨테이너 ID>
[sudo docker stop hello] 


[sudo docker rm hello] 생성된 컨테이너 삭제
docker rm <컨테이너 이름 or 컨테이너 ID>

[sudo docker rmi hello] 생성된 이미지 삭제
docker rmi <컨테이너 이름 or 컨테이너 ID>

![DOCKER LIFECYCLE](./images/docker_lifecycle.png)

## 이미지 생성하기

팁
마지막에 실행된 컨테이너의 ID

alias dl='docker ps -l -q'
docker run ubuntu echo hello world
docker commit `dl` helloworld
명령어와 함께 커밋하기

docker commit -run='{"Cmd":["postgres", "-too -many -opts"]}' `dl` postgres
IP address 정보

docker run -i -t ubuntu /bin/bash
명령어를 사용하거나 아래 명령어를 사용합니다.

wget http://stedolan.github.io/jq/download/source/jq-1.3.tar.gz
tar xzvf jq-1.3.tar.gz
cd jq-1.3
./configure && make && sudo make install
docker inspect `dl` | jq -r '.[0].NetworkSettings.IPAddress'
이미지의 환경 변수 읽어오기

docker run -rm ubuntu env 
오래된 컨테이너들 삭제하기

docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm
멈춰있는 컨테이너들 삭제하기

docker rm `docker ps -a -q`
이미지의 의존관계 이미지로 출력하기

docker images -viz | dot -Tpng -o docker.png


DOCKER 치트
[레지스트리(Registry) & 저장소(Repository)]
저장소(repository)란 컨테이너를 위한 파일 시스템을 생성할 수 있는 호스트되는 태그가 붙어있는 이미지들의 집합.
레지스트리란 저장소를 저장해두고 HTTP API를 통해 저장소의 업로드, 관리, 다운로드를 제공하는 호스트를 의미한다. @@@

Docker.io는 매우 다양한 저장소를 포함하고 있는 이미지 [index]를 가지고 있는 중앙 레지스트리이다. @@@

docker login 레지스트리에 로그인한다.
docker search 레지스트리에서 이미지를 검색한다.
docker pull 이미지를 레지스트리에서 로컬 머신으로 가져온다(pull).
docker push 이미지를 로컬 머신에서 레지스트리에 집어넣는다(push).

[이미지]

라이프 사이클
docker images 모든 이미지 목록을 보여준다.
docker import tarball 파일로부터 이미지를 생성한다.
docker build Dockerfile을 통해 이미지를 생성한다.
docker commit 컨테이너에서 이미지를 생성한다.
docker rmi 이미지를 삭제한다.
docker insert URL에서 이미지로 파일을 집어넣는다. * docker load 표준 입력으로 tar 파일에서 (이미지와 태그를 포함한) 이미지를 불러온다.(0.7부터 사용가능).
docker save 모든 부모 레이어와 태그, 버전 정보를 tar 형식으로 표준출력을 통해 @@@ (0.7부터 사용가능).

관련된 정보를 출력해주는 명령어
docker history 이미지의 이력 정보를 보여준다.
docker tag 이미지에 이름으로 태그를 붙여준다(local 혹은 registry).

[컨테이너]
라이프 사이클
docker run 컨테이너를 생성한다.
docker stop 컨테이너를 정지시킨다.
docker start 컨테이너를 다시 실행시킨다.
docker restart 컨테이너를 재가동한다.
docker rm 컨테이너를 삭제한다.
docker kill 컨테이너에게 SIGKILL을 보낸다. 이에 관련된 이슈가 있다..
docker attach 실행중인 컨테이너에 접속한다. * docker wait 컨테이너가 멈출 때까지 블럭한다.

관련된 정보를 출력해주는 명령어
docker ps 명령어는 실행중인 컨테이너 목록을 보여준다.
docker inspect ip 주소를 포함한 특정 컨테이너에 대한 모든 정보를 보여준다.
docker logs 컨테이너로부터 로그를 가져온다.
docker events 컨테이너로부터 이벤트를 가져온다.
docker port 컨테이너의 특정 포트가 어디로 연결되어있는지 보여준다.
docker top 컨테이너에서 실행중인 프로세스를 보여준다.
docker diff 컨테이너 파일 시스템에서 변경된 파일들을 보여준다.

Import / Export
docker cp 컨테이너 내의 파일을 호스트로 복사한다.
docker export 컨테이너 파일 시스템을 tarball로 출력한다.

DOCKER 관련 URL
https://gist.github.com/nacyot/8366310 도커 치트 시트
https://docs.docker.com/engine/reference/builder/ 도커파일 레퍼런스
http://programmingsummaries.tistory.com/392 도커파일 작성하기

###################################
[SPLUNK]

```bash
## 실행중인 컨테이너에 들어감.
## docker exec -t -i [컨테이너명] /bin/bash
docker exec -t -i splunkpublic /bin/bash
```

자동라이센스 추가

```bash
./splunk add licenses /opt/splunk/etc/licenses/enterprise/enterprise.lic
./splunk list licenses
./splunk list licenser-pools
./splunk list licenser-slaves
./splunk list licenser-localslave
./splunk edit licenser-localslave -master_uri 'https://master:port'
./splunk list licenser-messages
```

./splunk add licenses /opt/splunk/etc/licenses/Splunk.License