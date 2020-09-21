# DNS

<http://coffeenix.net/board_view.php?cata_code=0&bd_code=1689&bpage=>

bind 네임서버는 네임서버관련한 여러 로그를 생성해서 모니터링에 도움을 준다. 이 로그는 주로 2가지 방식으로 처리를 한다. 하나는 syslog를 통해서 남기는 것이고, 다른 하나는 null로 버리는 것이다. 2가지 방법 모두 아쉬운 점이 있다. syslog에 함께 쌓기에는 다른 시스템 로그에 섞여서 보기 불편하고, 로그를 남기지 않기(null)에는 답답하다. bind는 이 딜레마의 명쾌한 답을 제시하고 있다.

1. syslog에 남은 네임서버 로그

다음은 네임서버의 security 카테고리의 로그가 syslog 통해서 저장된 예이다. security 카테고리에 포함된 로그들이어서 남길 필요성이 있지만, 이런 로그들이 많이 쌓이다보면 정작 봐야할 시스템 로그가 가려질 수 있는 문제도 있다. bind 네임서버의 logging 설정에는 로그 카테고리별로 '별도 파일'에 로그를 남기는 방법을 제공한다.

 	
Apr  2 06:59:56 ns1 named[620]: client 59.160.XXX.XXX#53109: query (cache) './A/IN' denied
Apr  2 06:59:56 ns1 named[620]: client 211.111.XXX.XXX#53801: query (cache) './A/IN' denied
Apr  2 07:00:02 ns1 named[620]: client 59.160.XXX.XXX#53129: query (cache) './A/IN' denied
Apr  2 07:00:02 ns1 named[620]: client 211.111.XXX.XXX#53812: query (cache) './A/IN' denied
Apr  2 07:00:08 ns1 named[620]: client 59.160.XXX.XXX#53150: query (cache) './A/IN' denied
Apr  2 07:00:08 ns1 named[620]: client 211.111.XXX.XXX#53826: query (cache) './A/IN' denied
Apr  2 07:00:14 ns1 named[620]: client 59.160.XXX.XXX#53174: query (cache) './A/IN' denied
Apr  2 07:00:14 ns1 named[620]: client 211.111.XXX.XXX#53840: query (cache) './A/IN' denied
 


2. named.conf 에서 logging 설정

 	
logging {
        category lame-servers { null; };
        category notify       { null; };

        channel default_debug { null; };
        channel debug_log {
                file "/var/log/named_debug.log" versions 10 size 20M;
                severity dynamic;
                print-category yes;
                print-severity yes;
                print-time yes;
        };

        category security { debug_log; };
        category xfer-in  { debug_log; };
        category xfer-out { debug_log; };
        category general  { debug_log; default_syslog; };
};
 


위의 logging 설정의 하나 하나가 정확히 어떤 것을 의미하는지 모르더라도, 짐작은 될 것이다.

1) notify, lame-servers 카테고리는 로그를 남기지 않는다.
2) security, xfer-in, xfer-out, general 카테고리(category security 항목)에 해당하는 로그를 /var/log/named_debug.log 파일로 저장한다. 파일 크기는 20MB 로 제한한다.

짐작했던 것이 맞았는가? 그렇다면 이제 상세하게 살펴보자.

