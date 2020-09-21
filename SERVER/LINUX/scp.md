# SCP

1 다른 서버로 복사 (보내기)
1.1 문법
1.2 예시
2 다른 서버에서 복사 (가져오기)
3 같이 보기
4 참고

## 다른 서버로 복사 (보내기)

### 1 문법

파일 보내기
> scp 파일 계정@서버주소:목적경로

디렉토리 보내기
> scp -r 디렉토리 계정@서버주소:목적경로

### 2 예시

```bash
# 파일보내기
scp test.txt testuser@135.79.246.80:/home/testuser/
→ test.txt를 135.79.246.80 서버의 /home/testuser/ 폴더에 업로드
```

## 다른 서버에서 복사 (가져오기)

기본 포트 사용
> scp 계정@서버주소:원본경로 목적파일명

다른 포트 사용
> scp -P 포트 계정@서버주소:원본경로 목적파일명

폴더 복사
> scp -r 계정@서버주소:원본경로 목적상위폴더

```bash
scp -r testuser@135.79.246.81:/var/www/html/ /var/www/
```

## 리눅스 scp 다른 포트 접속/리눅스 scp 다른 포트 사용

scp -P 옵션
> 기본 SSH 포트 22번 대신 다른 포트를 사용하려면 -P 옵션을 쓰면 된다.

원격 → 로컬
> scp -P 포트 계정@서버주소:경로/파일명 경로/파일명

로컬 → 원격
> scp -P 포트 경로/파일명 계정@서버주소:경로/파일명

## scp 자동화

### 1 방법 1: sshpass + scp

명령어

```bash
sshpass -p패스워드 scp -o StrictHostKeyChecking=no 로컬파일 아이디@호스트주소:/폴더/파일명
```

실행예시

```bash
[root@zetawiki ~]# sshpass -pP@ssw0rd scp -o StrictHostKeyChecking=no hello.txt root@135.79.246.99:/root/hello.txt
```

→ 135.79.246.80(jmnote)에서 135.79.246.99(jmtest01)로 hello.txt 복사

### 2 방법 2: expect + scp

명령어

```bash
expect <<EOF
spawn scp -oStrictHostKeyChecking=no hello.txt 아이디@호스트주소:/폴더/위치/
expect "password:"
        send "패스워드\r"
expect eof
EOF
```

실행예시

```bash
[root@zetawiki ~]# expect <<EOF
> spawn scp -oStrictHostKeyChecking=no hello.txt root@135.79.246.99:/root/
> expect "password:"
>         send "P@ssw0rd\r"
> expect eof
> EOF

spawn scp -oStrictHostKeyChecking=no hello.txt root@135.79.246.99:/root/
root@135.79.246.99's password:
hello.txt                              100%    6     0.0KB/s   00:00
```

## 4 참고

<http://en.wikipedia.org/wiki/Secure_copy>
<http://sangchul.kr/4522378>