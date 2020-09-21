# Splunk + Docker

## 지원되는 태그

* `7.1.2`,`latest`- Splunk Enterprise base image [Dockerfile](https://github.com/splunk/docker-splunk/blob/master/enterprise/Dockerfile)
* `6.5.3-monitor` - Splunk Enterprise with Docker Monitoring [Dockerfile](https://github.com/splunk/docker-itmonitoring/blob/master/enterprise/Dockerfile)

## Splunk Enterprise란 무엇입니까

Splunk Enterprise는 운영 인텔리전스를위한 플랫폼입니다. 이 소프트웨어를 사용하면 기술 인프라, 보안 시스템 및 비즈니스 응용 프로그램에서 생성되는 큰 데이터의 미개척 값을 수집, 분석 및 실행할 수 있습니다. 운영 성과 및 비즈니스 성과를 이끌어내는 통찰력을 제공합니다.

이 저장소에는 [Splunk](https://splunk.com) Docker 이미지를 작성하는 데 사용할 수있는 Dockerfiles가 들어 있습니다.

## Splunk Enterprise Docker 이미지 시작하기

이전에 Docker를 사용하지 않았다면 Docker의 [Getting started tutorial](https://docs.dmacer.com/mac/started)를 참조하십시오.

1. (선택 사항) [Docker Hub](https://hub.docker.com)에서 Docker ID에 가입하십시오.
2. 시스템에 Docker를 다운로드하여 설치하십시오.
3. 쉘 프롬프트 또는 터미널 창을 엽니다.
4. 다음 명령을 입력하여 Splunk Enterprise 버전 7.1.2 이미지를 가져옵니다.
    ```bash
    docker pull splunk/splunk
    ```
5. Docker 이미지 실행
    ```bash
    docker run -d -e "SPLUNK_START_ARGS=--accept-license --seed-passwd <your password>" -e "SPLUNK_USER=root" -p "8000:8000" splunk/splunk
    ```
6. 이 컨테이너의 도커 로그에서 임의로 생성 된 관리자 비밀번호를 찾습니다.
7. Docker 시스템 IP 주소 및 Splunk 웹 포트를 사용하여 브라우저로 Splunk 인스턴스에 액세스합니다. 예를 들어 `(http://localhost:8000)`

> 추가 예제 명령은 [How to use the Splunk Enterprise Docker image](#How-to-use-the-Splunk-Enterprise-Docker-image)을 참조하십시오.

## Splunk Enterprise Docker 이미지 사용법

다음 명령은 쉘 프롬프트 또는 Docker QuickStart 터미널 (Mac OS X)에서 실행할 수 있습니다.

### Splunk Enterprise 버전 7.1.2의 이미지를이 저장소에서 가져오기

```bash
docker pull splunk/splunk:7.1.2
```

### 최신 버전의 Splunk Enterprise를 사용하는 이미지를이 저장소에서 가져오기

```bash
docker pull splunk/splunk:latest
```

### Splunk Enterprise 컨테이너를 시작하고 자동으로 라이센스 계약에 동의

이 명령은이 저장소의 Docker 컨테이너에서 Splunk Enterprise 인스턴스를 시작하고 사용권 계약에 동의하며 TCP 포트 8000을 열어 로컬 시스템에서 Splunk 인스턴스에 액세스 할 수 있도록합니다.

```bash
docker run --name splunk --hostname splunk -p 8000:8000 -d -e "SPLUNK_START_ARGS=---accept-license --seed-passwd <your password>" splunk/splunk:7.1.2
```

### Splunk Enterprise 컨테이너를 시작하고 필요한 컨테이너 볼륨을 마운트

```bash
docker run --name vsplunk -v /opt/splunk/etc -v /opt/splunk/var busybox
docker run --hostname splunk --name splunk --volumes-from=vsplunk -p 8000:8000 -d -e "SPLUNK_START_ARGS=--accept-license --seed-passwd <your password>" splunk/splunk:7.1.2
```

> 참고 :
>
> 데이터 볼륨 컨테이너를 사용할 때, docker는 이미지 필수 볼륨을 /var/lib/docker/volumes/... 아래에 자동으로 제공합니다.
>
> 자세한 내용은 링크를 참조하십시오. [link to Docker documentation](https://docs.docker.com/engine/tutorials/dockervolumes/#locating-a-volume)

### Splunk Enterprise 컨테이너를 시작하고 호스트에서 볼륨을 마운트

```bash
docker run --name splunk --hostname splunk -p 8000:8000  -e "SPLUNK_START_ARGS=--accept-license --seed-passwd <your password>" -v /opt/splunk/etc:/opt/splunk/etc -v /opt/splunk/var:/opt/splunk/var  splunk/splunk:7.1.2
```

### entrypoint.sh를 사용하여 Splunk 명령을 실행하십시오

다음 명령을 입력하여 컨테이너에서 명령을 실행할 수 있습니다.

```bash
docker exec splunk entrypoint.sh splunk version
```

entrypoint.sh에서 사용할 수있는 명령에 대한 자세한 내용은 Splunk 설명서의 [Administrative CLI commands](https://docs.splunk.com/Documentation/Splunk/latest/Admin/CLIadmincommands)를 참조하십시오.

entrypoint.sh를 사용하여 환경 변수로 Splunk 서비스를 구성 할 수도 있습니다. [Basic configuration with environment variables](#basic-configuration-with-environment-variables)을 참조하십시오.

## Splunk Enterprise Docker 컨테이너를 [docker-compose](https://docs.docker.com/compose/)로 구성

1. 쉘 프롬프트에서 `docker-compose.yml` 텍스트 파일을 아직 작성하지 않은 경우 작성하십시오.
2. 편집을 위해 `docker-compose.yml`을 엽니다.
3. 다음 텍스트 블록을 파일에 삽입하십시오.
    ```yuml
    version: '3'

    volumes:
    opt-splunk-etc:
    opt-splunk-var:

    services:
    splunkenterprise:

        hostname: splunkenterprise
        image: splunk/splunk:7.1.2
        environment:
        SPLUNK_START_ARGS: --accept-license --seed-passwd <your password>
        SPLUNK_ENABLE_LISTEN: 9997
        SPLUNK_ADD: tcp 1514
        volumes:
        - opt-splunk-etc:/opt/splunk/etc
        - opt-splunk-var:/opt/splunk/var
        ports:
        - "8000:8000"
        - "9997:9997"
        - "8088:8088"
        - "1514:1514"
    ```
4. 파일을 저장하고 닫으십시오.
5. 같은 디렉토리에서 `docker-compose` 유틸리티를 실행하십시오.
    ```bash
    docker-compose up
    ```

## 설정

### 이미지 변형

`splunk/splunk` 이미지는 여러 변형이 있습니다 :

`splunk/splunk:7.1.2`
이것이 기본 Splunk Enterprise 이미지입니다.

`splunk/splunk:6.5.3-monitor`
이 이미지는 일부 데이터 입력 (예 : 도커 호스트 JSON 로그, HTTP 이벤트 수집기, Syslog 등의 파일 모니터)이 활성화 된 상태로 제공됩니다. 또한 수집 된 로그를 분석하고 통계, 이벤트, 상단과 같은 도커 정보를 분석하고 실행중인 이미지를 검사 할 수 있도록 도와주는 Docker 앱이 포함되어 있습니다.

### 데이터 저장소

이 Docker 이미지에는 두 가지 데이터 볼륨이 있습니다.

*`/opt/splunk/etc` - 응용 프로그램 및 조회를 포함하여 Splunk 구성을 저장합니다.
*`/opt/splunk/var` - 인덱싱된 데이터, 로그 및 내부 Splunk 데이터를 저장합니다.

### 사용자

모든 Splunk 프로세스는 기본적으로 `splunk` 사용자로 실행됩니다. 사용자는 SPLUNK_USER 환경 변수를 설정하여 변경할 수 있습니다.

### 포트

이 Docker 컨테이너는 다음 네트워크 포트를 노출합니다.

*`8000/tcp` - Splunk 웹 인터페이스
*`8088/tcp` - HTTP 이벤트 수집기
*`8088/tcp` - Splunk 서비스
*`8191/tcp` - 응용 프로그램 키 값 저장
*`9997/tcp` - Splunk 유니버셜 포워더가 일반적으로 사용하는 포트 (기본값으로 사용되지 않음)
*`1514/tcp` - 네트워크 입력 (기본값으로 사용되지 않음) 일반적으로 syslog TCP 데이터를 수집하는 데 사용됩니다

이 Docker 이미지는 1024 이하의 네트워크 포트에 루트 액세스가 필요하기 때문에 syslog 포트의 표준 포트 514 대신 포트 1514를 사용합니다. [Run Splunk Enterprise as a different or non-root user](http://docs.splunk.com/Documentation/Splunk/latest/Installation/RunSplunkasadifferentornon-rootuser)을 참조하십시오.

### Hostname

이 Docker 이미지를 사용할 때, 그것의 `hostname`을 설정하십시오. 나중에 인스턴스를 다시 만들면 이미지에 호스트 이름이 유지됩니다.

### 환경 변수를 이용한 기본 설정

인덱서 및 전달자의 기본 구성에 환경 변수를 사용할 수 있습니다. 고급 구성을 위해서는 컨테이너 내에 구성 파일을 만들거나 Splunk 배포 서버를 사용하여 인스턴스에 구성을 제공하십시오.

* `SPLUNK_ENABLE_DEPLOY_SERVER='true'` - Indexer에 배포 서버를 활성화합니다.
* `SPLUNK_DEPLOYMENT_SERVER='<servername>:<port>` - [configure deployment client](http://docs.splunk.com/Documentation/Splunk/latest/Updating/Configuredeploymentclients).
    배포 서버 URL을 설정하십시오..
  * Example: `--env SPLUNK_DEPLOYMENT_SERVER='splunkdeploymentserver:8089'`.
* `SPLUNK_ENABLE_LISTEN=<port>` - enable [receiving](http://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Enableareceiver).
  * Additional configuration is available using `SPLUNK_ENABLE_LISTEN_ARGS`
        environment variable.
* `SPLUNK_FORWARD_SERVER=<servername>:<port>` - [forward](http://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Deployanixdfmanually)
    data to indexer.
  * Additional configuration is available using `SPLUNK_FORWARD_SERVER_ARGS`
        environment variable.
  * Additional forwarders can be set up using `SPLUNK_FORWARD_SERVER_<1..30>`
        and `SPLUNK_FORWARD_SERVER_<1..30>_ARGS`.
  * Example: `--env SPLUNK_FORWARD_SERVER='splunkindexer:9997' --env
        SPLUNK_FORWARD_SERVER_ARGS='method clone' --env
        SPLUNK_FORWARD_SERVER_1='splunkindexer2:9997' --env
        SPLUNK_FORWARD_SERVER_1_ARGS='-method clone'`.
* `SPLUNK_ADD='<monitor|add> <what_to_monitor|what_to_add>'` - execute add command,
    for example to [monitor files](http://docs.splunk.com/Documentation/Splunk/latest/Data/MonitorfilesanddirectoriesusingtheCLI)
    or [listen](http://docs.splunk.com/Documentation/Splunk/latest/Data/Monitornetworkports) on specific ports.
  * Additional add commands can be executed (up to 30) using
        `SPLUNK_ADD_<1..30>`.
  * Example `--env SPLUNK_ADD='udp 1514' --env SPLUNK_ADD_1='monitor /var/log/*'`.
* `SPLUNK_CMD='any splunk command'` - execute any splunk command.
  * Additional commands can be executed (up to 30) using
        `SPLUNK_CMD_<1..30>`.
  * Example `--env SPLUNK_CMD='edit user admin -password random_password -role
        admin -auth admin:changeme'`.

#### 예

다음은 Docker에서 Splunk Enterprise 및 Splunk universal 전달자를 구성하는 방법의 예입니다.

```bash
> echo "Creating docker network, so all containers will see each other"
> docker network create splunk
> echo "Starting deployment server for forwarders"
> docker run -d --net splunk \
    --hostname splunkdeploymentserver \
    --name splunkdeploymentserver \
    --publish 8000 \
    --env SPLUNK_ENABLE_DEPLOY_SERVER=true \
    splunk/splunk
> echo "Starting Splunk Enterprise"
> docker run -d --net splunk \
    --hostname splunkenterprise \
    --name splunkenterprise \
    --publish 8000 \
    --env SPLUNK_ENABLE_LISTEN=9997 \
    splunk/splunk
> echo "Starting forwarder, which forwards data to Splunk"
> docker run -d --net splunk \
    --name forwarder \
    --hostname forwarder \
    --env SPLUNK_FORWARD_SERVER='splunkenterprise:9997' \
    --env SPLUNK_FORWARD_SERVER_ARGS='-method clone' \
    --env SPLUNK_ADD='udp 1514' \
    --env SPLUNK_DEPLOYMENT_SERVER='splunkdeploymentserver:8089' \
    splunk/universalforwarder
```

> 이 스크립트를 실행 한 후에는 컨테이너 *forwarder*의 *udp* 포트에 syslog 데이터를 전달할 수 있습니다 (내부 컨테이너의 경우 Splunk는 포트를 게시하지 않으므로). 데이터는 Splunk Enterprise에 도착해야하며 배포 서버에 등록 된 전달자를 확인해야합니다.

## 이미지 문제 해결

### 기본 문제 해결

Docker 앱에서 Docker 개요 앱을로드 할 때 데이터가 표시되지 않으면 다음을 확인하십시오.

* 올바른 환경 변수로 컨테이너를 시작했습니다. 특히, docker 호스트가 수집하는 기본 JSON 로그 파일을 읽으려면 마운트 지점에 대한 적절한 액세스 제어 권한이 있어야합니다. 자세한 내용은 [Required Permissions] (필수 권한 #)을 참조하십시오.
* Docker 이미지에 필요한 볼륨을 포함 시켰습니다.
* Docker 컨테이너에 올바른 파일 시스템 권한이 있습니다.

### 필요 권한

다음 탑재 지점에는 특수 사용 권한이 필요합니다.

*`/var/lib/docker/containers` : 기본적으로 Docker 호스트는 루트 사용자에게만 읽기 액세스 권한을 제공합니다. Splunk 프로세스를 시작한 모든 사용자는 볼륨에 대한 읽기 액세스 권한을 변경할 수 있습니다.
*`/var/run/docker.sock` - docker stats, tops와 같은 정보를 수집하려면 [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api/)에 액세스해야합니다. , 사건들과 검사.

SPLUNK_USER 환경 변수를 권한이 부여 된 사용자 (예 : "root")에게 다시 정의하면 Docker 응용 프로그램이 수집 된 Docker 정보를 분석하는 데 필요한 마운트 지점에 대한 필수 액세스 권한이 제공됩니다.

## docker-compose의 업그레이드 문제 해결

`docker-compose` (또는`docker run`과 함께 기존 볼륨 참조)를 사용하여 Docker 이미지를 구성하고 실행하면 `docker-compose.yml`을 변경 한 후에 Splunk Enterprise Docker 컨테이너가 업그레이드를 감지합니다. 이미지를 업그레이드 프롬프트를 무시하도록 다음 절차를 완료하십시오.

1. 편집을 위해 `docker-compose.yml`을 엽니다.
2. Splunk Enterprise 이미지의 `Environment :`섹션에 다음 행을 추가하십시오.
    ```bash
    SPLUNK_START_ARGS: --accept-license --seed-passwd <your password>
    ```
3. `docker-compose.yml`을 저장하고 닫습니다.
4. `docker-compose up`을 다시 실행하십시오.

## 추가사항

Splunk Enterprise Docker 이미지로 데이터를 수집하거나 분석하는 데 여전히 문제가있는 경우 다음 옵션 중 하나를 사용하십시오.

* [Splunk Answers](http://answers.splunk.com)에 질문을 게시하십시오.
* [Splunk Slack channel](http://splunk-usergroups.slack.com)
* [EFNet Internet Relay Chat](http://www.efnet.org)의 #splunk 채널 방문
* [docker-maint@splunk.com](mailto:docker-maint@splunk.com)으로 전자 메일 보내기

## Splunk Dev

```bash
docker exec -i -t splunkpublic /bin/bash
```