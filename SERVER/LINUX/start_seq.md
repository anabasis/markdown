# bash 쉘 초기화 파일 실행 순서

**profile bashrc bash_profile 실행 순서**
**profile bashrc bash_profile 호출 순서**

## 목차

1 실행순서 (간단히 정리)
2 호출 순서
3 코드분석
4 실행순서 (정확히)
5 같이 보기
6 주석
7 참고

### 1. 실행순서

기본 설정에서 실행 순서는 다음과 같다.

각 파일의 맨 아랫부분에 실행 코드를 추가할 때의 기준
특별히 코드를 변경하지 않았다면 이 순서대로 실행
실행코드를 하단에 추가하는 것이 보통이므로 이 정도만 알아두면 OK.

```bash
/etc/profile.d/test.sh[1]
/etc/profile
/etc/bashrc
~/.bashrc
~/.bash_profile
```

### 2. 호출 순서

기본 설정에서 호출 순서

각 파일의 맨 윗부분에 실행 코드를 추가할 때의 기준
/etc/profile → /etc/profile.d/test.sh
~/.bash_profile → ~/.bashrc → /etc/bashrc

### 3 코드분석

~/.bash_profile에 다음과 같은 내용

```bash
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
```

--> ~/.bashrc 가 있으면 그것을 실행

~/.bashrc 에 다음과 같은 내용

```bash
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

--> /etc/bashrc가 있으면 그것을 실행

/etc/profile에는 다음과 같은 내용

```bash
for i in /etc/profile.d/*.sh ; do
    if [ -r "$i" ]; then
        if [ "$PS1" ]; then
            . $i
        else
            . $i >/dev/null 2>&1
        fi
    fi
done
```

--> /etc/profile.d 폴더 내의 모든 sh 파일을 실행

실행파일의 호출 순서를 정리하면 다음과 같음.
(OS는 /etc/profile과 ~/.bash_profile 2개를 순서대로 호출할 뿐인데, 각 파일이 내부적으로 다른 파일을 호출하는 것이다.)

리눅스 → /etc/profile → /etc/profile.d/test.sh
리눅스 → ~/.bash_profile → ~/.bashrc → /etc/bashrc
이것을 각 파일의 마지막에 코드를 추가했을 때 기준으로 실행순서를 정리하면, 맨 위 문단에서 설명한 바와 같음

/etc/profile.d/test.sh → /etc/profile
/etc/bashrc → ~/.bashrc → ~/.bash_profile

### 4. 실행순서 (정확히)

각 파일에 아래와 같은 형식으로 명령어를 추가하여 추적해보았다.

```bash
NOW=`date +%Y-%m-%d\ %H:%M:%S`
echo "$NOW [/etc/profile.d/a.sh]" >> /root/run_order.txt

[root@zetawiki ~]# cat run_order.txt
2013-08-03 07:47:28 [START OF /etc/profile]
2013-08-03 07:47:28 [/etc/profile.d/a.sh]
2013-08-03 07:47:28 [/etc/profile.d/b.sh]
2013-08-03 07:47:28 [END OF /etc/profile]
2013-08-03 07:47:28 [START OF ~/.bash_profile]
2013-08-03 07:47:28 [START OF ~/.bashrc]
2013-08-03 07:47:28 [START OF /etc/bashrc]
2013-08-03 07:47:28 [END OF /etc/bashrc]
2013-08-03 07:47:28 [END OF ~/.bashrc]
2013-08-03 07:47:28 [END OF ~/.bash_profile]
```

### 5. 같이 보기

```bash
/etc/profile
/etc/bashrc
~/.bashrc
~/.bash_profile
```

Bash 쉘 초기화 파일

### 6. 주석

 여기서는 test.sh라고 했지만 어떤 이름이든 상관없다. /etc/profile.d에 있는 모든 sh 파일이 여기에 해당된다.

### 7. 참고

<http://www.thegeekstuff.com/2008/10/execution-sequence-for-bash_profile-bashrc-bash_login-profile-and-bash_logout/>

---------------------------------------------------------------------

```bash
# 대상
/etc/init.d/test
/etc/profile.d/test.sh
/etc/profile
/etc/bashrc
~/.bashrc
~/.bash_profile

vi /etc/init.d/test
vi /etc/profile.d/test.sh
vi /etc/profile
vi /etc/bashrc
vi ~/.bashrc
vi ~/.bash_profile

# 삽입
NOW=`date +%Y-%m-%d\ %H:%M:%S`
echo "START $NOW [/etc/profile.d/test.sh]" >> /root/run_order.txt

NOW=`date +%Y-%m-%d\ %H:%M:%S`
echo "END $NOW [/etc/profile.d/test.sh]" >> /root/run_order.txt
```

```bash
START 2018-03-12 09:49:39 [/etc/profile]
START 2018-03-12 09:49:39 [/etc/profile.d/test.sh]
START 2018-03-12 09:49:39 [/etc/profile.d/testa.sh]
END 2018-03-12 09:49:39 [/etc/profile]
START 2018-03-12 09:49:40 [~/.bash_profile]
START 2018-03-12 09:49:40 [~/bashrc]
START 2018-03-12 09:49:40 [/etc/bashrc]
END 2018-03-12 09:49:40 [/etc/bashrc]
END 2018-03-12 09:49:40 [~/bashrc]
END 2018-03-12 09:49:40 [~/.bash_profile]
```