1) file "파일명" : 로깅 파일의 경로를 지정한다. 만약 chroot된 상태로 운영한다면, 실제 저장경로는 chroot된 디렉토리 아래에 쌓인다. chroot 디렉토리가 /var/named라고 할 때, 실제로는 /var/named/var/log/ 아래에 쌓이게 되는 것이다. 네임서버가 다른 유저 권한으로 실행된다면(데몬 실행시 -u user명 옵션), 해당 유저에게 디렉토리 쓰기 권한이 있어야 한다.
2) versions 숫자 : 로이테션하여 보관할 로그 갯수를 지정한다. 로테이션이 되면 가장 최신 로그가 .0 이며, 그 이전의 로그는 .1, 그 이전은 .2, .3, .4... 처럼 로그 파일명이 정해진다. 'versions 99' 는 로테이션시 unlimited로 간주된다.
3) size 크기 : 파일 크기를 지정한다. 단위가 없으면 bytes단위. 10M은 10 MBytes를 뜻한다. 사용할 수 있는 단위는 k, K, m,M, g, G
4) versions와 size와의 관계를 살펴보자.
   ① size, versions 모두 설정한 경우 (일반적인 경우임)
      size값으로 설정한 크기에 도달하면 로그는 로테이션 된다.
   ② versions 파라미터가 없고, size 값만 설정한 경우 
      size로 지정한 크기까지 로그를 남기고 멈춘다. files이 다시 줄어든 경우(삭제나 truncate) 로그를 남긴다.
   ③ size 값 설정이 없고, versions만 설정한 경우
      bind가 재실행될 때만 로테이션 된다.

5) severity 레벨 : 

 	critical | error | warning | notice | info  | debug [ level ] | dynamic	 


severity로 지정할 수 있는 것은 위 7가지. 오른쪽으로 갈수록 보다 상세한 로그가 남는다. 이는 syslog의 level을 생각하면 된다. debug 레벨은 debug 0으로 지정하면 디버깅 모드를 사용하지 않는 것이며, 1~3까지 레벨을 지정하면 디버깅 모드로 로그(보다 상세한 로그)를 남긴다.

dynamic은 뭘까? debug 레벨로 로그를 남긴다. 그러나 이 로그 레벨이 0~3 중에서 하나로 고정되는 것이 아니라, 동적으로 변경할 수 있다는 의미이다. 동적으로 어떻게 변경할까? 2가지 방법이 있다. bind(데몬명 : named) 데몬을 실행할 때 -d '레벨'으로 지정하거나, rndc trace명령으로 debug 레벨을 변경할 수 있다. debug 레벨을 0으로 초기화하려면 rndc notrace 명령을 내리면 된다.

다음은 rndc에서 debug 레벨관련 관련 명령어이다.
 	
  trace　　　　　　Increment debugging level by one.
  trace level　　　Change the debugging level.
  notrace　　　Set debugging level to 0.
 


rndc status 명령으로 현재 debug 레벨을 확인할 수 있다.
 	
# rndc status
number of zones: ???
debug level: 0
... 생략 ...
server is up and running
 


6) print-time yes     : 로그 파일에 날짜, 시간 정보를 남긴다. default = no
7) print-category yes : 로그 카테고리 정보를 남긴다. default = no
8) print-severity yes : 로그 severity(중요도 정도, 이를테면 error, notice, info, debug 등)를 남긴다. default = no

위에서 반드시 print-time은 yes로 설정해줘야 모니터링이 편하며, 3개 모두 yes로 설정하는 것을 개인적으로 권장한다.

9) category 카테고리명 : 로그의 카테고리를 지정한다. 카테고리가 상당히 많으므로 bind 매뉴얼을 살펴보는게 좋다.

10) channel 채널명 :

channel은 logging 설정의 묶음이다. 동일한 설정을 여러 카테고리별로 사용할 때, 각 카테고리별로 반복 설정할 필요없이 하나의 channel명으로 정의를 해놓으면 된다. 위 logging 설정 중 category security 설정을 다시 보자.

 	
        category security { debug_log; };
 


category security는 debug_log 채널 설정을 따른다고 되어 있다. 결국 security 카테고리는 1) named_debug.log 로그 파일에, 2) 최대 20MB크기, 10개까지 로테이션, 3) 로그 파일에는 시간정보와 카테고리, 레벨 정보를 남기게 된다.

3. channel 설정 깊이 알기

이 부분은 모르고 넘어가도 되니, 급한 분은 '4. 네임서버 로그'로 넘어가기 바란다.
channel은 사용자가 정의하는 채널도 있지만, 미리 정의된 4개의 채널이 존재한다.

- default_syslog
- default_debug
- default_stderr
- null

