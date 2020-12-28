자주하는 질문
나만의 프로토콜을 추가 할 수 있습니까?
아니요. Splunk Stream은 프로토콜을 추가하는 메커니즘을 제공하지 않습니다.

Splunk_TA_stream에서 특정 인덱스로 트래픽을 보내려면 어떻게해야합니까?
inputs.conf에서 수정 Splunk_TA_stream/local/하여 색인을 지정할 수 있습니다 .

참고 : 이는 특정 인스턴스가 Splunk_TA_stream캡처 하는 모든 트래픽에 적용됩니다 .

프로토콜을 기반으로 데이터를 특정 인덱스로 보낼 수 있습니까?
Splunk Stream을 사용하면 프로토콜에 따라 데이터를 다른 인덱스로 보낼 수 없습니다. 그러나 props.conf및 transforms.conf파일을 사용하여이 기능을 설정할 수 있습니다 . 지침은 특정 이벤트를 다른 인덱스로 라우팅을 참조하십시오 .

특정 프로토콜을 수신하도록 엔드 포인트를 구성 할 수 있습니까?
끝점에서 특정 프로토콜을 수신하도록 스트림 필터를 구성 할 수 있습니다. 예를 들어, 공통 흐름 속성 인 s_ip (source_ip)를 사용하여 DNS 서버에서만 DNS 트래픽을 필터링 할 수 있습니다. 호스트 이름 별 필터링은 지원되지 않습니다.

참고 : 네트워크 스위치가 트래픽을 엔드 포인트로 향하는 패킷으로 만 제한하지 않기 때문에 엔드 포인트가 서로의 트래픽을 볼 수있는 경우 중복 가능성이 있습니다.

더 진보 된 구성에서의 이름이 변경된 사본을 배포 할 수 있습니다 splunk_app_stream및 Splunk_TA_stream엔드 포인트 복사하는 수신 제어에 배포 서버를 사용합니다. 이 경우 이름이 바뀐 파일 Splunk_TA_stream은 etc/apps/local/inputs.conf올바른 상위 앱을 가리 키도록 수정 해야합니다 .

주의 : 이것은 고도로 사용자 정의 된 구성입니다. 이러한 유형의 구성을 구현하기 전에 Splunk 전문 서비스에 문의하는 것이 좋습니다 .

Splunk_TA_stream이 기본적으로 검색 헤드에 설치되는 이유는 무엇입니까?
Splunk_TA_stream단일 인스턴스 배포 를 지원하기 위해 기본적으로 검색 헤드에 설치됩니다 .

Splunk_TA_stream$SPLUNK_HOME/etc/deployment-apps기본적으로 설치됩니다 . 이렇게 하면 분산 배포에 추가 할 수있는 모든 범용 포워더에 자동으로 배포 할 수 있는 배포 서버를 쉽게 사용할 Splunk_TA_stream수 있습니다 .

검색 헤드의 Splunk_TA_stream에서 데이터 캡처를 중지 할 수 있습니까?
sc_ip 필드를 사용하여 검색 헤드의 스트림 데이터를 필터링 할 수 있습니다. 또는 Splunk_TA_stream검색 헤드에서 제거 할 수 있습니다 .

Stream에서 단방향 트래픽을 캡처 할 수 있습니까 (수신 또는 송신 만 해당)?
스트림은 요청과 응답을 올바르게 결정하기 위해 전체 TCP 연결 핸드 셰이크 (및 종료)를 확인해야합니다.

TA에서 splunk_app_stream에서 구성을 가져 오도록 URL을 설정해야합니까?
Splunk_TA_streamsplunk_app_stream지정된 URL에서 와 정기적으로 통신 합니다. TA가 구성 변경을 감지 splunk_app_stream하면 업데이트 된 구성을 검색 하기 위해 GET 요청을 보냅니다 . 의 URL은 splunk_app_stream에 지정되어 Splunk_TA_stream/local/inputs.conf있습니다. 와 통신 하는 방법을streamfwdsplunk_app_stream 참조하십시오 .

Stream에서 pcap 파일을 읽을 수 있습니까?
Stream을 사용하면 다음 streamfwd명령을 사용하여 pcap 파일을 읽고 구조화 된 pcap 데이터를 인덱서에 보낼 수 있습니다 .

./streamfwd -r foo.pcap -s <host><server>.

Stream 명령 줄 옵션을 참조하십시오 .

Stream에서 원시 pcap 파일 데이터를 Splunk Enterprise로 보낼 수 있습니까?
streamfwdSplunk 인덱서로 보내는 pcap 데이터 는 원시 패킷 데이터가 아니라 구조화 된 이벤트 데이터입니다. PCAP 데이터 보내기를 참조하십시오.

