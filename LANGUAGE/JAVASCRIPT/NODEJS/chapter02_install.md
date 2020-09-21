# NodeJS

## Nodejs 특징

- 비동기 I/O 처리 / 이벤트 위주: Node.js 라이브러리의 모든 API는 비동기식(Non-blocking)
- Node.js 기반 서버는 API가 실행되었을때, 데이터를 반환할때까지 기다리지 않고 다음 API 를 실행하고 이전에 실행했던 API가 결과값을 반환할 시, NodeJS의 이벤트 알림 메커니즘을 통해 결과값을 받아옴
- 빠른 속도: 구글 크롬의 V8 자바스크립트 엔진을 사용하여 빠른 코드 실행을 제공
- 단일 쓰레드 / 뛰어난 확장성: Node.js는 이벤트 루프와 함께 단일 쓰레드 모델을 사용
- 이벤트 메커니즘은 서버가 멈추지않고 반응하도록 해주어 서버의 확장성
- 일반적인 웹서버는 (Apache) 요청을 처리하기 위하여 제한된 쓰레드를 생성
- Node.js 는 쓰레드를 한개만 사용하고 Apache 같은 웹서버보다 훨씬 많은 요청을 처리
- 노버퍼링: Node.js 어플리케이션엔 데이터 버퍼링이 없고, 데이터를 chunk로 출력
- 라이센스: Node.js 는 MIT License가 적용

## 설치

<https://nodejs.org/en/download/>

1) 윈도우/맥 설치 : 인스톨
2) 리눅스 설치

  ```bash
  # 데비안 계열(우분투)에서 일반 사용자 계정인 경우,
  $ sudo apt-get install curl
  $ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  $ sudo apt-get install -y nodejs

  # 데비안 계열(우분투)에서 루트인 경우,
  $ apt-get install curl
  $ curl -sL https://deb.nodesource.com/setup_6.x | bash -
  $ apt-get install -y nodejs

  # CentOS/RHEL
  $ curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
  $ curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -
  $ yum -y install nodejs

  # SOURCE COMPILE
  wget http://nodejs.org/dist/v0.10.24/node-v0.10.24.tar.gz

  tar zxvf node-v*.tar.gz
  cd node-v*

  ./configure
  make
  make install
  ```

## 설치확인

```bash
node --version
```

```js
# hello.js
console.log("Hello World!!!!");
```

```bash
node hello.js
```