각각 채널은 다음과 같이 미리 정의되어 있다.

 	
channel default_syslog {
        syslog daemon;        # send to syslog's daemon facility
        severity info;        # only send priority info and higher
};

channel default_debug {
        file "named.run";     # write to named.run in the working directory
                              # Note: stderr is used instead of "named.run"
                              # if the server is started with the "-f" option.
        severity dynamic;     # log at the server's current debug level
};

channel default_stderr {  # writes to stderr
        file "<stderr>";      # this is illustrative only; there's currently
                              # no way of specifying an internal file
                              # descriptor in the configuration language.
        severity info;        # only send priority info and higher
};

channel null {            
        null;                 # toss anything sent to this channel
};
 


syslog로 보내지는 로그는 default_syslog 채널의 설정을 적용받는다. 로그 분류(facility)는 daemon으로 되어 있고, info 레벨 이상의 로그만 syslog로 보내진다. 이를 변경하려면 default_syslog를 재정의하면 된다. local1~local6 등으로 facility를 설정하면 syslog로그로 보내진 로그도 별도 파일에 저장하기 편할 것이다.

debug 레벨 로그는 default_debug 채널의 설정을 적용받는다. rndc trace 명령으로 debug 레벨을 변경한 상태라면 named.run 파일(file "named.run"; 설정)에 로그를 저장하려고 할 것이다. 그러나 named.run 파일을 생성하거나 write할 퍼미션이 없다면 다음과 같이 로그가 syslog쪽에 남게 된다.

 	
Apr  2 08:06:42 ns1 named[17414]: loading configuration from '/etc/named.conf'
Apr  2 08:06:42 ns1 named[17414]: isc_log_open 'named.run' failed: permission denied
 


debug 레벨 로그가 필요없다면 다음과 같이 null 처리해 주는 것도 괜찮다.

 	
　　　channel default_debug { null; };
 


또는 별도 파일명을 지정해도 된다. 이 글 맨 위의 logging 설정에 다음 설정을 추가한다면 의미는 이런 의미를 갖게 된다. security, xfer-in, xfer-out, general 이외 카테고리의 debug 레벨 로그는 named_misc.log에 남게 된다.

 	
　　　channel default_debug {
　　　　　　file "/var/log/named_misc.log";
　　　　　　severity dynamic;

　　　　　　print-category yes;
　　　　　　print-severity yes;
　　　　　　print-time yes;
　　　};
 


다음과 같이 general 카테고리는 2개의 채널이 적어진 경우도 있다. 이는 2개의 채널 설정을 모두 따르겠다는 의미이다. 즉, debug 로그 파일에도 저장이 되고, syslog로도 보내진다.

 	
　　　category general  { debug_log; default_syslog; };
 


general 카테고리는 네임서버와 관련된 기본적 로그들이 많다. 따라서 general 카테고리를 별도 정의하고 싶다면 syslog에도 보내도록 설정해주는 것이 좋다.

4. 네임서버 로그

