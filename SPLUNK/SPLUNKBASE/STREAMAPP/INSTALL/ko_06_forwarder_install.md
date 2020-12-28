Stream Forwarder 용 Splunk 추가 기능 설치
배포 서버를 사용하여 Stream Forwarder 용 Splunk 추가 기능을 모든 포워더에 푸시합니다. 개별 포워더에 Splunk Add-on for Stream Forwarder를 설치할 수도 있습니다.

독립 Stream Forwarder를 설치하려면 독립 Stream Forwarder 설치를 참조하십시오 .

포워더를 7.3 이상으로 업그레이드하려면 분산 배포에서 Splunk Stream 마이그레이션을 참조 하십시오 .

yoru 전달자를 구성하려면 Splunk Stream 전달자 구성을 참조하십시오.

배포 서버를 사용하여 Splunk Add-on for Stream Forwarder를 유니버설 포워더에 배포
http://splunkbase.com/app/5238로 이동합니다 .
다운로드를 클릭 합니다. Splunk_TA_stream_<latest_version>.tgz로컬 호스트에 설치 패키지 다운로드.
Splunk Web에 로그인합니다.
앱 관리 > 파일에서 앱 설치를 클릭 합니다 .
Splunk_TA_stream_<latest_version>.tgz설치 프로그램 파일을 업로드 하십시오.
메시지가 표시되면 Splunk Enterprise를 다시 시작합니다. 이것은 설치 Splunk_TA_stream에서 $SPLUNK_HOME/etc/deployment-apps디렉토리. 이것은 Splunk_TA_stream배포 서버를 사용하여 유니버설 포워더에 배포 할 수 있는 사전 구성된 복사본입니다 .
Splunk_TA_stream권한 설정 : Linux 및 OSX의 set_permissions.sh경우 Splunk_TA_stream디렉토리 에서 스크립트를 실행합니다 .
cd $ SPLUNK_HOME / etc / apps / Splunk_TA_stream
sudo chmod + x ./set_permissions.sh
sudo ./set_permissions.sh
Windows에서는 Splunk Enterprise를 관리자로 실행하거나 WinPcap을 대상 컴퓨터에 독립 실행 형 패키지로 설치합니다. 이 페이지의 Windows 설치 고려 사항을 참조하십시오.
yoru 전달자를 구성하려면 Splunk Stream 전달자 구성을 참조하십시오.

Splunk 포워더에 Stream Forwarder 용 Splunk 애드온을 수동으로 설치
배포 서버를 사용하지 않고 하나 이상의 전달자에서 네트워크 데이터를 수집하려면 Splunk_TA_stream각 전달자에 수동으로 설치 합니다.

