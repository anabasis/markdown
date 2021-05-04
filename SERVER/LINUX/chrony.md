# Chrony

```bash
sudo yum install chrony
sudo apt install chrony

sudo systemctl enable chronyd
sudo systemctl start chronyd

sudo systemctl restart chronyd
```

- /etc/chrony.conf

```bash
# server 0.centos.pool.ntp.org iburst
# server 1.centos.pool.ntp.org iburst
# server 2.centos.pool.ntp.org iburst
# server 3.centos.pool.ntp.org iburst

server time.bora.net iburst
server send.mx.cdnetworks.com iburst
```

- 확인

```bash
# timedatectl  동기화 설정 확인
timedatectl set-timezone Asia/Seoul

 Local time: Sat 2020-01-11 17:19:18 KST
  Universal time: Sat 2020-01-11 08:19:18 UTC
        RTC time: Sat 2020-01-11 08:19:18
       Time zone: Asia/Seoul (KST, +0900)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a
```

```bash
# chronyc sources -v 정상 연결 여부 확인

210 Number of sources = 2

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^? time.bora.net                 0   6     0     -     +0ns[   +0ns] +/-    0ns
^* send.mx.cdnetworks.com        2   6    17     9  +4376ns[ +135us] +/-   47ms
```