위처럼 logging 설정을 하고, rndc reload 또는 네임서버 재실행을 하면 된다. rndc trace명령으로 debug 레벨을 변경해보면 debug 레벨별로 어떤 로그가 남기는지 알 수 있다. 경험적으로는 debug 0 (즉, info)또는 debug 1 이 로그량에서 적당하다. 참고로 날짜 형식은 '02-Apr-2009' 또는 'Apr  2'형식으로 남는다.

 	
02-Apr-2009 07:47:44.849 security: info: client 211.111.XXX.XXX#59106: query (cache) './A/IN' denied
02-Apr-2009 07:47:50.453 security: info: client 59.160.XXX.XXX#61038: query (cache) './A/IN' denied
02-Apr-2009 07:47:50.849 security: info: client 211.111.XXX.XXX#59122: query (cache) './A/IN' denied
02-Apr-2009 07:47:56.453 security: info: client 59.160.XXX.XXX#61060: query (cache) './A/IN' denied
02-Apr-2009 07:48:02.233 security: info: client 202.108.XXX.XXX#60838: query (cache) './NS/IN' denied
02-Apr-2009 07:48:02.276 security: info: client 202.108.XXX.XXX#60838: query (cache) './NS/IN' denied
... 생략 ...
02-Apr-2009 07:52:38.968 security: debug 1: client 164.124.XXX.XXX#32904: recursion available: denied
02-Apr-2009 07:52:42.813 security: debug 1: client 222.254.XXX.XXX#17366: recursion available: denied
02-Apr-2009 07:53:43.920 security: debug 3: client 66.249.XXX.XXX#55479: request is not signed
02-Apr-2009 07:53:43.920 security: debug 1: client 66.249.XXX.XXX#55479: recursion available: denied
02-Apr-2009 07:53:43.920 security: debug 3: client 66.249.XXX.XXX#55479: query '도메인명/IN' approved




 최신 소스 다운로드 받기 (2013.05.13 기준 9.9.2 P2)
다운로드 주소 : ftp://ftp.isc.org/isc/bind9/

 



[root@localhost src]# cd /usr/local/src/

[root@localhost src]# wget  ftp://ftp.isc.org/isc/bind9/9.9.2-P2/bind-9.9.2-P2.tar.gz

 

최신버전을 설치 할 것을 권장하며, 가급적 예전 버전의 경우 취약점이 있다고 알려진 것들은 받지 않는다.

  

 

기존 설정 백업 
 

[root@localhost src]# cp /etc/named.conf named.conf-20130513

[root@localhost src]# /usr/sbin/named -v

BIND 9.2.4

[root@localhost src]# cp /usr/sbin/named /usr/sbin/named-924

혹시 모르니.. 기존 zone 파일 위치 백업도 함께 하자.

[root@localhost src]# mkdir /home/bind_20130513

[root@localhost src]# cp -rp /var/named/chroot/var/named /home/bind_b201305013

 

 

컴파일하기 
 

[root@localhost src]# tar zxvf bind-9.9.2-P2.tar.gz

[root@localhost src]# cd bind-9.9.2-P2

[root@localhost bind-9.5.0rc1]# vi  bin/named/include/named/globals.h

 

EXTERN const char *             ns_g_defaultpidfile     INIT(NS_LOCALSTATEDIR

                                                             "/run/named.pid");

EXTERN const char *             lwresd_g_defaultpidfile INIT(NS_LOCALSTATEDIR

                                                            "/run/lwresd.pid");

이부분을 

  

EXTERN const char *             ns_g_defaultpidfile     INIT(NS_LOCALSTATEDIR

                                                             "/named.pid");

EXTERN const char *             lwresd_g_defaultpidfile INIT(NS_LOCALSTATEDIR

                                                            "/lwresd.pid");

 

로 수정

 

[root@localhost bind-9.9.2-P2]# ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/run/named --enable-threads

[root@localhost bind-9.9.2-P2]# make && make install

 

 

컴파일후 바로 데몬을 확인하시길 바랍니다.

가끔 데몬이 죽는경우가 생기기 때문입니다.

 

[root@localhost bind-9.5.0rc1]# ps -ef | grep named

named    13608     1  0 May19 ?        00:00:00 /usr/sbin/named -u named

root     24320 23450  0 18:15 pts/1    00:00:00 grep named

 

재시작 하기전에 위에서 언급 한 보안설정을 적용한다.

[root@localhost bind-9.9.2-P2]# vi /etc/named.conf

 

recursion 취약점 설정
아래 두가지 옵션 중 하나를 사용할 것을 권장.

(메일 서비스 사용하는 서버의 경우 2번 방법 적용 할 것을 권장함)