Stream에서 패킷과 애플리케이션 데이터를 해독 할 수 있습니까?
streamfwd동일한 개인 키를 사용하는 RSA 암호를 사용하여 데이터가 암호화 된 경우 SSL 개인 키를 사용하여 바이너리가 캡처 하는 데이터를 해독 할 수 있습니다 .

Stream에서 Diffie-Hellman (SSL 키) 트래픽을 해독 할 수 있습니까?
streamfwd바이너리가 TAP에서 데이터를 수집 하는지 또는 호스트 자체에서 실행 되는지에 관계없이 Diffie-Hellman 트래픽을 캡처 할 방법이 없습니다 .

Chef, Puppet 및 기타 유틸리티를 사용하여 Stream 구성 파일을 배포하고 관리 할 수 ​​있습니까?
Chef, Puppet 및 기타 유틸리티를 사용하여 streamfwd바이너리를 유니버설 포워더 로 푸시 할 수 있습니다 .

참고 :streamfwd 바이너리와의 연결 유지해야 splunk_app_stream스트림 구성을 검색 할 수 있습니다. 따라서 배포 서버 + 스트림 전달자 시나리오에서는 범용 전달자 (배포 클라이언트 메커니즘을 통해 Splunk 호스트의 기본 포트 8089) 및 Splunk_TA_stream( splunk_app_stream인스턴스의 기본 포트 8000 )에서 연결을 적극적으로 유지해야합니다 . Puppet 등 시나리오에서는 엔드 포인트에서 App for Stream 호스트로의 활성 연결을 계속 유지해야합니다.

streamfwd 프로세스가 시작되지 않는 이유는 무엇입니까?
Q : 전달자의 splunkd.log 파일에 다음과 같은 불만이 있습니다.

10-07-2014 16:11:26.140 -0400 INFO ModularInputs - Introspection setup completed for scheme "streamfwd".

10-07-2014 16:11:27.029 -0400 INFO ModularInputs - No stanzas found for scheme "streamfwd" in inputs.conf at script (re)start.

10-07-2014 16:11:27.034 -0400 INFO ExecProcessor - New scheduled exec process: /opt/splunkforwarder/etc/apps/Splunk_TA_stream/linux_x86_64/bin/streamfwd

10-07-2014 16:11:32.601 -0400 ERROR ExecProcessor - message from "/opt/splunkforwarder/etc/apps/Splunk_TA_stream/linux_x86_64/bin/streamfwd" log4cplus:ERROR Unable to open file: /opt/splunk/var/log/splunk/streamfwd.log .

A : 현재 설치시 Splunk_TA_stream설치된 복사본 deployment-apps이 소스 시스템과 동일한 디렉토리 구조를 가진 시스템에 놓일 것이라는 가정이 있습니다. 위의 문제를 해결하려면 deployment-apps/Splunk_TA_stream/default/streamfwdlog.conf대상 전달자의 올바른 경로를 반영하도록 수정 한 다음 앱을 다시 배포하십시오.

모든 것이 올바르게 설정되었지만 이벤트가 표시되지 않습니다. 뭐가 문제 야?
1. streamfwd바이너리 splunk_app_stream는 구성을 검색하기 위해 정기적으로 통신 합니다. splunk_app_stream이 커뮤니케이션에 사용 된 URL 은에서 찾을 수 있습니다 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/local/inputs.conf. 스트림 이벤트를 수신하지 못하는 경우 splunk_app_streamURL에 대한 액세스를 차단하는 방화벽 규칙이 없는지 확인하십시오 .

2. Stream Forwarder가 업그레이드 후 데이터 전송에 실패하면 다음과 유사한 메시지가 표시 될 수 있습니다.