http://splunkbase.com/app/5238로 이동 $SPLUNK_HOME/etc/apps하여 Universal Forwarder 에서 최신 설치 패키지를 다운로드하십시오.
패키지 압축 해제 $SPLUNK_HOME/etc/apps
Splunk_TA_stream/local/inputs.conf의 올바른 위치 를 지정 하는지 확인하십시오 splunk_app_stream.
 [streamfwd : // streamfwd]
splunk_stream_app_location = https : // localhost : 8000 / en-us / custom / splunk_app_stream /
stream_forwarder_id = 
비활성화 됨 = 0
Splunk_TA_stream/local/streamfwd.conf네트워크 인터페이스에서 데이터를 수집하도록 구성 되었는지 확인하십시오 . 기본적으로 streamfwd.conf모든 네트워크 인터페이스에서 데이터를 수집합니다.
Splunk_TA_stream권한 설정 : Linux 및 OSX의 set_permissions.sh경우 Splunk_TA_stream디렉토리 에서 스크립트를 실행합니다 .
cd $ SPLUNK_HOME / etc / apps / Splunk_TA_stream
sudo chmod + x ./set_permissions.sh
sudo ./set_permissions.sh
Splunk Enterprise를 다시 시작합니다.

Stream Forwarder 용 Splunk 추가 기능 업그레이드
모든 포워더에서 쉽고 일관된 구현을 위해 배포 서버를 사용합니다. Stream 배포에 배포 서버에없는 추가 포워더가 포함되어 있거나 배포 서버를 사용하지 않는 경우 Splunk_TA_stream각 포워더에서 Splunk Add-on for Stream Forwarder ( )를 수동으로 업그레이드해야합니다 .

검색 헤드 클러스터 및 인덱서 클러스터에 앱 및 추가 기능을 배포하는 방법에 대한 자세한 내용은 Splunk Enterprise 관리자 매뉴얼 의 앱 배포 개요 를 참조하십시오 .

http://splunkbase.com/app/5238 에서 Stream Forwarders 용 Splunk 추가 기능을 다운로드 하십시오 .

기존 버전의 Splunk_TA_stream.
이전 버전에서 Stream Forwarder 용 Splunk 애드온 (Splunk_TA_stream)의 최신 버전을 추출하십시오.
Splunk Enterprise 인스턴스를 다시 시작합니다.
Splunk Stream은 WinPcap 드라이버를 사용하여 Windows 시스템에서 패킷을 캡처합니다. WinPcap 보안 모델의 결함으로 인해 Windows에 Stream을 설치하면 모든 로컬 사용자가 패킷 스니핑에 WinPcap을 사용할 수 있습니다. https://wiki.wireshark.org/CaptureSetup/CapturePrivileges를 참조하십시오. Windows 시스템에서 Splunk Stream은 관리자 역할 만 지원합니다.


스트림 전달자 구성
Splunk Stream Forwarder를 설치 한 후 Spunk Stream 배포로 데이터를 전달하도록 구성합니다.

Stream Forwarder 용 Splunk 추가 기능에 Stream 설치용 Splunk 앱의 위치를 ​​제공합니다 .
데이터 캡처 매개 변수를 지정하도록 로컬 Stream Forwarder를 구성합니다 .
매개 변수 구성 streamfwd.conf
Stream 설치를위한 Splunk 앱의 위치와 함께 Stream Forwarders 용 Splunk 추가 기능 제공
스트림 데이터 캡처를 설정하기 전에 Splunk_TA_stream/local/inputs.confSplunk App for Stream과 통신 하도록 구성 하십시오. 스트림 전달자는이 위치를 사용하여 스트림 구성 UI에서 정의한 프로토콜, 필드 및 집계 유형을 포함한 스트림 캡처 구성을 검색합니다.

를 엽니 다 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/local/inputs.conf.
[streamfwd://streamfwd]스탠자에 올바른 splunk_app_stream설치 위치 (URI)가 포함되어 있는지 확인 하십시오 . 검색 헤드 클러스터의 경우이 주소는 고정 세션이있는로드 밸런서이거나 SHC의 단일 구성원 인 단일 URL 일 수 있습니다.
[streamfwd : // streamfwd]
splunk_stream_app_location = https : // localhost : 8000 / en-us / custom / splunk_app_stream /
비활성화 됨 = 0
자세한 내용 은이 설명서의 Splunk_TA_stream 이 splunk_app_stream과 통신하는 방법을 참조하십시오 .

참고 :splunk_app_stream URI 지원 http및 https프로토콜을 지원합니다. SSL을 사용하는 경우 URI 경로를 변경하여을 지정해야 https합니다. http 포트를 변경하는 경우 URI 경로를 변경하여 새 포트를 지정해야합니다.

스트림 전달자 식별자 구성
배포 서버를 사용할 때 stream_forwarder_id프로세스가 실행되는 동안 Stream 전달자 를 설정하거나 수정하는 경우 변경 사항을 .NET Framework에 적용하려면 유니버설 전달자를 다시 시작해야합니다 stream_forwarder_id.

를 사용하여 stream_forwarder_id분산 스트림 전 달기 인스턴스를 관리 할 수도 있습니다 . 자세한 내용은 분산 전달자 관리를 참조하십시오 .

SSL 인증서 유효성 검사 활성화
SSL 연결에 대한 인증서 유효성 검사를 활성화 Splunk_TA_stream하여 splunk_app_stream서버 의 ID를 확인 합니다. 인증서 유효성 검사를 활성화하려면에서 매개 변수를 편집합니다 inputs.conf.

편집하려면여십시오 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/local/inputs.conf.
다음 매개 변수를 설정하십시오.
sslVerifyServerCert = true: splunk_app_stream클라이언트 ( streamfwd) 측 에서 서버 ( ) 인증서 유효성 검사를 활성화합니다 .
rootCA = <path>: 루트 CA 인증서 파일의 파일 이름을 가리 킵니다. sslVerifyServerCert매개 변수가 true로 설정된 경우 rootCA루트 CA 인증서 파일의 전체 경로를 표시해야합니다. 이 매개 변수가 비어 있거나 존재하지 않는 파일을 가리키는 경우 인증서 유효성 검증이 발생하지 않습니다.
sslCommonNameToCheck = <commonName>: 이렇게하면 인증서 CN과 비교할 공통 이름 값을 재정의 할 수 있습니다. 이 매개 변수를 비워두면 splunk_app_stream서버 의 정규화 된 호스트 이름 이 서버 인증서의 CN에 대해 확인됩니다. 인증서 CN의 경우 일반 이름 형식 *.app.splunk.com또는 streamapp.app.splunk.com지원됩니다. 인증서 유효성 검사가 활성화되고 인증서가 유효하지 않거나 일반 이름이 일치하지 않아 유효성 검사가 실패 streamfwd하면 splunk_app_stream서버에 연결되지 않습니다 .
Splunk Stream 데이터에 대한 인덱서 수신 포트를 구성합니다.
인덱서 탭에서 설정 > 전달 및 수신으로 이동합니다 .
수신 구성을 클릭합니다 .
새로 만들기를 클릭 합니다 .
수신 포트 번호를 입력하십시오. 예 : 포트 9997.
저장을 클릭 합니다.


전달자 매개 변수 구성 streamfwd.conf
streamfwd.conf스트림 전달자에 대한 시스템 수준 매개 변수를 지정하려면 편집 합니다. 다음을 구성 streamfwd.conf할 수 있습니다 .

특정 IP 주소 및 포트에서 수신
SSL 활성화
로그 파일 리디렉션
네트워크 이벤트 수집
네트워크 인터페이스 지정
다음 streamfwd.conf에서 편집 할 수 있습니다 .

Stream Forwarder 용 Splunk 애드온은 다음 위치에 있습니다. $SPLUNK_HOME/etc/apps/Splunk_TA_stream/default/
에 위치한 독립 스트림 포워더 /opt/streamfwd/default/.
Streamfwd.conf 매개 변수
streamfwd.conf 구성 파일은 이러한 매개 변수를 허용합니다.

매개 변수	기술	값 유형	기본값
clientIpSslHashBytes	SSL 프로세서 스레드 해시 알고리즘에 사용할 클라이언트 IP 옥텟 수를 정의합니다. 최소값 = 0; 최대 값 = 4입니다. _disabled_ useGlobalSSLSessionKeyCache가있는 경우에만 적용됩니다.	클라이언트 IP 옥텟	2
dedicatedCaptureMode	호환되는 네트워크 인터페이스에서 10Gbps 캡처를 지원하는 전용 캡처 모드를 활성화합니다. 전용 캡처 모드를 활성화하려면 다음을 추가하십시오 dedicatedCaptureMode = 1.streamfwd.conf	부울	0 (거짓)
duplicatePacketWindow	롤링 창을 사용하여 메모리에 캐시 된 패킷 수를 정의합니다. 네트워크 패킷의 자동 중복 제거를 사용하려면 0보다 큰 값으로 설정하십시오.	메모리에 캐시 된 패킷	0
hideCreditCardNumbers	신용 카드 번호를 마스킹합니다. false모든 신용 카드 번호를 표시 하도록 설정 합니다.	부울	진실
mapSslServers	해당 서버의 IP 주소에 대한 SSL 서버 인증서의 자동 캐싱을 비활성화하려면 False로 설정합니다.	부울	진실
maxEventQueueSize	Splunk로 전달하기 위해 대기중인 최대 이벤트 수를 지정합니다.	이벤트	10000
maxFieldSize	콘텐츠 필드의 최대 크기를 정의합니다.	바이트	10240
maxPacketQueueSize	각 처리 스레드의 패킷 대기열에 대한 최대 크기를 정의합니다. 전용 캡처 모드의 경우 2의 거듭 제곱이어야합니다.	패킷	262144
maxTcpReassemblyPacketCount	처리 스레드 당 리 어셈블리 큐의 최대 TCP 패킷 수를 지정합니다.	TCP 패킷	500000
maxTcpSessionCount	처리 스레드 당 동시 TCP / UDP 흐름의 최대 수를 지정합니다.	TCP / UDP 흐름	50000
pcapBufferSize	각 네트워크 장치의 버퍼 크기를 지정합니다. 손실 된 패킷이 보이면 바이트 수를 늘리십시오.	바이트	33554432
pingInterval	Ping 서버 간격을 수정합니다.	초	5
processingThreads	네트워크 트래픽 처리에 사용할 스레드 수를 지정합니다.	스레드	2
sessionKeyTimeout	SSL 세션 키가 만료되기 전 유휴 시간을 지정합니다.	초	3600
sslServer	SSL 복호화 대상 IP 주소 / 포트를 직접 지정할 수 있습니다.		
streamfwdcapture	데이터 캡처를 지정된 네트워크 인터페이스로 제한합니다.		
tcpConnectionTimeout	TCP / UDP 흐름이 만료되기 전 유휴 시간을 지정합니다.	초	180
tcpServer	TCP 서버에 대한 끝점을 정의합니다.		
useGlobalSSLSessionKeyCache	처리 스레드간에 SSL 캐시를 공유 할 수 있습니다. SSL 캐시를 공유하려면 True로 설정하십시오.	부울	그릇된
usePacketMemoryPool	True로 설정하면 Stream Forwarder는 풀 할당자를 사용하여 네트워크 패킷 저장을위한 메모리를 할당합니다. 풀 할당자는 사용하지 않는 메모리를 운영 체제로 다시 해제하지 않기 때문에이 매개 변수를 true로 설정하면 메모리 사용량이 높아질 수 있습니다. 대용량 트래픽을 처리하는 전용 캡처 서버에서 Stream Forwarder가 실행중인 경우에만 True로 설정하십시오.	부울	그릇된
참고 : 의 전체 목록은 streamfwd.conf매개 변수를 참조하십시오 streamfwd.conf.spec에서 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/README.

일반적인 사용 사례
이러한 예제를 참조하여 streamfwd.conf몇 가지 일반적인 사용 사례를 구성 하는 데 도움을받을 수 있습니다 .

사용하여 tcpServer엔드 포인트를 지정합니다
Stream Forwarder는 TCP 연결의 시작을 캡처 할 때 클라이언트 및 서버 엔드 포인트를 자동으로 감지합니다. TCP 연결을 설정 한 후 트래픽 캡처를 시작하면 Stream Forwarder는 첫 번째 패킷의 발신자가 클라이언트라고 가정합니다.

tcpServer매개 변수를 편집하여 특정 TCP 서버의 엔드 포인트를 정의 하여이 동작을 수정할 수 있습니다 . 패킷의 보낸 사람이 끝점과 일치하면 Stream Forwarder는이를 서버 응답 패킷으로 올바르게 분류합니다.

예 : 다음을 사용하여 단일 HTTP 서버 엔드 포인트 정의 tcpServer
tcpServer.N.address = 192.168.1.102
tcpServer.N.port = 80
예 : 다음을 사용하여 와일드 카드 끝점 정의 tcpServer
tcpServer. <N> .address = 192.168.1.0
tcpServer. <N> .addressWildCard = 255.255.255.0
tcpServer. <N> .port = 80
sslServer매개 변수를 사용하여 암호화 / 해독 된 트래픽을 지정합니다.
Stream Forwarder는 엔드 포인트 암호화를 감지하고 사용 가능한 개인 키를 사용하여 SSL 세션 암호 해독을 시도합니다. 선택적으로 sslServer매개 변수 를 추가하여 트래픽을 암호화 된 것으로 정의 할 수 있습니다 .

sslServer. <N> .address = 192.168.1.102
sslServer. <N> .port = 443
streamfwdcapture네트워크 인터페이스를 지정하는 데 사용
기본적으로 streamfwd.conf사용 가능한 모든 네트워크 인터페이스에서 트래픽을 수신합니다. streamfwdcapture매개 변수를 사용하여 데이터 캡처를 특정 인터페이스로 제한 하십시오 .

이 streamfwdcapture매개 변수는 다음 옵션을 지원합니다.

매개 변수	기술
streamfwdcapture.<N>.interface	네트워크 인터페이스 이름 또는 PCAP 파일 경로 지정
streamfwdcapture.<N>.interfaceRegex	여러 네트워크 인터페이스와 일치하는 정규식 지정
streamfwdcapture.<N>.offline	PCAP를 사용하려면 True로 설정하십시오. <Interface>가 네트워크 장치 이름임을 나타내려면 False로 설정합니다. 기본값은 False입니다.
streamfwdcapture.<N>.filter	커널 수준 패킷 필터링을위한 BPF ( Berkeley Packet Filter )를 설정할 수 있습니다 . 이 태그의 값은 BPF 구문을 준수해야 합니다 . streamfwdcapture매개 변수 당 하나의 필터 변수 만 지원됩니다.
streamfwdcapture.<N>.repeat	연속로드를 위해 PCAP 파일을 반복적으로 재생하려면 True로 설정합니다.
streamfwdcapture.<N>.sysTime	PCAP 파일의 실제 타임 스탬프 대신 패킷 타임 스탬프에 시스템 시간을 사용하려면 True로 설정합니다.
streamfwdcapture.<N>.bitsPerSecond	정의되지 않고 <반복>이 참인 경우 기본적으로 10Mbps로 설정되는 속도 제한 기입니다.
데이터 캡처를 특정 네트워크 인터페이스로 제한하려면 [streamfwd]스탠자를 streamfwd.conf. streamfwdcatpure매개 변수를 사용하여 단일 streamfwd.conf파일 에 여러 네트워크 인터페이스를 지정할 수 있습니다 . 예를 들어 * nix에서 서로 다른 BPF 필터로 구성된 두 개의 네트워크 인터페이스 (eth0 및 eth1)를 지정하려면 다음을 수행하십시오.

[streamfwd]
streamfwdcapture.0.interface = eth0
streamfwdcapture.0.filter = tcp 포트 80
streamfwdcapture.1.interface = eth1
streamfwdcapture.1.filter = udp 포트 53

Windows에서 네트워크 인터페이스 지정
이 예는 Windows 네트워크 인터페이스를 지정합니다.

streamfwdcapture.0.interface = \ Device \ NPF_ {D6995D00-B75C-48DB-99AA-69F0150126BC}
streamfwdcapture.0.offline = false
streamfwdcapture.0.filter = tcp 포트 80
Windows에서는 대체 할 수 streamfwdcapture.<N>.interface또는 streamfwdcapture.<N>.InterfaceRegex이름을 (예 : \Device\NPF_{D6995D00-B75C-48DB-99AA-69F0150126BC}와 함께) <Alias>또는 <Description>에 의해 반환 된 값 --iflist명령 줄 옵션을 선택합니다.

예를 들어, streamfwdcapture.<N>.interface = Local Area Connection 2또는 streamfwdcapture.<N>.InterfaceRegex = Local Area.*.

자세한 내용 은이 설명서의 " Windows 및 Linux에서 네트워크 인터페이스 나열 "을 참조하십시오 .

streamfwdcapture 예
예 : streamfwd.conf로컬 루프백 캡처를 포함하도록 구성
기본적으로 Stream Forwarder는 동일한 시스템에서 시작 및 종료되는 트래픽을 캡처하지 않습니다. streamfwdcapture구성 파일 의 매개 변수를 사용하여이 "로컬 루프백"트래픽 캡처를 활성화 할 수 있습니다 .

   streamfwdcapture. <N> .interface = lo0
참고 : streamfwdcapture.<N>.interfaceRegex>매개 변수를 사용하여 로컬 루프백 인터페이스를 지정할 수 없습니다 .

예 : streamfwd.conf여러 시스템에서 사용하도록 구성
모범 사례로, streamfwd.conf네트워크 장치 이름이 다른 여러 시스템에서 재사용 할 수 있는 마스터 사본을 유지하십시오 . 다음 streamfwd.conf구성은 일치하는 모든 인터페이스를 수신합니다. 이 구성은 로컬 루프백 인터페이스를 캡처하지 않습니다.

streamfwdcapture. <N> .interfaceRegex =. *
이 구성은 수동 데이터 캡처를 지원하지 않는 모든 장치에 대해 시작 경고를 생성 할 수 있습니다.

예 : 특정 네트워크 인터페이스에서 데이터 캡처
이 예에서 8 개의 네트워크 인터페이스가있는 시스템 streamfwd.conf에서 해당 인터페이스 중 2 개 (4 개 및 5 개)에서만 tcp 포트 80 트래픽 만 수신합니다.

streamfwdcapture. <N> .interfaceRegex = eth [45]
streamfwdcapture. <N> .offline = false
streamfwdcapture. <N> .filter = tcp 포트 80
예 : 네트워크 인터페이스 대신 PCAP 파일 사용
네트워크 인터페이스 대신 이전에 생성 된 PCAP 파일을 사용합니다.

streamfwdcapture. <N> .interface = /tmp/data.cap
streamfwdcapture. <N> .offline = true
streamfwdcapture. <N> .filter = tcp 포트 80
streamfwdcapture. <N> .repeat = true
streamfwdcapture. <N> .sysTime = true
streamfwdcapture. <N> .bitsPerSecond = 10000000
streamfwdcapture매개 변수를 사용하여 PCAP 파일을 수집하는 방법에 대한 자세한 내용 은이 설명서의 streamfwd.conf를 사용하여 pcaps 수집을 참조하십시오 .

예 : 구성 파일에 streamfwdcapture매개 변수 추가streamfwd.conf
스탠자에 하나 이상의 streamfwdcapture매개 변수를 추가 [streamfwd]하여 특정 네트워크 인터페이스에 대한 캡처 동작을 정의 할 수 있습니다.

[streamfwd]
streamfwdcapture.0.interfaceRegex = eth [45]
streamfwdcapture.0.offline = false
streamfwdcapture.0.filter = tcp 포트 80
streamfwdcapture.1.interface = eth0
streamfwdcapture.1.offline = false
streamfwdcapture.1.filter = udp 포트 53

독립 스트림 전달자 설치
ISF (Independent Stream Forwarder)는 독립형 Stream Forwarder입니다. ISF는 유선 데이터를 수집하기 위해 Splunk Universal Forwarder가 필요하지 않습니다. 대신 ISF는 HTTP 이벤트 수집기를 사용하여 캡처 된 네트워크 데이터를 Splunk로 보냅니다.

Independent Stream Forwarder는 Splunk Universal Forwarder를 설치할 수없는 네트워크 및 배포에 유용합니다.

Splunk Stream은 Splunk Cloud 또는 Splunk Enterprise 구성으로 데이터를 전송할 수있는 호환되는 Linux 시스템에 Independent Stream Forwarder를 설치할 수있는 바이너리 코드를 제공합니다.

예를 들어 Splunk IT 서비스 인텔리전스 (ITSI) 배포에서 네트워크 서비스의 일부로 모니터링중인 Linux 호스트에서 네트워크 데이터를 캡처하려는 경우 독립적 인 Stream 전달자 배포를 사용할 수 있습니다.

전제 조건
64 비트 Linux 전용.
기존 Splunk Stream 6.5.0 이상 배포.
독립적 인 Stream 전달자로부터 데이터를 수신하려면 인덱서에서 HTTP 이벤트 수집기 (HEC)를 구성해야합니다.
Independent Stream Forwarder에는 Universal Forwarder가 필요하지 않습니다.

Splunk Enterprise 또는 Splunk Cloud 구성에 Splunk App for Stream을 설치하고 구성해야합니다.
curl을 사용하여 독립적 인 Stream Forwarder 설치
Splunk App for Stream ( splunk_app_stream) curl은 포워더를 설치하기 위해 명령 줄에서 실행할 수 있는 스크립트를 생성합니다 .

Splunk App for Stream 기본 메뉴에서 구성 > 분산 전달자 관리를 클릭합니다 .
Stream Forwarder 설치를 클릭 합니다 . Stream Forwarder 설치 창이 나타납니다.
curl 스크립트를 복사하십시오.
Independent Stream Forwarder를 설치하려는 Linux 시스템에 SSH를 사용하십시오.
에서 복사 한 curl 스크립트를 실행하십시오 splunk_app_stream. 예를 들면 :
curl -sSL http : // stream-cont-func02 : 8000 / en-us / custom / splunk_app_stream / install_streamfwd | sudo bash
streamfwd바이너리 를 다운로드, 설치 및 시작하라는 각 프롬프트에서 예 또는 아니오로 응답하십시오 .

필요에 따라 프롬프트없이 완전 자동화 된 모드에서 curl 스크립트를 실행할 수 있습니다.

다음 매개 변수를 추가하여 5 단계에 표시된대로 curl 스크립트를 실행하십시오 -s -- --accept-defaults.. 예를 들면 :
curl -sSL http : // stream-cont-func02 : 8000 / en-us / custom / splunk_app_stream / install_streamfwd | sudo bash -s---accept-defaults
streamfwd서비스를 시작하십시오 .
sudo 서비스 streamfwd 시작
에서 splunk_stream_app_location주소가 올바르게 설정되었는지 확인하십시오 /opt/streamfwd/local/inputs.conf.
SSL 인증서 유효성 검사 활성화
SSL 연결에 대한 인증서 유효성 검사를 활성화 streamfwd하여 splunk_app_stream서버 의 ID를 확인 합니다.

HTTP 이벤트 콜렉터가 스트림 전달자로부터 데이터를 수신하도록 설정
독립적 인 Stream Forwarder에서 데이터를 수신하려면 Splunk 인덱서에서 HTTP 이벤트 수집기 (HEC)를 활성화해야합니다. 독립 스트림 전달자에 대한 HEC 구성을 관리하는 방법에는 두 가지가 있습니다.

splunk_app_stream검색 헤드에서에 의해 생성 된 기본 HEC 구성을 사용합니다 .
streamfwd.conf로컬 스트림 전달자 인스턴스에서 수동으로 구성 합니다.
Splunk Web에서 기본 HEC 구성 활성화
Splunk Stream을 설치하면 기본 HEC 구성이 자동으로 생성됩니다. 독립 스트림 전달자는 splunk_app_streamREST API를 통해이 기본 구성을 수신합니다 . 구성에 HEC가 활성화되어 있는지 확인하십시오.

Splunk App for Stream 사용자 인터페이스에서 Configuration > Distributed Forwarder Management를 클릭 합니다.
스트림 전달자 설치를 클릭 합니다 .
HTTP 이벤트 콜렉터 streamfwd 토큰 구성이 사용 불가능한 경우 구성보기를 클릭하십시오 . HTTP 이벤트 콜렉터 페이지가 열립니다.
전역 설정을 클릭 합니다 .
전역 설정 편집 모달에서 사용을 클릭 합니다 . 이렇게하면 HTTP 이벤트 수집기가 활성화됩니다.
저장을 클릭합니다 .
활성화 streamfwd HTTP 이벤트 수집기 ' "입력합니다.
HEC 구성 streamfwd.conf
streamfwd.confIndependent Stream Forwarder에서 수동으로 구성 하여 HEC 토큰 값 및 인덱서 URI를 지정할 수 있습니다 .

데이터를 수집하려는 인덱서에서 HEC 토큰을 수동으로 생성합니다.
Independent Stream Forwarder 인스턴스에서 /opt/streamfwd/local/streamfwd.conf
에서 [streamfwd]스탠자의 HEC 토큰 값과 인덱서 URI를 지정합니다. 기본적으로 HEC는 TCP 포트 8088에서 http를 통해 데이터를 수신합니다. URI에이를 지정해야합니다.
[streamfwd]
httpEventCollectorToken = 6fe91580-2156-4644-8416-8b8d22b197ab
indexer. <N> .uri = http://idx-01.sv.splunk.com:8088
HEC 토큰을 생성에 대한 자세한 내용은 참조 를 사용하여 HTTP 이벤트 수집기 에서 데이터에서 가져 오기 .

HTTP Event Collector는 Independent Stream Forwarder에 대해서만 지원됩니다.

HTTP Event Collector 구성을 인덱서 클러스터로 전파
HTTP 이벤트 수집기가 활성화되어 있어야하며 [httpː//streamfwd]Stream 전달자가 이벤트를 보내는 모든 인덱서의 입력 및 SSL 구성에 대해 동일한 구성이 있어야 합니다.

주 splunk_app_stream에만 생성 streamfwd이 실행중인 인스턴스에 대한 HTTP 이벤트 콜렉터 입력합니다. 인덱서 클러스터로 데이터를 보내려면 구성된 인스턴스 의 [httpː//streamfwd]스탠자를 모든 인덱서 splunk_httpinput/local/inputs.conf의 해당 splunk_httpinput/local/inputs.conf파일에 복사하십시오 .

[http : // streamfwd]
비활성화 됨 = 0
토큰 = 521F51A6-093C-4954-80F9-47A5445DFBDD
독립 스트림 포워더 업그레이드
현재 버전의 독립 스트림 포워더를 실행하는 Linux 시스템에 로그인합니다.
기존 Independent Stream Forwarder 구성을 백업하십시오.
streamfwd서비스를 중지하십시오 .
cd / opt / streamfwd / bin
sudo 서비스 streamfwd 중지
curl 스크립트를 실행하십시오.
curl -sSL http : // stream-cont-func02 : 8000 / en-us / custom / splunk_app_stream / install_streamfwd | sudo bash
[yes]프롬프트에서 입력 하여 기존 설치를 덮어 씁니다.
이 스크립트는 기존 버전의 Independent Stream Forwarder를 업데이트 된 버전으로 덮어 쓰고 마이그레이션에서 모든 기존 Stream Forwarder 구성을 유지합니다.
서비스 [yes]를 시작하려면 프롬프트에 입력하십시오 streamfwd.


Independent Stream Forwarder에 대한 명령 줄 옵션
Independent Stream Forwarder에는 PCAP 파일 데이터를 읽고, PCAP 파일 데이터를 인덱서로 보내고, 네트워크 인터페이스를 식별하고, SSL 키를 관리하고, 기타 구성 작업을 수행 할 수있는 명령 줄 옵션이 포함되어 있습니다.

streamfwd바이너리에 위치 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/<OS_arch>/bin하거나 독립 위해 streamfwd배포 /opt/streamfwd/bin.

모든 streamfwd명령 줄 옵션 을 보려면 옵션을 지정하십시오 -h. 예를 들면 :


[root @ myserver bin] # ./streamfwd -h

용법:
  streamfwd [-r FILE1] ... [--pcapdir DIR1] ... [pcap_options] [options] [output_option]
      라이브 또는 저장된 네트워크 트래픽을 처리하고 결과 이벤트를 전달합니다.

  streamfwd 명령
      명령을 실행하고 종료하십시오.

옵션 :
  -D 데몬 (또는 Windows 서비스)으로 실행합니다.
  -v Verbose. (streamfwdlog.conf도 참조하십시오.)
  --PARAM VALUE streamfwd.conf 또는 inputs.conf에서 PARAM = VALUE 설정과 동일합니다.
                               비 중첩 매개 변수에만 적용됩니다.
                               예 :
                                 --index my_index
                                 --processingThreads 6
                               하지만:
                                 --streamfwdcapture.2.repeat false
  -r FILE pcap 파일에서 네트워크 트래픽을 읽습니다.
  --pcapdir DIR 디렉토리의 pcap 파일에서 네트워크 트래픽을 읽습니다.

상대 파일 또는 디렉토리는 cwd에 상대적입니다.
PCAP 파일 또는 디렉터리를 지정하지 않으면 라이브 네트워크 트래픽이 캡처됩니다.

pcap_options :
  -b BITS_PER_SECOND 비트 전송률을 제한합니다 (대략). 모든 pcap 파일에 적용됩니다.
                               기본값 : 100Mbps (10Mbps, --repeat 사용)
  --systime pcap 파일의 시간 대신 시스템 시간을 사용합니다. 모든 pcap 파일에 적용됩니다.
  --repeat pcap 파일을 영원히 반복합니다. -r로 지정된 모든 파일에만 적용됩니다.
  --afteringest ACTION 디렉토리에서 pcap 파일을 수집 한 후 수행 할 작업입니다.
                               --pcapdir로 지정된 모든 디렉토리에만 적용됩니다.
                               기본값 : --afteringest 이동

작업 (--afteringest 용) :
  delete 파일을 삭제합니다.
  move [SUBDIR] 파일을 하위 디렉터리로 이동합니다. [기본값 : finished_pcaps]
                               필요한 경우 디렉토리가 생성됩니다.
  무시 파일을 그대로두고 이미 처리 된 것으로 표시합니다.
  반복 모든 pcap 파일을 계속해서 다시 처리합니다.
  stop 파일을 그대로 둡니다. 각 디렉토리를 한 번 처리 한 후 모니터링을 중지하십시오.

output_options :
  --modinput 모듈 식 입력으로 실행합니다. stdout으로 출력합니다. (또한 stdin에서 입력합니다.)
  -s SERVER streamfwd의 다른 인스턴스로 출력을 보냅니다. (서버 = [https : //] HOST [: PORT])

출력 동작은 <code> streamfwd </ code> 및 <code> output_option </. code> (지정된 경우)을 포함하는 디렉토리 구조에 의해 결정됩니다.
우선 순위에 따른 규칙 :
1) 디렉토리가 독립 에이전트처럼 보이면 HTTP를 통해 splunk로 출력이 전송됩니다.
2) <code> output_option = --modinput </ code> 인 경우 <code> streamfwd </ code>는 모듈 식 입력으로 실행됩니다.
3) 디렉토리가 TA처럼 보이고 명령 줄 인수가없는 경우 <code> streamfwd </ code>는 모듈 식 입력으로 실행됩니다.
4) <code> output_option = -s SERVER </ code>이면 출력이 SERVER로 전송됩니다.
5) 그렇지 않으면 출력이 <code> localhost : 8889 </ code>로 전송됩니다.

명령 :
  -h, --help이 메시지를 표시하고 종료합니다.
  --version 버전을 표시하고 빌드하고 종료합니다.
  --scheme 모듈 식 입력 체계를 표시하고 종료합니다.
  --validate-arguments 모듈 식 입력 XML 구성의 유효성을 검사하고 종료합니다.
  --iflist 네트워크 인터페이스를 나열하고 종료합니다.
  --sslkeylist SSL 키를 나열합니다.
  --addsslkey KEY_NAME PEM_FILE [비밀번호]
                               지정된 SSL 키를 추가합니다.
  --deletesslkey KEY_NAME 지정된 SSL 키를 삭제합니다.
  -c [TEMPLATE_NAME] 지정된 제품 템플릿을 활성화합니다.
  -c 활성 제품 템플릿을 비활성화합니다.
  --listtemplates 설치된 제품 템플릿을 나열합니다.
streamfwd명령 줄 옵션은 streamfwd.conf기본적으로 모든 네트워크 장치에서 데이터를 캡처 하는 구성 파일 을 재정의합니다 . streamfwd명령 줄 옵션은 또한의 streamfwdcapture매개 변수로 지정된 특정 캡처 위치를 재정의합니다 streamfwd.conf.

참고 :streamfwd 명령 을 실행하기 위해 루트 권한이 필요하지 않습니다 .

streamfwd 출력 동작 정보
streamfwd명령 의 출력 동작은 streamfwd바이너리를 독립적 인 배포로 실행하는지 또는의 일부로 실행하는지에 따라 다릅니다 Splunk_TA_stream.

Independent Stream Forwarder streamfwd배포를 사용하는 경우 출력은 HTTP 이벤트 수집기에 의해 인덱서로 전송됩니다. 이 설명서의 독립 스트림 전달자 배포를 참조 하십시오 .
를 사용하는 경우 Splunk_TA_stream출력은 localhost:8889Splunk TA for Stream Wire Data로 전송되어 수신 된 이벤트와 자체 생성 이벤트를 전달합니다. Stream Wire Data 용 Splunk TA가 실행 중인지 확인하려면 :
설정 > 데이터 입력 > 와이어 데이터를 클릭 합니다 .
모듈 식 입력 상태가 비활성화로 표시 되면 활성화를 클릭 합니다.
찾기 streamfwd.conf
streamfwdstreamfwd.conf다음 디렉토리에서 찾습니다 .

대상 Splunk_TA_stream:
$ SPLUNK_HOME / etc / apps / Splunk_TA_stream / default
$ SPLUNK_HOME / etc / apps / Splunk_TA_stream / local
대한 streamfwd독립 배포 :
$ STREAMFWD_PATH / 기본값
$ STREAMFWD_PATH / 로컬
기본적으로 $STREAMFWD_PATH는 어디에 /opt/streamfwd있습니다.

default및 local디렉터리 의 올바른 사용에 대한 정보 는 Splunk Enterprise 관리자 매뉴얼 의 구성 파일 정보를 참조하십시오 .

예
네트워크 인터페이스 나열
이 --iflist옵션을 사용하여 Windows 또는 Linux 시스템에서 모든 네트워크 인터페이스를 볼 수 있습니다.

예를 들어, Windows 시스템의 경우 :

C : \ Splunk_Home \ etc \ apps \ Splunk_TA_stream \ windows_x86_64 \ bin> streamfwd.exe --iflist
<스니퍼>
  <인터페이스>
    <이름> \ Device \ NPF_ {D6995D00-B75C-48DB-99AA-69F0150126BC} </ 이름>
    <Alias> 로컬 영역 연결 </ Alias>
    <Description> Intel (R) PRO / 1000 MT 네트워크 연결 </ Description>
  </ 인터페이스>
</ 스니퍼>
PCAP 파일 읽기
-r <PCAP_FILE>옵션을 사용하여 PCAP 파일의 내용을 읽습니다.

예를 들면 :

[root @ myserver bin] ./streamfwd -r my.pcap
16 : 35 : 30.094 INFO stream.CaptureServer-Found DataDirectory : / opt / splunk / etc / apps / Splunk_TA_stream / data
16 : 35 : 30.113 INFO stream.CaptureServer-Found UIDirectory : / opt / splunk / etc / apps / Splunk_TA_stream / ui
16 : 35 : 30.917 정보 stream.StreamSender-성공적으로 ping 된 서버 : d35f1088-eec6-4a9e-baf3-de0b0ad6870c
16 : 35 : 30.917 INFO stream.CaptureServer-기본 구성 디렉토리 : / opt / splunk / etc / apps / Splunk_TA_stream / default
16 : 35 : 30.921 INFO stream.CaptureServer-pcap 데이터 보내기 시작
16 : 35 : 31.005 정보 stream.StreamSender-성공적으로 ping 된 서버 : d35f1088-eec9-4a9e-baf1-de0b0ad6021c
16 : 35 : 31.248 INFO stream.CaptureServer-pcap 파일 /root/network_data.pcap로 오프라인 캡처 구성
16 : 35 : 31.266 INFO stream.CaptureServer-데이터 캡처 시작
16 : 35 : 31.267 INFO stream.SnifferReactor-네트워크 캡처 시작 : 스니퍼
16 : 35 : 31.318 정보 stream.main-streamfwd가 성공적으로 시작되었습니다 (버전 7.0.0 빌드 99).
16 : 35 : 31.331 INFO stream.SnifferReactor-pcap 파일 읽기 완료 : /root/network_data.pcap
이 -r옵션을 여러 번 사용하여 병렬로 읽을 여러 PCAP 파일을 지정할 수 있습니다 . -r당신의 인수 중 하나가 유효한 PCAP 파일 이름 인 경우 옵션을 암시한다. 다음은 위 예제의 명령과 기능적으로 동일합니다.

[root @ myserver bin] ./streamfwd my.pcap
-s옵션 없이 PCAP 파일을 제공하는 경우 streamfwd"-s localhost : 8889"로 가정합니다. 이 두 예 모두 서버에서 실행되는 Stream Wire Data 용 Splunk TA로 PCAP 파일에 포함 된 데이터를 보냅니다.

참고 : Stream은 .pcapngWindows에서 파일 형식을 지원하지 않습니다 . .pcapngWindows에서 파일 을 사용하려면 먼저 .pcap파일 형식으로 변환해야 합니다.

자세한 내용 은이 설명서의 Ingest pcap 파일 을 참조하십시오 .

PCAP 파일 반복
이 --repeat옵션을 사용하면 streamfwdPCAP 파일이 종료 될 때까지 계속 반복됩니다.

예를 들어, 각각 1Mbps의 속도 (총 2Mbps)로 두 개의 PCAP 파일을 연속적으로 반복하려면 :

./streamfwd -r my.pcap -r your.pcap -b 1048576-반복
streamfwd버전 받기
--version옵션을 사용하여 현재 streamfwd버전 을 가져옵니다 .

예를 들면 :

[root @ myserver bin] ./streamfwd --version
streamfwd 버전 7.0.0 빌드 99
모듈 식 입력 체계 얻기
--scheme옵션을 사용하여 모듈 식 입력 체계를 인쇄합니다.

예를 들면 :

[root @ myserver bin] ./streamfwd --scheme
<scheme> <title> 유선 데이터 </ title> <description> 네트워크 트래픽에서 유선 데이터를 수동으로 캡처합니다. </ description>
<use_external_validation> true </ use_external_validation> <use_single_instance> true </ use_single_instance>
<streaming_mode> xml </ streaming_mode> <endpoint> <args> <arg name = "splunk_stream_app_location"> <title> Splunk App for Stream 
위치 </ title> <description> URI (splunk_app_stream 설치의 전체 경로 포함) (예 : http : // localhost : 8000 / en-us / custom / splunk_app_stream /)
</ description> <validation> validate (match ( 'splunk_stream_app_location', '^ https? : //.+'), '위치는 http : // 또는 https : //'로 시작해야 함) </ validation> </ arg
<arg name = "stream_forwarder_id"> <title> 스트림 전달자 식별자 </ title> <description> 스트림 전달자의 문자열 식별자 </ description>
</ arg> <arg name = "sslVerifyServerCert"> <title> 서버 인증서 확인 </ title> <description> true 인 경우 스트림 전달자는 서버가 
연결에 유효한 SSL 인증서가 있는지 확인합니다. 기본값은 false입니다. </ description> <validation> validate (is_bool ( 'sslVerifyServerCert'), 'Verify Server
인증서는 참 또는 거짓이어야합니다. ') </ validation> </ arg> <arg name = "rootCA"> <title> 루트 CA 파일 </ title> <description> 루트 인증 기관 파일의 경로입니다. 이 값은 서버 인증서 확인이 true로 설정된 경우에만 사용됩니다. </ description> </ arg> <arg
name = "sslCommonNameToCheck"> <title> 서버 인증서의 공통 이름 </ title> <description> 기본적으로 Stream Forwarder는 호스트 이름을 사용하여 
서버 인증서 일반 이름과 일치합니다. 해당 동작을 변경하려면이 값을 재정의하십시오. 이 값은 서버 인증서 확인이 다음으로 설정된 경우에만 사용됩니다.
참. </ description> </ arg> </ args> </ endpoint> </ scheme>


streamfwd.conf
다음은 streamfwd.conf의 스펙 파일입니다.

streamfwd.conf.spec

[streamfwd]
* 이것은 현재이 스펙 파일에 대해 유일하게 지원되는 스탠자입니다.
* 모든 streamfwd.conf 설정은이 단일 스탠자 아래에 통합됩니다.

clientIpSslHashBytes = <정수>
* SSL 프로세서 스레드 해시 알고리즘에 사용할 클라이언트 IP 옥텟 수를 정의합니다. (최소값 = 0, 최대 값 = 4)
* _disabled_ useGlobalSSLSessionKeyCache가있는 경우에만 적용됩니다.

duplicatePacketWindow = <정수>
* 중복 패킷을 감지하기 위해 메모리에 캐시 된 패킷 수 (롤링 창 사용)를 정의합니다.
* 네트워크 패킷의 자동 중복 제거를 사용하려면 0보다 큰 값으로 설정하십시오.

hideCreditCardNumbers = <부울>
* 신용 카드 번호를가립니다. 모든 신용 카드 번호를 표시하려면 false로 설정하십시오.

mapSslServers = <부울>
* 해당 서버의 IP 주소에 대한 SSL 서버 인증서의 자동 캐싱을 비활성화하려면 false로 설정합니다.

maxEventQueueSize = <정수>
* Splunk로 전달하기 위해 대기중인 최대 이벤트 수를 정의합니다.

maxFieldSize = <바이트>
* 콘텐츠 필드의 최대 크기를 정의합니다.

maxPacketQueueSize = <정수>
* 각 처리 스레드의 패킷 대기열에 대한 최대 크기를 정의합니다.

maxTcpReassemblyPacketCount = <정수>
* 처리 스레드 당 리 어셈블리 큐의 최대 TCP 패킷 수를 정의합니다.

maxTcpSessionCount = <정수>
* 처리 스레드 당 동시 TCP / UDP 흐름의 최대 수를 정의합니다.

maxFlows = <정수>
* 진행 스레드 당 최대 동시 흐름 수를 정의합니다.

pcapBufferSize = <바이트>
* 각 네트워크 장치의 버퍼 크기를 정의합니다. 손실 된 패킷이 보이면 바이트 수를 늘리십시오.

pingInterval = <초>
* 핑 서버 간격을 수정합니다.

processingThreads = <정수>
* 네트워크 트래픽 처리에 사용할 스레드 수를 정의합니다.

sessionKeyTimeout = <초>
* SSL 세션 키가 만료되기 전 유휴 시간을 나타냅니다.

tcpConnectionTimeout = <초>
* TCP / UDP 흐름이 만료되기 전 유휴 시간을 나타냅니다.

tcpFlowTimeout = <초>
* TCP 흐름이 만료되기 전 유휴 시간을 나타냅니다.

udpFlowTimeout = <초>
* UDP 흐름이 만료되기 전 유휴 시간을 나타냅니다.

arpFlowTimeout = <초>
* ARP 흐름이 만료되기 전 유휴 시간을 나타냅니다.

ipFlowTimeout = <초>
* IP 흐름이 만료되기 전 유휴 시간을 나타냅니다.

useGlobalSSLSessionKeyCache = <부울>
* 처리 스레드간에 SSL 캐시를 공유 할 수 있습니다. 공유하려면 true로 설정하십시오.

usePacketMemoryPool = <부울>
* true로 설정하면 Stream Forwarder는 풀 할당자를 사용하여 네트워크 패킷 저장을위한 메모리를 할당합니다.
* 풀 할당자는 사용하지 않는 메모리를 운영 체제로 다시 해제하지 않기 때문에
매개 변수를 true로 설정하면 메모리 사용량이 높아질 수 있습니다.
* 스트림 포워더가 대용량을 처리하는 전용 캡처 서버에서 실행중인 경우에만 true로 설정
트래픽 볼륨.

configTemplateName = <값>
* 사용할 제품 템플릿을 나타냅니다. <value>는 설치된 제품 템플릿의 유효한 이름입니다 (예 : es, itsi).

indexer. <N> .uri = <값>
* 독립 에이전트 모드에서 Stream Forwarder를 실행할 때이 매개 변수를 사용하여 
Stream Forwarder 생성 이벤트를 수신하려는 Splunk 인덱서.
* <value>는 Splunk 인덱서를 가리키는 유효한 URI입니다.

analyzeRawSSL = <값>

creditCardNumbersMask = <값>

creditCardNumbersRegex = <값>

httpEventCollectorToken = <값>

ipAddr = <값>

logConfig = <값>

maxEventAttributes = <값>

packetBatchSize = <값>

포트 = <값>

sslKey = <값>

fileServerMountPoint = <값>
* 대상 패킷 캡처를 위해 패킷을 PCAP 파일에 저장하는 파일 서버의 마운트 지점. 이 설정은 네트워크를 통해 전송되는 동안 추출 된 파일을 저장하는데도 사용됩니다.
*이 값을 비워두면 파일이 저장되지 않습니다.

fileServerId = <값>
* 파일 서버 ID는 파일 서버를 고유하게 식별하는 데 사용됩니다.
*이 값은 검색 헤드 구성에 설정된 파일 서버 ID와 일치해야합니다. 
*이 값이 검색 헤드 구성에서 설정 한 값과 일치하지 않을 경우 사용자는 이벤트 흐름 동작을 사용하여 파일을 다운로드 할 수 없습니다. 

maxFlowPacketsToCache = <정수>
* 대상 패킷 캡처를 위해 캐시하고 PCAP 파일에 저장할 흐름 당 최대 패킷 수. 
* 패킷 캐시가이 제한에 도달하면이 캐시 제한을 유지하기 위해 이전 패킷이 삭제됩니다. 이 설정은 대상 패킷 캡처를위한 메모리 사용량을 관리하는 데 도움이됩니다.
*이 설정의 값이 높을수록 더 많은 메모리 사용량이 발생합니다.
* 기본값은 50입니다.

packetSenderQueueSize = <정수>
* 파일 서버에 저장할 파일 수를 정의합니다. 이 대기열이 가득 차면 새 파일을 추가 할 수있는 공간이 생길 때까지 파일 오버플로가 삭제됩니다.
* 파일 서버에 대한 처리량이 느리면이 대기열이 빠르게 가득 차게 될 수 있습니다.
* 기본값은 100000입니다.
streamfwdcapture
################################################ ################################################ #########################
# streamfwdcapture
#
# 기본적으로 streamfwd는 사용 가능한 모든 네트워크 인터페이스에서 트래픽을 수신합니다.
# streamfwdcapture 매개 변수를 사용하면 데이터 캡처를 특정 인터페이스로만 제한 할 수 있습니다.
################################################ ################################################ #########################

streamfwdcapture. <N> .bitsPerSecond = <정수>
* 오프라인이 참인 경우에만 적용됩니다.
* 속도 제한 : 정의되지 않은 경우 <반복>이 참이면 기본값은 10Mbps이고 그렇지 않으면 100Mbps입니다.

streamfwdcapture. <N> .filter = <BPF>
* 커널 수준 패킷 필터링을위한 BPF (Berkeley Packet Filter)를 설정할 수 있습니다. 이 태그의 값은 BPF 구문을 준수해야합니다.
* streamfwdcapture 매개 변수 당 하나의 필터 변수 만 지원됩니다.

streamfwdcapture. <N> .interface = <문자열>
* 네트워크 인터페이스 이름 또는 pcap 파일 경로 또는 pcap 파일 디렉토리를 지정합니다.

streamfwdcapture. <N> .interfaceRegex = <정규식>
* 캡처 할 네트워크 인터페이스를 지정하는 정규식.

streamfwdcapture. <N> .offline = <부울>
* True는 pcap 파일 사용을 의미합니다. interface는 pcap 파일 또는 pcap 파일을 모니터링 할 디렉토리 여야합니다.
* False는 인터페이스가 네트워크 장치 이름임을 의미합니다.
* 기본값은 거짓입니다.

streamfwdcapture. <N> .repeat = <부울>
* 인터페이스가 pcap 파일 인 경우에만 적용됩니다.
* True는 연속로드를 위해 pcap 파일을 반복적으로 재생하는 것을 의미합니다.

streamfwdcapture. <N> .afterIngest = 삭제 | move [<subdir>] | 무시 | 반복 | 중지
* 인터페이스가 디렉토리 인 경우에만 적용됩니다.
* 디렉토리에서 pcap 파일을 수집 한 후 수행 할 작업을 지정합니다.
* 삭제 : 파일을 삭제합니다.
* move [<subdir>] : 파일을 하위 디렉터리로 이동합니다 (필요한 경우 생성됨). 기본값은 finished_pcaps입니다.
* 무시 : 파일을 그대로두고 이미 처리 된 것으로 표시합니다.
* 반복 : 모든 pcap 파일을 계속해서 다시 처리합니다.
* 중지 : 파일에서 나갑니다. 각 디렉토리를 한 번 처리 한 후 모니터링을 중지하십시오.
* 기본값은 이동입니다.

streamfwdcapture. <N> .sysTime = <부울>
* 오프라인이 참인 경우에만 적용됩니다.
* True는 pcap 파일의 실제 타임 스탬프 대신 패킷 타임 스탬프에 시스템 시간을 사용함을 의미합니다. 기본값은 거짓입니다.

streamfwdcapture. <N> .munge = <부울>
tcpServer
################################################ ################################################ #########################
# tcpServer
#
# 스트림 포워더는 TCP 연결의 시작을 캡처 할 때 클라이언트 및 서버 엔드 포인트를 자동으로 감지합니다.
# TCP 연결을 설정 한 후 트래픽 캡처를 시작하면 일반적으로 Stream Forwarder는
# 첫 번째 패킷은 클라이언트입니다.
# tcpServer 매개 변수를 사용하여 특정 TCP 서버의 엔드 포인트를 정의하여이 동작을 수정할 수 있습니다.
# 패킷을 보낸 사람이 끝점과 일치하면 Stream Forwarder는이를 서버 응답 패킷으로 올바르게 분류합니다.
################################################ ################################################ #########################

tcpServer. <N> .address = <ip 주소>

tcpServer. <N> .addressWildCard = <주소 마스크>

tcpServer. <N> .port = <정수>
sslServer
################################################ ################################################ #########################
# sslServer
#
# 스트림 포워더는 엔드 포인트 암호화를 감지하고 사용 가능한 개인 키를 사용하여 SSL 세션을 해독하려고합니다.
# 선택적으로 sslServer 매개 변수를 추가하여 트래픽을 암호화 된 것으로 명시 적으로 정의 할 수 있습니다.
################################################ ################################################ #########################

sslServer. <N> .address = <ip 주소>

sslServer. <N> .port = <정수>
netflowReceiver
################################################ ################################################ #########################
# netflowReceiver
#
# 기본적으로 streamfwd는 사용 가능한 모든 네트워크 인터페이스에서 트래픽을 수신합니다.
# netflowReceiver 매개 변수를 사용하면 streamfwd는 네트워크 장치에서 흐름 데이터 (netflow / sflow)를 수신 할 수 있습니다.
################################################ ################################################ #########################

netflowReceiver. <N> .ip = <ip 주소>
* 바인딩 할 IP 주소. 기본값은 사용 가능한 첫 번째 IP를 사용합니다.

netflowReceiver. <N> .port = <정수>
* 흐름 데이터를 수신하는 포트 번호.

netflowReceiver. <N> .decoder = <흐름 디코더>
* 청취 할 흐름 프로토콜. 유효한 값은 netflow 및 sflow입니다.

netflowReceiver. <N> .filter = <ip 주소>
*이 streamfwd 인스턴스에 흐름 데이터를 보낼 수있는 쉼표로 구분 된 IP 주소 목록입니다. 기본값은 모든 IP가 데이터를 보낼 수 있도록 허용합니다.
netflowElement
################################################ ################################################ #########################
#netflowElement
#
# 엔터프라이즈 특정 흐름 요소를 추가 할 수 있습니다. 이는 엔터프라이즈 요소 ID를 스트림 전달자 어휘 용어에 매핑합니다.
# vocabulary.xml 파일의 steamfwd 어휘에 termid를 추가하십시오.
################################################ ################################################ #########################

netflowElement. <N> .enterpriseid = <정수>
* IANA는 여기 http://www.iana.org/assignments/enterprise-numbers/enterprise-numbers에 정의 된대로 Palo Alto Networks의 경우 25461과 같은 엔터프라이즈 ID를 정의했습니다.

netflowElement. <N> .id = <정수>
* Enterprise 요소 ID 예 : 100

netflowElement. <N> .termid = <어휘 용어>
* 위의 요소 id가 예를 들어 netflow-paloalto.user-id에 매핑되는 streamfwd 어휘 용어.


Stream Forwarder에 대한 데이터 구성 관리
설치시 Splunk Stream은 Splunk Stream 용 앱 splunk_TA_stream에서 Wire Data 용 Splunk Stream TA를 통해 인덱스 로 데이터를 전송하도록 유니버설 포워더를 자동으로 구성 합니다 Splunk_TA_stream_wire_data.

Stream Forwarder Splunk_TA_stream통계 데이터 용 Splunk 추가 기능 관리
Stream Forwarder 용 Splunk 애드온은 운영 통계 및 로그 데이터를 _internal인덱스로 보냅니다 . Splunk App for Stream은이 데이터를 사용하여 Stream Estimate 대시 보드를 비롯한 기본 제공 대시 보드를 채 웁니다.

Splunk_TA_stream색인에 세 가지 이벤트 유형 을 보냅니다 _internal.

stream:stats
stream:log
streamfwd*
streamfwd*이벤트 유형은 스트림 전달자의 로컬 로그 파일에서 비롯됩니다. 전달자는 $SPLUNK_HOME/var/log/splunk/디렉토리의 다른 파일과 함께 해당 로그 파일을 모니터링합니다 . 포함 된 데이터 streamfwd*는에서 찾은 데이터와 동일합니다 stream:log. streamfwd*중복 데이터 인덱싱을 방지하기 위해 이벤트 유형을 안전하게 차단할 수 있습니다 .

자세한 내용은 데이터 가져 오기 매뉴얼 의 특정 수신 데이터 포함 또는 제외를 참조 하십시오 .

유니버설 포워더 데이터 제한 수정
기본적으로 Splunk 유니버설 포워더는 최대 256Kbps의 데이터를 인덱서로 보냅니다. 사용자에 따라 streamfwd구성, 배포 이것보다 더 많은 데이터를 생성 할 수 있습니다.

기본 유니버설 포워더 제한을 수정하거나 제거하려면 :

1. 편집 $SPLUNK_HOME/etc/apps/SplunkUniversalForwarder/local/limits.conf.

2.[thruput] 스탠자를 수정하십시오 . 예를 들면 :

[처리량]
최대 KBps = 0

전달자에서 암호 해독을 위해 SSL 키 사용
SSL 개인 키를 사용하여 Splunk Stream Forwarder에서 캡처 한 데이터를 해독 할 수 있습니다 .

이렇게하려면 동일한 개인 키를 사용하는 RSA 암호를 사용하여 데이터를 암호화해야합니다.

일부 웹 서버는 RSA 개인 키를 사용하지 않는 세션 암호를 협상합니다. 이러한 임시 키 교환 프로토콜 (예 : Diffie-Hellman)은 수동 관찰자가 트래픽을 해독하는 것을 불가능하게하므로 Splunk Stream에서 지원하지 않습니다.

Splunk Stream이 암호화 된 모든 트래픽을 가로 챌 수 있도록 웹 서버에서 임시 암호에 대한 지원을 비활성화 할 수 있습니다. SSL을 구성하면 웹 서버는 연결에 동일하게 효과적인 대체 암호를 사용합니다.

SSL 개인 키 추가
SSL 키가 PEM 개인 키 파일인지 확인합니다.
----- 암호화 된 개인 키 시작 -----
MIIFDjBABgkqhkiG9w0BBQ0wMzAbBgkqhkiG9w0BBQwwDgQIS2qgprFqPxECAggA
MBQGCCqGSIb3DQMHBAgD1kGN4ZslJgSCBMi1xk9jhlPxP3FyaMIUq8QmckXCs3Sa
9g73NQbtqZwI + 9X5OhpSg / 2ALxlCCjbqvzgSu8gfFZ4yo + Xd8VucZDmDSpzZGDod
A .... 그와 같은 많은 라인 .... .... 그와 같은 많은 라인 .... 
X0R + meOaudPTBxoSgCCM51poFgaqt4l6VlTN4FRpj + c / WZeoMM / BVXO + nayuIMyH
blK948UAda / bWVmZjXfY4Tztah0CuqlAldOQBzu8TwE7WDwo5S7lo5u0EXEoqCCq
H0ga / iLNvWYexG7FHLRiq5hTj0g9mUPEbeTXuPtOkTEb / 0ckVE2iZH9l7g5edmUZ
GEs =
----- 암호화 된 개인 키 종료 -----
로 이동하십시오 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/linux_x86_64/bin.
streamfwd --addsslkey명령을 사용하여 PEM 개인 키 파일을 추가합니다.
 ./streamfwd --addsslkey <key_name> <pem_file> <password>
이렇게하면 새 개인 키 파일이 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/local/keystore.db. keystore.dbAES-256 암호를 사용하여 SSL 키를 보호합니다.
streamfwd를 다시 시작합니다.
설정 > 데이터 입력으로 이동합니다 .
와이어 데이터를 클릭합니다 .
streamfwd데이터 입력을 찾습니다 . 클릭 함은 다음을 클릭 사용 .
참고 : 여러 전달자에 개인 키를 눌러 원하는 어느하여 복사 할 경우 Splunk_TA_stream당신의 전달자에 디렉토리를하거나 복사 Splunk_TA_stream로 $SPLUNK_HOME/etc/deployment-apps하고 사용 배포 서버를 추가 기능을 배포 할 수 있습니다.

PFX 파일을 PEM 파일로 변환
Windows 서버는 종종 .pfx파일 대신 파일을 사용 .pem합니다. 다음 명령을 사용하여 .pfx파일을 .pem파일 로 변환 할 수 있습니다 openSSL.

openssl pkcs12 -in CUSTOMERSKEY.pfx -nocerts -out KEYFORSTREAM.pem -nodes </ code>