recursion 기능 비활성화
주의사항
recursion 옵션 no로 변경 시 일반 도메인에 대한 질의는 응답하지 않습니다.
자기 자신이 가지고 있는 Zone 파일 내용들만 응답 하게 됩니다.
일반질의가 되지 않을 때 문제가 되는 부분은 서버의 /etc/resolve.conf 자신의 아이피가 등록 되어 있다면 서버내에서 이름풀이가 되지 않는 문제 발생. 이 경우 외부 DNS 지정을 통해 해결. 예) 168.126.63.1 / 168.126.63.2
 

/etc/name.conf 혹은 /etc/named.caching-nameserver.conf

options {

...

...

        version "UNKNOWN";  // bind 의 버전을 UNKNOWN으로 표시

        recursion no;  // Recursion 질의 기능 OFF

        allow-transfer { 127.0.0.1; }; // Slove 서버가 존재한다면 추가 기입

...

...

};

 

2. recursion 서비스를 신뢰된 호스트로 제한

loopback IP와 211.239.151.211/32 서버 IP에 대해 recursion 이용 가능 하도록 하는 예
acl trust { 127.0.0.1/32; 211.239.151.211/32; };

options {

...

…

version "UNKNOWN";  // bind 의 버전을 UNKNOWN으로 표시

allow-recursion { trust; };

allow-transfer { 127.0.0.1; }; // Slove 서버가 존재한다면 추가 기입

...

... 

};



네임서버 재구동 ( 아래 방법 중 하나만 사용 하시면 됩니다.) 

[root@localhost bind-9.9.2-P2]# service named restart    



[root@localhost bind-9.9.2-P2]# /usr/sbin/named restart

 

버전 확인 

[root@localhost bind-9.9.2-P2]# /usr/sbin/named -v 

 BIND-9.9.2-P2


<https://darksoulstory.tistory.com/62>





<https://m.blog.naver.com/PostView.nhn?blogId=aaaa8229&logNo=220791754432&proxyReferer=https%3A%2F%2Fwww.google.com%2F>


DNS

​FQDN  <---> IP  도메인명과 ip를 서로 바꿔주는 기능.

 

최상위 root 도메인

도메인 맨뒤에 .

www.naver.com. <- root 도메인 (마지막 쩜..)

최상위 도메인이다. 

 

DNS 구성

 

-우리가 보내는 도메인을 DNS 서버에 가장 처음 방문하는 곳이 root 라는 최상위 도메인부터 방문한다.

 

-www.naver.com 에서 naver.com 부분이 Name Server라고 한다.

나머지 www.나 blog. cafe. 등등 과 같은 부분은 Host Name이라고 한다.

 

-IP로 구분하자면 네임서버는 같은 Network ID (대역대)로 구성되있다. 

ex) 192.168.8.2  blog.naver.com

192.168.8.3 mail.naver.com

192.168.8.4 cafe.naver.com

 

 

 

 

﻿DNS서버 구축시 필수적으로 거쳐야할 6가지 설정파일!!

1) /etc/hosts

2) /etc/resolv.conf

3) /etc/named.conf

4) /etc/named.rfc1912.zones

5) /var/named/포워드 존 설정파일

6) /var/named/리버스 존 설정파일

 

 

 

 

 

DNS 순서

 

[그림]

 

Client(PC)

1)ping www.google.com

2)client resolver.cache(client cache)확인

2-1)C:\Windows\System32\Drivers\ETC\Hosts

2-2)ipconfig /displaydns (cache : 이전에 조회했던 기록)

12)이제 www.google.com 접속가능하다.

 

DNS Server

3) www.google.com ? 

4)-zone 영역, -server cache 확인

11)client로 www.google.com (ip주소) 같이 보낸다.

 

DNS server에서 Root로

5)www.google.com?

7)www.google.com?

9)www.google.com

 

Root에서 DNS Server로

6)com의 NS(Name Server)

8)google의 NS     

          10)www.google.com(ip주소도 같이 보내짐)

 

 

1)기본설정파일

vi /etc/hosts

어던 ip주소를 연결시킬지 설정하는 것. hosts 파일

 

파일안에

192.168.8.128(자기ip)        www.naver.com      naver.com