WARN [139650313393920] (HTTPRequestSender.cpp:1485) stream.SplunkSenderHTTPEventCollector - (#7) TCP connection failed: Connection refused

이 문제를 해결하려면 먼저 스트림 전달자가 올바르게 구성되었는지 확인하십시오. 그런 다음 Stream Forward 앱으로 이동하여 HEC 구성을 업데이트합니다.

Stream 앱에서 Distributed Forwarder Management 페이지를 엽니 다.
"Stream Forwarders 설치"를 선택하십시오.
curl 명령이 Stream Forward 앱에서 실행중인 명령과 동일한 지 확인하십시오.
HEC 자동 구성 옵션을 끕니다.
HEC (HF 또는 인덱서) URL을 수동으로 입력하여 끝점 URL을 업데이트합니다.

문제 해결
Wire Data 모듈 식 입력이 업그레이드 후 작동을 중지 함
Splunk를 중지하고 앱을 설치 또는 업그레이드하지 않고 애플리케이션 폴더를 수동으로 삭제하면 Wire Data 모듈 식 입력이 작동을 중지합니다. 일부 증상은 다음과 같습니다.

Wire Data 모듈 식 입력 구성 ( splunk_app_stream위치)이 없습니다.
데이터 입력에 와이어 데이터가 없습니다.
Wire Data 구성이 있지만 streamfwdUI에서 활성화 해도 효과가 없습니다.

위의 증상 1 및 2의 경우 Splunk를 다시 시작하면 문제가 해결 될 수 있습니다. 그렇지 않으면 다음 해결 방법을 따르십시오.

Splunk를 중지합니다.
cd $ SPLUNK_HOME / bin
./splunk 중지
에서 및 폴더를 $SPLUNK_HOME/etc/apps삭제 합니다.splunk_app_streamSplunk_TA_Stream
Splunk를 시작합니다.
cd $ SPLUNK_HOME / bin
./splunk 시작
Splunk Web에서 Splunk Stream을 다시 설치합니다.
UI에서 Splunk를 다시 시작합니다.
설정> 데이터 입력을 엽니 다 .

이제 Wire Data 모듈 식 입력이 UI에 나타납니다.

활성화를 클릭 합니다.
splunk_app_stream또는 을 삭제하기 전에 Splunk를 중지하십시오 Splunk_TA_Stream directories.

와이어 데이터 모듈 식 입력이 Linux에서 시작되지 않음
splunkd.log에서 다음 오류 메시지를 확인하십시오.
앱 "Splunk_TA_stream"내에 정의 된 모듈 식 입력 "streamfwd"를 초기화 할 수 없습니다. 
체계 검사 = streamfwd : 스크립트 실행 실패 (신호 6 : 중단됨)
Splunk_TA_stream/linux_x86_64/bin/streamfwd --versionCLI 에서 명령을 실행하고 결과가 다음과 같은지 확인합니다.
/ opt / splunkforwarder / etc / apps / Splunk_TA_stream / linux_x86_64 / bin / streamfwd-버전
'std :: runtime_error'인스턴스를 던진 후 종료가 호출됩니다.
  what () : locale :: facet :: _ S_create_c_locale 이름이 유효하지 않습니다.
중단됨 (코어 덤프 됨)
이 해결 방법을 사용하십시오. LC_ALL 로케일을 "en_US.UTF-8"또는 "C.UTF-8"로 설정하십시오.
export LC_ALL = "en_US.UTF-8"
스트림 전달자는 데이터를 보내지 않습니다.
이전 버전에서 Splunk Stream 7.1.3 이상으로 업그레이드 할 때 스트림 포워더가 업그레이드 후 데이터를 보내지 않으면 오류 메시지가 표시 될 수 있습니다.

WARN [139650313393920] (HTTPRequestSender.cpp : 1485) stream.SplunkSenderHTTPEventCollector-(# 7) TCP 연결 실패 : 연결 거부
이를 완화하려면 스트림 전달 기가 올바르게 구성되었는지 확인하십시오. 필요에 따라 HEC 구성을 변경합니다.

Stream 앱에서 Distributed Forwarder Management를 엽니 다.
Stream Forwarders 설치를 선택 합니다 .
curl 명령이 Stream Forward 앱에서 실행 된 명령인지 확인하십시오.
HEC 자동 구성 옵션 끄기
HEC (HF 또는 인덱서) URL을 수동으로 추가합니다.
pcap 파일을 만드는 방법
Splunk Stream 배포에 문제가 발생하면 Stream 지원 팀에서 디버깅 목적으로 pcap 파일을 제공하도록 요청할 수 있습니다.

Linux에서 pcap 만들기
tcpdumpLinux에서 pcap을 만드는 데 사용 합니다. tcpdump기본적으로 패킷에서 데이터의 처음 96 바이트를 캡처합니다. 더 많은 데이터를 캡처하려면 -s<number>스냅 샷 (스냅 샷 길이)을 설정 하는 옵션을 사용합니다. 여기서은 <number>캡처 할 바이트 수입니다. 무제한 snaplen -s0으로 실행 tcpdump하는 데 사용 합니다 .

tcpdump –i eth0 –s0 –w filename.pcap

예를 들어 포트 1521에서만 Oracle TNS 트래픽을 캡처하려면 다음을 수행하십시오.

tcpdump –i eth0 –s0 –w file.pcap tcp port 1521

참고 : 서버의 NIC 이름 목록을 보려면를 입력하십시오 tcpdump –D.

Windows에서 pcap 만들기
Wireshark와 같은 유틸리티를 사용하여 Windows에서 pcap을 만들 수 있습니다.

Wireshark에서 pcap 파일을 만드는 방법에 대한 지침은 캡처 된 패킷 저장을 참조하십시오 .

