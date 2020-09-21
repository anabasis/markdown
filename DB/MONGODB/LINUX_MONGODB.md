# Linux MongoDB

## 설치

mongodb-org 패키지가 CentOS의 기본 리포지토리에 포함되어 있지 않기 때문에 리포지토리 파일을 생성하여 추가

```bash
sudo vi /etc/yum.repos.d/mongodb-org.repo

[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc

yum repolist

sudo yum install -y mongodb-org

sudo systemctl start mongod

sudo systemctl reload mongod
```

## YUM mongodb 업데이터 방지 설정

```bash
vi /etc/yum.conf

exclude=mongodb-org,mongodb-org-server,mongodb-org-shell,mongodb-org-mongos,mongodb-org-tools
```

## Selinux mongdb 포트 오픈

```bash
semanage port -a -t mongod_port_t -p tcp 27017
```

## 실행

```bash
mongo
```

## 외부접속 및 보안접근

```bash
vi /etc/mongod.conf

net
  bind_ip = 127.0.0.1
  bind_ip = 0.0.0.0

security:
  authorization: enabled
```

## 사용자 생성

```js
use admin # admin 데이터베이스 선택

db.createUser({
  user: "admin",  # 계정 이름
  pwd: "password",  # 비밀번호
  roles: [          # 사용자에게 주어진 권한 목록. 여러 데이터베이스에 대한 권한을 할당할 수 있다.
    {
      role: "root", # built-in 권한인 root. 문자 그대로 모든 데이터베이스를 관리할 수 있다.
      db: "admin"   # 어떤 데이터베이스에 대한 권한인지 명시
    }
  ]}
)
```

```js
use board

db.createUser({
  user: "board-user",
  pwd: "password",
  roles: [
    {
      role: "readWrite", # 읽기, 쓰기 권한
      db: "board"         # 위의 권한을 부여할 데이터베이스로 test를 지정
    }
  ]}
)
```