시스템 기본설정임. (데몬안해도됨)

리눅스의 파이어폭스 연다.

주소에 www.naver.com입력

​

아파치문구가 뜨게되어 제대로 접속이 안된다.

 

192.168.8.128              www.mydomain.com           mydomain.co.kr 

다시 요렇게 바꿔놓는다.

이렇게 하면 naver접속가능

 

vi /etc/resolv.conf

domain mydomain.co.kr
search mydomain.co.kr
nameserver 192.168.8.2             <-DNS1    (해당 도메인주소를 DNS1에 먼저 물어봄) 게이트웨이 주소
nameserver 192.168.8.128          <-DNS2    (그다음 순서대로 물어봄) 로컬 DNS서버 주소
nameserver 168.126.63.1            <-DNS3    (DNS서버가 여러대임 순서대로 읽게함) 수퍼 DNS서버 주소
~
~
~
~
-- INSERT --

요렇게 설정.

 

%네트워크 재시작하면 초기화됨.

 

2) 패키지 설치

rpm -qa | grep bind  (bind로 설치되어있는거) 7개

 

*bind-sdb 패키지는 .so의존성이 뜰텐데 postgresql-libs 설치해주면 의존성 사라짐. 무시 ㄴㄴ

대신 libs가 깔려있는데 계속 libs 의존성 뜨면 --nodeps로 깔자.


bind-chroot-9.8.2-0.37.rc1.el6.x86_64
bind-9.8.2-0.37.rc1.el6.x86_64
bind-utils-9.8.2-0.37.rc1.el6_7.2.x86_64
bind-sdb-9.8.2-0.37.rc1.el6.x86_64
bind-devel-9.8.2-0.37.rc1.el6.x86_64
bind-libs-9.8.2-0.37.rc1.el6_7.2.x86_64
bind-dyndb-ldap-2.3-8.el6.x86_64
 

 

vi /etc/named.conf

​가장처음 들어오는 DNS정보

any와 no로 변경

-----------------------​

options {
        listen-on port 53 { any; };                 // 53포트 DNS포트로 {}안의 아이피를 받겠다는 뜻.

        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";                            // DNS 설정파일들이 있는 디렉토리 파일경로 변경가능
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        allow-query     { any; };                  // 도메인네임의 아이피를 묻는 행위 쿼리를 허용하는 아이피를 적는것.

        recursion no;                            // DNS접근 허가여부

        dnssec-enable yes;
        dnssec-validation yes;
        dnssec-lookaside auto;

        /* Path to ISC DLV key */
        bindkeys-file "/etc/named.iscdlv.key";

        managed-keys-directory "/var/named/dynamic";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
-- INSERT --

맨밑에보면​

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
​있는데 C언어처럼 include 하는 것. 이 두개의 파일도 같이 포함하여 보여준다.

​

​

​

vi ​/etc/services에서 53포트가 도메인서버포트임

​

​

 

 

vi /etc/named.rfc1912.zones

DNS서버는 물어보는 것에 대해서 zone 영역을 확인한다고 했음. 관련 파일 잇음

 

# vi /etc/named.rfc1912.zones
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

zone "localhost.localdomain" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "localhost" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "0.in-addr.arpa" IN {
        type master;
        file "named.empty";
        allow-update { none; };
};


zone "linux.edu.kr" IN {                                                          (정방향)
        type master;
        file "linux.edu.kr.zone""; 각 영역별로 파일을 설정해주는 이것을 /var/named 속에 만들어줘야함.
        allow-update { any; };
        allow-transfer { any; };
};

zone "8.168.192.in-addr.arpa" IN {  <-자기아이피 거꾸로 한거                 (역방향)  
        type master;
        file "192.168.8.zone";  <- 요것도 자기 아이피대역  //파일이름은 아무렇게 해도됨.
        allow-update { any; };     //업데이터 허가요청
        allow-transfer { any; };    //파일변경 허가요청
}; 

추가 합시다. 2개

 

 

3) 각 영역별로 파일을 설정해주는 것을 /var/named 속에 만들어 줘야함

