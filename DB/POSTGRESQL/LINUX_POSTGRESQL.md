# Linux PostgreSQL

## 설치

```bash
# 설치
$sudo yum update -y
$sudo yum install -y postgresql postgresql-server postgresql-devel postgresql-contrib postgresql-docs

# 초기화
$sudo service postgresql initdb

# 기동
$sudo systemctl start postgresql
$sudo systemctl enable postgresql
```

- postgres계정으로 postgresql에 접속

```bash
$ sudo -u postgres psql
could not change directory to "/home/ec2-user"
psql (9.2.24)
Type "help" for help.

postgres=#
```

- postgresql 기본 계정인 postgres의 비번을 설정
  
```sql
postgres=# ALTER USER postgres WITH PASSWORD 'ngle123456';
ALTER ROLE
```

## Database 생성

```sql
postgres=# CREATE USER ngle SUPERUSER;
CREATE ROLE
postgres=# ALTER USER ngle WITH PASSWORD 'ngle123456';
ALTER ROLE
```

- Database : ngledb
- 계정 : ngle
- 기본 encoding은 SQL_ASCII.
- UTF8로 지정하려면 database를 생성할 때 설정
- 그렇지 않으면 database를 삭제하고 다시 만들어야함
- UTF8로 지정하려면 template을 template0으로 지정해야 합니다.

```sql
postgres=# create database ngledb with owner ngle encoding 'UTF8' template template0;
CREATE DATABASE
```

- database 생성과 권한(소유권)변경이 잘 되었는지 \l 명령으로 확인

```sql
postgres=# \l
                              List of databases
    Name    |  Owner   | Encoding  | Collate | Ctype |   Access privileges
------------+----------+-----------+---------+-------+-----------------------
 ngledb     | ngle     | UTF8      | C       | C     |
 postgres   | postgres | SQL_ASCII | C       | C     |
 template0  | postgres | SQL_ASCII | C       | C     | =c/postgres          +
            |          |           |         |       | postgres=CTc/postgres
 template1  | postgres | SQL_ASCII | C       | C     | =c/postgres          +
            |          |           |         |       | postgres=CTc/postgres
(4 rows)
```

## 외부접속설정

- pg_hba.conf 파일을 수정
- #IPv4 local connections : 부분을 아래와 같이 수정합니다.

```sql
 # "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             ngle            0.0.0.0/0               md5
host    all             postgres        0.0.0.0/0               md5
# IPv6 local connections:
host    all             all             ::1/128                 ident
```

- postgresql.conf 파일을 수정
- postgresql.conf 파일의 59라인의 내용(#listen_addresses = 'localhost')을 어디에서든 접속할 수 있도록 수정

    ```properties
    listen_addresses = '*'
    ```

- 두 개의 conf 파일이 수정되었다면 postgresql을 재시작

    ```bash
    $sudo systemctl restart postgresql
    ```
