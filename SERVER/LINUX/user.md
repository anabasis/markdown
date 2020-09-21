# 사용자 관리

참고자료 :
<https://freehoon.tistory.com/39>

## /etc/passwd

root : x : 0 : 0 : root : /root : /bin/bash
 1     2   3   4   5      6       7

1. 사용자 계정 ID
2. 패스워드
3. 사용자 UID
4. 그룹 GID
5. 계정정보(보통은 사용자 이름)
6. 홈 디렉토리
7. 쉘 환경

```bash
cut  -f1 -d: /etc/passwd

##UID가 500 이상인 계정중에 'us'를 포함하는 계정
awk -F':' '{if($3>=500)print $1}' /etc/passwd
```

## 계정등록

- useradd : 계정ID 만 생성 (홈디렉토리 등 설정 X)
- adduser : 계정ID 및 홈디렉토리, 계정정보 및 비밀번호 셋팅 기본으로 설정

> CentOS7 에서 테스트 한 결과 useradd와 adduser 의 결과는 같았

```bash
useradd -d /home/user05 -u 600 -s /bin/csh user05
```

 1. useradd : 사용자 계정 추가 명령어
 2. -d : 디렉토리 생성 옵션 (/home/user05 디렉토리 생성)
 3. -u : 사용자 UID 설정 (UID를 600 으로 설정)
 4. -s : 사용자 쉘 환경 설정 (/bin/csh 로 설정)
 5. user05 : 사용자 계정 ID

<table>
<tr><td>옵션</td><td>옵션설명</td></tr>
<tr><td>-c</td><td>계정설명, 대부분 사용자명 입력, finger 명령어로 확인 가능한 간단한 사용자 설명</td></tr>
<tr><td>-d</td><td>사용자 홈디렉토리 경로 설정</td></tr>
<tr><td>-m</td><td>사용자 홈디렉토리 생성</td></tr>
<tr><td>-e</td><td>사용자 계정의 사용 종료 일자</td></tr>
<tr><td>-f</td><td>사용자 계정의 유효 기간 (ex: -f 180  계정 생성일로부터 180동안만 사용가능)</td></tr>
<tr><td>-g</td><td>사용자 계정의 로그인 그룹</td></tr>
<tr><td>-G</td><td>사용자 계정의 추가 들록 계정의 그룹명</td></tr>
<tr><td>-p</td><td>사용자 계정의 패스워드</td></tr>
<tr><td>-s</td><td>사용자 계정의 로그인 쉘</td></tr>
<tr><td>-u</td><td>사용자 계정의 UID</td></tr>
</table>

## 계정 삭제

- userdel 의 경우
  - 계정만 삭제 # userdel 계정명
  - 계정과 홈디렉토리 삭제 # userdel -r 계정명
- deluser의 경우
  - 계정만 삭제 # deluser 계정명
  - 계정과 홈디렉토리 삭제 # deluser --remove 계정명
  - 계정과 홈디렉토리, 계정명으로된 모든 파일 삭제 # deluser --remove-all-files

> CentOS7 에서는 deluser 명령이 없음 : -bash: deluser: command not found

## 계정 권한 변경

<table>
<tr><td>타입</td><td>설명</td><tr>
<tr><td>-</td><td>plain file, 일반적인 파일이 여기에 해당합니다. (실행 파일도 포함)</td><tr>
<tr><td>-d</td><td>directory</td><tr>
<tr><td>-l</td><td>link. 다른 파일을 가리키는 링크파일</td><tr>
<tr><td>-p</td><td>pipe. 두개의 프로그램을 연결하는 파일</td><tr>
<tr><td>-b</td><td>block device. 블록단위로 하드웨어와 반응하는 파일</td><tr>
<tr><td>-c</td><td>character device. 스트림 단위로 하드웨어와 반응하는 파일</td><tr>
</table>

## 그룹생성 및 삭제, 관리

시스템용 그룹(GID 499 이하)

/etc/group

```bash
groups

groupadd -g 1000 user01
groupadd -r user02

groupdel user01
```

gpasswd 옵션

<table>
<tr><td>옵션</td><td>옵션설명</td></tr>
<tr><td>-a</td><td>특정 그룹웨 새로운 계정을 등록.</td></tr>
<tr><td>-d</td><td>특정 그룹웨서 지정한 계정을 삭제.</td></tr>
<tr><td>-r</td><td>특정 그룹 패스워드를 제거.</td></tr>
<tr><td>-R</td><td>특정 그룹웨 접근을 제한.</td></tr>
<tr><td>-A</td><td>특정 그룹의 관리자를 설정</td></tr>
<tr><td>-M</td><td>특정 그룹의 계정을 새로 설정 ( 이 경우 기존 계정은 무시함)</td></tr>
</table>

```bash
gpasswd -A nestgoer wheel

grep wheel /etc/gshadow
wheel::nestgoer:nestgoer,zezz

grep wheel /etc/gshadow
wheel::nestgoer:nestgoer,zezz

grep wheel /etc/gshadow
wheel::nestgoer:nestgoer,zezz
```