cd /var/named

 

zone 파일 두개 만들었으니깐  zone 파일 두개 만들기

 

1번쨰 zone 파일       (정방향)

 

# vi linux.edu.kr.zone
$TTL 86400
@       IN SOA  ns.linux.edu.kr.        root(
                                        77      ; serial
                                        10800   ; refresh
                                        900     ; retry
                                        604800  ; expire
                                        86400 ) ; minimum
        NS      ns.linux.edu.kr.
        A       192.168.8.128

www     A       192.168.8.128
~
~
~
~
~
~

2번쨰 zone 파일                   (역방향)

 

#
# vi 192.168.8.zone
$TTL 1D
@       IN SOA  ns.linux.edu.kr         root(
                                        42      ; serial
                                        3H      ; refresh
                                        15M     ; retry
                                        1W      ; expire
                                        1D )    ; minimum

        IN      NS      ns.linux.edu.kr
        IN      A       192.168.8.128

128     IN      PTR     www.linux.edu.kr
~
~
~
~
~
~
~

 


 

마지막으로 생성한 두개의 파일의 허가권을 660으로 바꾸고, 소유권을 root:named로 바꾸자 데몬재실행 후 nslookup


 

4) 데몬 재실행 -> 오류 잡기
# /etc/rc.d/init.d/named restart 

named 정지 중:                                             [  OK  ]
Generating /etc/rndc.key:                                  [  OK  ]
named 시작 중:
Error in named configuration:
zone localhost.localdomain/IN: loaded serial 0
zone localhost/IN: loaded serial 0
zone 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa/IN: loaded serial 0
zone 1.0.0.127.in-addr.arpa/IN: loaded serial 0
zone 0.in-addr.arpa/IN: loaded serial 0
zone linux.edu.kr/IN: NS 'ns.linux.edu.kr' has no address records (A or AAAA)
zone linux.edu.kr/IN: not loaded due to errors.
_default/linux.edu.kr/IN: bad zone
zone 8.168.192.in-addr.arpa/IN: loading from master file 192.168.8.zone failed: file not found
zone 8.168.192.in-addr.arpa/IN: not loaded due to errors.
_default/8.168.192.in-addr.arpa/IN: file not found
                                                           [실패]
#

자가진단 : #nslookup

>www.linux.edu.kr

>192.168.8.128

 

name = www.linux.edu.kr 나와야 정상적

 

 

 

해답!)

.  과  ns를 => www로

mydomian.co.kr를 => linux.edu.kr로 변경

 

named 데몬은 /named.conf 를 불러와서 확인합니다.  (지금 설정에는 오류가 없다.)

 

 

# vi linux.edu.kr.zone (정방향) 주소 -> ip

​$TTL 86400
@       IN SOA  www.linux.edu.kr.       root(
                                        77      ; serial
                                        10800   ; refresh
                                        900     ; retry
                                        604800  ; expire
                                        86400 ) ; minimum
        IN      NS      www.linux.edu.kr.
        IN      A       192.168.8.128
www     IN      A       192.168.8.128
~
~
~
~
~

 

​#vi 192.168.8.zone (역방향) ip->주소

$TTL 1D
@       IN SOA  www.linux.edu.kr.       root(
                                42      ; serial
                                3H      ; refresh
                                15M     ; retry
                                1W      ; expire
                                1D )    ; minimum
        IN      NS      www.linux.edu.kr.
        IN      A       192.168.8.128
128     IN      PTR     www.linux.edu.kr.
~
~
~
~
~
"192.168.8.zone" 10L, 199C

 

 

# vi /etc/resolv.conf
# Generated by NetworkManager
domain linux.edu.kr
search linux.edu.kr              
//nameserver 192.168.8.2   <- 주석처리해준다.  지금 나는 192.168.8.128 (DNS서버)에 DNS zone을 만들고 설정했기때문에 여기는 게이트웨이라 주석처리해준다.
nameserver 192.168.8.128
nameserver 168.126.63.1
~
~

 

 

 

