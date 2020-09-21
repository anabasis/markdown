# NTP

NTP(Network Time Protocol)는 인터넷상의 시간을 정확하게 유지시켜 주기 위한 통신망 시간 규약이다. 라디오나 원자시계에 맞추어 시간을 조정하며 밀리초 1/1000초 단위까지 시간을 맞출 수 있다.

우리나라에서 운영되고 있는 NTP 서버

- kr.pool.ntp.org
- time.bora.net

## 설치

```bash
# Ubuntu
sudo apt-get install ntp
# CentOS
sudo yum install ntp
```

## 설정

sudo vi /etc/ntp.conf

server 1.kr.pool.ntp.org
server 1.asia.pool.ntp.org
server time.bora.net

## 재시작

```bash
sudo service ntp restart
```

```bash
sudo ntpq -p

remote refid st t when poll reach delay offset jitter
=====================================================================
+121.182.147.191 141.223.182.106 2 u 840 1024 377 10.594 -0.556 0.627
xtime.bora.net .M-L{^B^E. 16 u 538 1024 276 7.123 -132.68 114.924
*ntp1.sjtel.net 192.168.18.6 2 u 1026 1024 377 6.113 -1.264 0.832
+europium.canoni 193.79.237.14 2 u 837 1024 377 284.452 -0
.044 2.758
```

- `*`는 현재 sync 를 받고 있음을 의미
- `+` 는 ntp 알고리즘에 의해 접속은 가능하지만 sync 를 하고 있지는 않음을 의미
- `-` 는 ntp 알고리즘에 의해 접속은 가능하지만 sync 가능 리스트에서 제외
- blank는 접속이 불가능함을 의미

remote 는 sync 를 하는 straum 2 서버주소
refid 는 각 straum 2 서버가 현재 sync 를 하고 있는 straum 1 서버를 보여줌
st 가 16일 경우 해당 서버에 접속 할 수 없음

NTP는 udp 포트 123 를 사용

- server 127.127.1.0  # 로컬 타임 서버로 동작하기 위한 설정 꼭 127.127.1.0 입력
- /etc/ntp.conf와 driftfile 과 keys의 경우 배포본마다 위치가 다를 수 있으니 확인을 하도록 한다.
- restric 설정은 peer 들이 본 서버로 sync 하는 것에 대한 제한을 한다.
- restrict default nomodify notrap noquery 설정은 기본으로 모든 권한을 주지 않음을 의미한다.
- restrict 127.0.0.1 설정은 127.0.0.1 즉, 서버 자신에서는 모든 권한을 가진다.
- restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap 설정은 192.168.0.0 ~ 192.168.0.255 c class 에서는 질의를 할 수 있는 권한을 가진다. 즉, 위의 2 라인은 항상 기본으로 들어가는 설정이며, peer 를 거느릴 서버에서는 (즉 A 의 입장에서는) 하위 peer 들의 질의를 받을수 있도록 3 번째 라인과 같이 restrict 설정을 해 주어야 한다.

## 명령어

### Server에서 확인

```bash
[root@localhost ~]# ntpq -pn
remote         refid     st t when poll reach delay   offset  jitte     r
==========================================================================
*127.127.1.0  .LOCL.     5 l  51   64   17    0.000   0.000   0.00      0
```

### Client에서 확인

```bash
[root@localhost ~]# ntpq -p
remote           refid      st t when poll reach delay   offset  jitte
==========================================================================
*192.168.0.127   LOCAL(0)   6 u  55   64   177   0.200   0.463   0.257
```

### Client에서 수동으로 동기화

```bash
[root@localhost ~]ntpdate 192.168.0.127
```

### OS 시간을 CMOS 시간으로 변경

```basg
[root@localhost ~]# clock -w
```

### 원하는 시간으로 변경

```bash
[root@localhost ~]# date -s 시:분:초
```

### 현재 시간 확인

```bash
[root@localhost ~]# date
```

### 외부 시간과 동기화(time.bora.net)

```bash
[root@localhost ~]# rdate -s time.bora.net
```

## 우선순위

```bash
[root@localhost ~]# vi /etc/ntp.conf
   ...
restrict default kod nomodify notrap nopeer noquery
restrict 127.0.0.1  # local clock
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

127.127.1.0 이 localtime이고 stratum이 우선 순위이다.
낮을 수록 우선하게 되는데 10을 줌으로써 localtime의 우선순위를 낮춘다.

혹은

```bash
[root@localhost ~]vi /etc/ntp.conf
/etc/ntp.conf
...
server 192.168.0.127 prefer
```

prefer 옵션을 주면 기준이 된다.