# /etc/rc.d/init.d/named restart 

named 정지 중:                                             [  OK  ]
Generating /etc/rndc.key:                                  [  OK  ]
named 시작 중:
Error in named configuration:
zone localhost.localdomain/IN: loaded serial 0
zone localhost/IN: loaded serial 0
zone 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa/IN: loaded serial 0
zone 1.0.0.127.in-addr.arpa/IN: loaded serial 0
zone 0.in-addr.arpa/IN: loaded serial 0
zone linux.edu.kr/IN: NS 'ns.linux.edu.kr' has no address records (A or AAAA)    <---A레코드? 주소가 없당.
zone linux.edu.kr/IN: not loaded due to errors.
_default/linux.edu.kr/IN: bad zone
zone 8.168.192.in-addr.arpa/IN: loading from master file 192.168.8.zone failed: file not found   <-- root 로? 192.168.8.zone 파일을 찾을 수 없다?
zone 8.168.192.in-addr.arpa/IN: not loaded due to errors.
_default/8.168.192.in-addr.arpa/IN: file not found
                                                           [실패]
#

빨간색 칠한 zone영역에서 오류인거 보니 우리가 만든 zone 파일 2개를 수정해주면 된다.

 

 

#nslookup

> www.linux.edu.kr 

 

 

---------------------------------------

​

​Zone 파일 내부 설명

# vi linux.edu.kr.zone

$TTL 86400   //데이터의 수면 (86400 = 1Day)
@       IN SOA  www.linux.edu.kr.       root(
                                        77      ; serial          버전같은개념
                                        10800   ; refresh       (3 hours)
                                        900     ; retry            (15 minutes)
                                        604800  ; expire        (1 week)
                                        86400 ) ; minimum     (1 day)
        IN      NS      www.linux.edu.kr.    //네임서버 자체 설정
        IN      A       192.168.8.128        //네임서버 자체 설정
 

www  IN      A       192.168.8.128      //네임서버가 관리하는 각종서버들의 아이피를 관리해준다.
128     IN      A       192.168.8.128   //ex) 네이터카페같은거 cafe     IN   A  192.168.8.129 이런거. 각종 하위서버관리
~
~

 

SOA = www.linux.edu.kr. 이 SOA에 검사받아서 IN <- internet에 올려주는것

NS = NAME 서버

A = 호스트 레코드

AAAA = IPv6용

PTR = A의 역방향 레코드 (A레코드당 하나씩 존재해야함)

 

serial : 해당파일의 몇번째를 수정해줬는지 (파일의 일련번호) 업데이트,패치 버전같은거

(serial을 제외하고 나머지는 초단위)

 

 

 

minimum ??

             0D 0H 0M : NS start

10800 -> 0D 3H 0M : refresh 해당사이트가 살아있는지 확인

 

900    -> 0D 3H 15M : retry 900 이라는 초시간 후에 재확인 신호 

+10800 -> 0D 6H 15M : refresh

+900   -> 0D 6H 30M  : retry

7일     -> 7D(1W)      :expire 이 시간이 지난 후 파기 7일(1주)

사이트주소 바꿔야할떄

yahoo.co.kr를  naver.com 이렇게   

 

절때 파일명을 바꾸면 안됨.

 

패키지를 다시 설치해야함..--

 

bind 패키지

<http://blog.naver.com/PostView.nhn?blogId=cjk1995&logNo=220701243705&parentCategoryNo=&categoryNo=26&viewDate=&isShowPopularPosts=true&from=search>
<https://www.whatap.io/blog/10/>


enp0s31f6


nohup tcpdump -n icmp -i enp0s31f6 -l >> tcpdump_icmp.txt &

nohup tcpdump -n icmp -i enp0s31f6 -l > tcpdump_icmp.txt &

tcpdump & tcpdump_icmp.pcap -n icmp
