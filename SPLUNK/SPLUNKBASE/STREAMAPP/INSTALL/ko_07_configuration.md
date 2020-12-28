PCAP 파일 수집
패킷 캡처 (PCAP)는 네트워크 트래픽을 캡처하기 위해 Splunk Stream과 함께 사용할 수있는 애플리케이션 인터페이스입니다. Splunk Stream은 PCAP 파일 데이터를 수집하기위한 여러 가지 방법을 제공합니다.

Splunk Web에서 PCAP 데이터 업로드
명령 줄 옵션을 사용하여 PCAP 파일 수집
Independent Stream Forwarder를 사용하여 PCAP 파일 수집
지원되는 파일 형식
Splunk Stream은 Linux 및 Mac에서 .pcap및 .pcapng파일 형식을 지원합니다 .

Splunk Stream은 .pcapngWindows에서 파일 형식을 지원하지 않습니다 . .pcapngWindows에서 파일 을 사용하려면 먼저 .pcap파일 형식으로 변환해야 합니다.

Splunk Web에서 PCAP 데이터 업로드 및 인덱싱
Splunk Web에서 PCAP 데이터를 업로드하고 인덱싱하려면 검색 헤드에 두 개의 Splunk Stream 패키지가 설치되어 있어야합니다.

Stream 용 Splunk 앱 splunk_app_stream
Stream Forwarder 용 Splunk 애드온 Splunk_TA_stream
Splunk Web에서 설정 > 데이터 입력으로 이동합니다 .
PCAP 파일 > 새로 만들기를 클릭합니다 .
모듈 식 입력 구성 :
들	기술
이름	PCAP 파일 모듈 식 입력의 이름입니다.
PCAP 파일	파일 선택을 클릭 하고 업로드 할 PCAP 파일을 선택합니다.
시스템 시간	각 패킷 읽기에 대한 타임 스탬프로 시스템 시간 시계를 사용합니다.
반복	streamfwd프로세스가 종료 될 때까지 PCAP 파일을 계속 반복합니다 .
호스트 필드 값	PCAP 이벤트에 나타날 호스트의 이름입니다.
인덱스	PCAP 파일 데이터의 대상 인덱스입니다.
다음을 클릭 합니다.
PCAP 파일 데이터가 업로드되고 지정된 인덱스로 전송됩니다.
명령 줄 옵션을 사용하여 PCAP 파일 수집
PCAP 파일 데이터를 읽고 해당 데이터를 Splunk 인덱서로 보내려면 Splunk_TA_stream설치되어 있어야합니다.

streamfwd [-r FILE1] ... [--pcapdir DIR1] ... [pcap_options] [options] [output_option]
참고 : 상대 파일 또는 디렉토리는 현재 작업 디렉토리에 상대적입니다.

자세한 streamfwd명령 줄 옵션 및 사용 정보 는이 설명서의 streamfwd 명령 줄 옵션 을 참조하십시오 .

PCAP 파일 읽기
-r옵션을 사용하여 개별 PCAP 파일을 읽습니다.

./streamfwd -r my.pcap
디렉터리에서 PCAP 파일 수집
--pcapdir DIR옵션을 사용하여 디렉토리에서 PCAP 파일을 모니터링하고 색인화합니다.

./streamfwd --pcapdir ~ / test_pcap_dir --afteringest repeat
명령의 출력 동작 동작은 구성에서 사용하는 Stream 전달자의 유형에 따라 다릅니다.

Independent Stream Forwarder를 사용하는 경우 출력은 HTTP 이벤트 수집기에 의해 인덱서로 전송됩니다.
Stream Forwarder 용 Splunk Add-on을 사용하면 Stream Wire Data 용 Splunk Add-on을 사용하여 출력이 인덱서로 전달됩니다.
자세한 내용은 streamfwd 명령 줄 옵션을 참조하십시오 .

참고 : 디렉토리에서 PCAP 파일을 수집 할 때 파일 데이터가 잘리지 않도록 파일을 디렉토리로 이동하기 전에 PCAP가 완료되었는지 확인하십시오. PCAP 파일이 완성 될 때까지 다른 파일 확장자 이름 (예 : .temp)을 사용한 다음 확장자 이름을 .pcap.

Independent Stream Forwarder를 사용하여 PCAP 파일 수집
Independent Stream Forwarder를 구성하여 다음을 수행 할 수 있습니다.

개별 PCAP 파일 수집
특정 디렉토리에서 PCAP 파일을 모니터링하고 수집합니다.
실시간 네트워크 트래픽 모니터링,
동시에 이들의 모든 조합.
PCAP를 사용하도록 독립 Stream Forwarder를 구성하려면에 streamfwdcapture매개 변수를 추가 합니다 streamfwd.conf.

매개 변수	기술
streamfwdcapture.<N>.interface	네트워크 인터페이스 이름 또는 PCAP 파일 경로 또는 PCAP 파일 디렉터리를 지정합니다.
streamfwdcapture.<N>.interfaceRegex	캡처 할 네트워크 인터페이스를 지정하는 정규식입니다.
streamfwdcapture.<N>.offline	* True는 Independent Forwarder가 PCAP 파일을 사용함을 의미합니다. 인터페이스는 PCAP 파일을 모니터링 할 PCAP 파일 또는 디렉토리 여야합니다.
False는 인터페이스가 네트워크 장치 이름임을 나타냅니다.
기본값은 false입니다.
streamfwdcapture.<N>.repeat	* 인터페이스가 PCAP 파일 인 경우에만 적용됩니다.
True는 Independent Forwarder가 지속적인로드를 위해 PCAP 파일을 반복적으로 재생 함을 나타냅니다.
streamfwdcapture.<N>.afterIngest	* 인터페이스가 디렉토리 인 경우에만 적용됩니다.
디렉토리에서 PCAP 파일을 수집 한 후 취할 조치를 지정합니다.
가능한 값 :
delete: 파일을 삭제합니다.
move <subdir>: 파일을 하위 디렉토리 (필요한 경우 생성됨)로 이동합니다. 기본값은finished_pcaps.
ignore: 파일을 그대로두고 이미 처리 된 것으로 표시합니다.
repeat: 모든 PCAP 파일을 계속해서 다시 처리합니다.
stop: 파일을 그대로 둡니다. 각 디렉토리를 한 번 처리 한 후 모니터링을 중지하십시오.

* 기본값은 move입니다.

streamfwdcapture.<N>.sysTime	* streamfwdcapture.<N>.offline가 true로 설정된 경우에만 적용됩니다 .
True는 Independent Forwarder가 PCAP 파일의 타임 스탬프 대신 패킷 타임 스탬프에 시스템 시간을 사용하도록 지시합니다.
기본값은 거짓입니다.
streamfwdcapture.<N>.bitsPerSecond	* 오프라인이 참인 경우에만 적용됩니다.
* Rate limiter : 정의되지 않은 경우 <Repeat>가 참이면 기본값은 10Mbps이고 그렇지 않으면 100Mbps입니다.
참고 :streamfwdcapture.<N>.interface 매개 변수는 모두 절대 및 상대 디렉토리를 지원합니다. 상대 디렉터리는 Splunk_TA_stream/default(Splunk App for Stream) 또는 streamfwd/default(Independent Stream Forwarder)에 상대적 입니다.

예
다음 예 streamfwd.conf는 streamfwdcapture매개 변수를 사용하여 PCAP 파일을 수집 하는 구성을 보여줍니다 . 이러한 예는 Stream Forwarder 및 Independent Stream Forwarder 용 Splunk 추가 기능에 적용됩니다.

단일 PCAP 파일 수집
PCAP 파일을 수집하고 /tmp/server1.pcap무기한 반복하려면 다음 매개 변수를 추가하십시오.

[streamfwd]
streamfwdcapture.0.offline = true
streamfwdcapture.0.interface = /tmp/server1.pcap
streamfwdcapture.0.repeat = true
어디 streamfwdcapture.0.offline = truePCAP 섭취 할 수 있습니다.

단일 디렉터리 모니터링
단일 디렉토리에서 PCAP 파일을 모니터링하고 수집하려면 편집 /tmp/test_pcap_dir하여 다음 매개 변수를 추가하십시오.

[streamfwd]
streamfwdcapture.0.offline = true
streamfwdcapture.0.interface = / tmp / test_pcap_dir
이 예에서는 streamfwdcapture.0.offline = truePCAP 수집을 활성화합니다.

참고 : 경우 afterIngest매개 변수가 지정되지는 move옵션은 기본적으로 사용됩니다. 그러면 ./finished_pcapPCAP 수집 후 자동으로 PCAP가 하위 디렉터리로 이동합니다 .

여러 디렉터리 모니터링
둘 이상의 디렉토리에서 PCAP 파일을 모니터링하고 수집하려면 여러 streamfwdcapture.<N>그룹을 사용하십시오 . 각 디렉토리는 다른 옵션을 가질 수 있습니다.

[streamfwd]
streamfwdcapture.0.offline = true
streamfwdcapture.0.interface = C : \ temp \ pcap_dir_1
streamfwdcapture.0.sysTime = true

streamfwdcapture.1.offline = true
streamfwdcapture.1.interface = C : \ temp \ pcap_dir_2
streamfwdcapture.1.afterIngest = 삭제
이 예에서 :

pcap에만 적용 C:\temp\pcap_dir_2되므로의 파일은 원래 타임 스탬프를 사용하여 처리 streamfwdcapture.0.sysTime됩니다 C:\temp\pcap_dir_1.
C:\temp\pcap_dir_2수집 후의 파일 이 삭제됩니다.
에서 파일 C:\temp\pcap_dir_1이로 이동되었습니다 C:\temp\pcap_dir_1\finished_pcaps.
네트워크 인터페이스와 디렉토리 모두 모니터링
pcap지정된 디렉토리에서 파일을 모니터링하고 수집하는 동시에 라이브 인터페이스에서 트래픽을 캡처하려면 여러 streamfwdcapture.<N>그룹을 사용하십시오 . 예를 들면 :

streamfwdcapture.0.offline = false

streamfwdcapture.1.offline = true
streamfwdcapture.1.interface = / tmp / test_pcap_dir
이 예에서 :

streamfwdcapture.0.offline = falsestreamfwdcapture.0.interface지정되지 않았 으므로 사용 가능한 모든 네트워크 인터페이스에서 모니터링을 활성화 합니다.
streamfwdcapture.1.offline = true디렉터리 pcap에서 수집 할 수 있습니다 /tmp/test_pcap_dir.
네트워크 인터페이스 지정에 대한 자세한 내용은 이 매뉴얼에서 streamfwdcapture를 사용 하여 네트워크 인터페이스 지정을 참조 하십시오 .


Splunk Stream을 사용하여 Netflow 및 IPFIX 데이터 수집
NetFlow 및 IPFIX (IP Flow Information Export)는 네트워크 장치에서 흐름 데이터를 수집하는 프로토콜입니다. Splunk Stream을 사용하여 Netflow 및 IPFIX 데이터를 수집합니다. Splunk Stream을 사용하여 Netflow 및 IPFIX 데이터를 수집 할 수 있습니다. Splunk Stream은 UDP 프로토콜을 통해 전송되는 흐름 데이터를 지원합니다.

인덱서 구성
Http_inputSplunk 플랫폼 배포의 인덱서 에서 수신기를 활성화합니다 .

splunk_httpinput디렉토리로 이동하십시오 .
단일 인스턴스 배포의 경우 다음으로 이동합니다. $SPLUNK_HOME/etc/apps/splunk_httpinput/local/
분산 배포의 경우 다음으로 이동합니다. $SPLUNK_HOME/etc/master-app/splunk_httpinput/local/
inputs.conf파일이없는 경우 파일을 만듭니다 .
inputs.conf수신을 사용하려면 스탠자를 열고 추가하십시오. 예를 들면 :
[http] 
비활성화 됨 = 0 
포트 = 8088 
전용 IoThreads = 8 

[http : // streamfwd] 
비활성화 됨 = 0
index = main
토큰 = 152B6F2B-8A21-4850-A444-CB0646FD88BE 
indexes = _internal, main
변경 사항을 저장하고 종료하십시오.
Splunk 플랫폼 배포를 다시 시작하십시오.
(선택 사항) Splunk Stream Forwarder를 수정하고 클러스터형 인덱서로 푸시
인덱서 클러스터링을 사용하는 Splunk 플랫폼 배포의 경우 Splunk Stream Forwarder를 수정하십시오.

Splunk_TA_stream/defaultSplunk 플랫폼 배포에서로 이동 합니다.
다음 파일이있는 경우 제거하십시오.
inputs.conf
inputs.conf.spec.
수정 된 항목 Splunk_TA_stream을 Splunk 플랫폼 배포의 모든 인덱서에 푸시 합니다.
독립 스트림 전달자 구성
Splunk 플랫폼 배포에서 Independent Stream Forwarder를 구성하십시오.

배포의 Independent Stream Forwarder에서 streamfwd.conf.
streamfwd.conf전달을 열고 활성화합니다.
[streamfwd] 
httpEventCollectorToken = 152B6F2B-8A21-4850-A444-CB0646FD88BE 
# (이것을 인덱서의 토큰과 일치) 
ipAddr = 0.0.0.0 
processingThreads = 4
당신에 streamfwd.conf파일 배포의 NetFlow를 구성을 추가합니다.
[streamfwd]

httpEventCollectorToken = <GUID>

indexer.0.uri = <HEC VIP>
netflowReceiver.0.port = 9996
netflowReceiver.0.decoder = netflow
netflowReceiver.0.ip = 172.18.1.4
netflowReceiver.0.decodingThreads = 16
변경 사항을 저장하십시오.
Splunk 플랫폼 배포를 다시 시작하십시오.
독립적 인 Stream Forwarder의 etc/sysctl.conf디렉터리로 이동합니다 .
대용량 패킷 캡처를 위해 버퍼 크기를 늘리려면 커널 설정을 조정하십시오.
sysctl -w net.core.rmem_default = 33554432
sysctl -w net.core.rmem_max = 33554432
sysctl -w net.core.netdev_max_backlog = 10000
설정을 다시로드합니다.
/ sbin / sysctl -p
streamfwd서비스를 다시 시작하십시오 .
서비스 streamfwd 재시작
검색 헤드 구성
Splunk App for Stream이 설치된 검색 헤드에 로그인합니다.
Splunk App for Stream으로 이동 한 다음 Configuration > Distributed Forwarder Management 를 클릭 합니다.
새 그룹 만들기를 클릭합니다 .
이름을 입력하십시오. 예를 들면 INFRA_NETFLOW입니다.
설명을 입력하십시오.
다음을 클릭 합니다.
규칙으로 INFRA_NETFLOW를 입력하고 다음을 클릭 합니다.
옵션을 선택하지 마십시오. 마침을 클릭 합니다 .
Splunk App for Stream으로 이동 한 다음 Configuration > Configure Streams 를 클릭 합니다.
새 스트림 > 메타 데이터를 클릭 합니다 .
이름 을 INFRA_NETFLOW로 입력합니다 .
프로토콜로 NetFlow 를 선택하십시오 .
NetFlow 옵션은 NetFlow, sFlow, jFlow 및 IPFIX 프로토콜에서 작동합니다.

설명을 입력 한 후 클릭 다음 .
선택 없음 의 집계를 다음을 클릭있는 상자가 다음을 .
(선택 사항)을 선택 취소 모든 사용 사례에 적용되지 않는 필드는 다음을 클릭 다음 .
(선택 사항) 다음을 클릭합니다 높은 트래픽 장치에서 노이즈 감소 필터를 개발 Next (다음)를 .
이 컬렉션에 대한 인덱스를 선택하고 클릭 사용을 클릭 한 다음 다음을 .
단지 선택 Infra_netflow group하고 Create_Stream.
레코드를 새 streamfwd.
Splunk 플랫폼 배포에서 구성된 인덱스를 검색하여 결과를 검증하십시오.
흐름 수집기 구성
Splunk Stream은 네트워크 장치에서 플로우 프로토콜 데이터 수집을 지원합니다. 스위치, 라우터, 방화벽이있는 경우. 또는 NetFlow 및 sFlow와 같은 흐름 프로토콜 데이터를 생성하는 기타 요소의 경우 흐름 데이터 수집을 지원하도록 Splunk Stream Forwarder 또는 Splunk Independent Stream Forwarder를 구성 할 수 있습니다.

지원되는 흐름 프로토콜
Stream은 다음 흐름 프로토콜의 수집을 지원합니다.

NetFlow 버전 5, 9 및 IPFIX.
sFlow 버전 5
jFlow
Splunk Stream은 UDP 프로토콜을 통해 전송되는 흐름 데이터를 지원합니다.

흐름 수집 확장 모범 사례
흐름 프로토콜 수집을 확장 할 때 다음 모범 사례를 고려하십시오.

최상의 결과를 얻으려면 독립 스트림 전달자를 사용하십시오. 독립 스트림 전달자 배포를 참조 하십시오 .
인덱서 클러스터 노드간에로드를 분산하도록 Nginx 또는 다른로드 밸런서를 구성합니다.
적절한 경우 HEC 입력에서 SSL을 비활성화합니다. (Splunk Cloud로 데이터를 전송하거나 다른 보안 고려 사항이 적용되는 경우 SSL을 비활성화하지 마십시오.)
참고 : Splunk Stream Forwarder 및 Splunk Independent Stream Forwarder 배포는 모두 흐름 프로토콜 수집을 지원합니다. 그러나에서 사용하는 Stream Wire Data 용 Splunk의 제한된 수집 기능으로 인해 Splunk_TA_streamSplunk Stream Forwarder를 낮은 대역폭 또는 집계 된 NetFlow 캡처에만 사용하는 것이 좋습니다.

흐름 데이터 수집 구성
흐름 데이터를 수집하려면 streamfwd특정 IP 주소 및 포트에서 데이터를 수신 하도록 구성 하고 흐름 프로토콜을 지정합니다. 이를 수행하려면 streamfwd.conf다음과 같이 플로우 구성 매개 변수 세트를 추가하십시오 .

편집 local/streamfwd.conf.
다음 매개 변수를 추가하여 바인딩 할 IP 주소, 바인딩 할 포트 번호 및 흐름 프로토콜을 지정합니다.
netflowReceiver. <N> .ip = <ip_address>
netflowReceiver. <N> .port = <포트 _ 번호>
netflowReceiver. <N> .decoder = <흐름 _ 프로토콜>
예를 들어, 각각 포트 9995 및 6343의 IP 주소 172.23.245.122에서 NetFlow 및 sFlow 데이터를 수신하려면 streamfwd.conf다음과 같이 구성하십시오 .

[streamfwd]
logConfig = streamfwdlog.conf
포트 = 8889

netflowReceiver.0.ip = 172.23.245.122
netflowReceiver.0.port = 9995
netflowReceiver.0.decoder = netflow

netflowReceiver.1.ip = 172.23.245.122
netflowReceiver.1.port = 6343
netflowReceiver.1.decoder = sflow
NetFlow 데이터가있는 대용량의 경우 다음과 같이 추가 NetFlow 처리 스레드를 구성합니다.

netflowReceiver.0.decodingThreads = 4
Splunk를 다시 시작하십시오.
기본적으로 netflowReceiver.<N>.ip매개 변수는 사용 가능한 첫 번째 IP 주소에 바인딩됩니다. netflowReceiver.<N>.port및 netflowReceiver.<N>.decoder구성 매개 변수에 대한 기본값은 없습니다 .

독점 요소 매핑 구성
Splunk Stream은 IPFIX 독점 요소를 Stream Forwarder 어휘 용어에 매핑하는 것을 지원합니다. 이를 통해 스트림 구성 UI에서 생성 한 NetFlow 프로토콜 스트림 구성의 필드로 독점적 인 흐름 요소를 추가하고 지정할 수 있습니다. 이 기능을 구현하려면 Splunk 지원 담당자에게 문의하십시오.

흐름 데이터 검색 구문
NetFlow 또는 sFlow 프로토콜 데이터에 대한 검색을 실행하려면 다음 검색 구문을 사용하십시오.

sourcetype = stream : netflow
sourcetype = stream : sflow
흐름 프로토콜 스트림 만들기
streamfwd.conf흐름 데이터 수집 을 구성한 후에 는 Splunk App for Stream에서 Configure Streams UI를 사용하여 고유 한 필드 정의가있는 NetFlow 및 sFlow 프로토콜 스트림을 생성 할 수 있습니다. 스트림 구성을 참조하십시오 .

NetFlow 이벤트 타임 스탬프 계산 방법
NetFlow 데이터에 다음 필드가있는 경우 Stream Forwarder는 Splunk timestamp필드를 NetFlow flowStart*필드에 포함 된 값으로 설정 하고 Splunk endtime필드 값을 NetFlow flowEnd*필드에 포함 된 값으로 설정 합니다.

flowStartSeconds
flowEndSeconds
flowStartMilliseconds
flowEndMilliseconds
flowStartMicroseconds
flowEndMicroseconds
flowStartNanoseconds
flowEndNanoseconds
흐름과 관련이없는 NetFlow 레코드의 경우 observationTime*필드를 사용할 수있는 경우 Stream Forwarder가 NetFlow 의 Splunk timestamp및 endtime필드 값을 채 웁니다 observationTime*.

NetFlow 데이터에 flowStart*및 observationTime*필드 가 모두있는 경우 Stream Forwarder는 Splunk 검색 timestamp을 NetFlow flowStart*값으로 설정하고 Splunk 검색 endtime필드에 NetFlow observationTime*값 을 포함하도록 설정 합니다.

위의 필드가없고 NetFlow 레코드에 다음 필드가있는 경우 :

"첫 번째 스위치"(flowStartSysUpTime)
"마지막 스위치"(flowEndSysUpTime)
"시스템 가동 시간"
"유닉스 시대의 현재 장치 시간"
다음 스트림 전달자는 인 Splunk 검색을 계산 timestamp하고 endtime같은 다음과 같습니다 :

timestamp = ( "유닉스 시대의 장치 시간"- "시스템 가동 시간") + "처음 전환됨"(flowStartSysUpTime)
endtime = ( "유닉스 시대의 기기 시간"- "시스템 가동 시간") + "마지막 전환"(flowEndSysUpTime)

스트림 구성 템플릿 사용
스트림 구성 템플릿은 Splunk 제품에 대한 프로토콜 필드 매핑을 제공하는 미리 정의 된 스트림 구성입니다.

Splunk IT 서비스 인텔리전스 (ITSI) : ITSI 구성 템플릿은 Splunk ITSI 모듈의 메트릭에 매핑되는 사용자 지정 프로토콜 필드를 제공합니다.
ES (Enterprise Security) : ES 구성 템플릿은 Splunk ES에서 사용되는 CIM 데이터 모델에 매핑되는 사용자 지정 프로토콜 필드를 제공합니다.
streamfwd데이터 캡처를 구성 할 수있는 명령 줄 옵션을 사용 하여 바이너리에 구성 템플릿을 적용 할 수 있습니다 . Stream forwarder와 ISF는 모두 구성 템플릿을 지원합니다.

Stream 구성 템플릿 활성화
Stream 구성 템플릿을 활성화하려면 configTemplateName=<product name>매개 변수를에 추가합니다 streamfwd.conf. streamfwd명령 옵션을 사용 하여이 매개 변수를 추가하거나 streamfwd.conf파일을 수동으로 편집 할 수 있습니다. 한 번에 하나의 활성 스트림 구성 템플릿을 사용할 수 있습니다.

Stream은 streamfwd설치된 템플릿을 활성화, 비활성화 또는 나열 하는 다음 명령 옵션을 제공합니다 .

  -c [TEMPLATE_NAME] 지정된 제품 템플릿을 활성화합니다.
  -c 활성 제품 템플릿을 비활성화합니다.
  --listtemplates 설치된 제품 템플릿을 나열합니다.
예를 들어, ITSI 구성 템플릿을 활성화하려면 :

./streamfwd -c itsi
예 : Splunk Stream Forwarder에서 구성 템플릿 활성화
에 대한 itsi구성 템플릿 을 활성화하려면 Splunk_TA_stream:

$ SPLUNK_HOME / etc / apps / Splunk_TA_stream / linux_x86_64 / bin으로 이동합니다.
다음 명령을 실행하십시오.
[root @ sr-centos2 bin] # ./streamfwd -c itsi
/ opt / splunk / etc / apps / Splunk_TA_stream / configs / itsi에있는 구성 템플릿이 활성화되었습니다. 
Splunk를 다시 시작하십시오.
configTemplateName = itsi매개 변수가에 추가 되었는지 확인합니다 Splunk_TA_stream/local/streamfwd.conf. 예를 들면 :
[streamfwd]
포트 = 8889
ipAddr = 127.0.0.1

configTemplateName = itsi
예 : Independent Stream Forwarder에 대한 구성 템플릿 활성화
독립 Stream Forwarder 배포는 HEC (HTTP Event Collector)를 사용하여 인덱서에 데이터를 보냅니다. Independent Stream Forwarder 배포에 대한 구성 템플릿을 활성화 할 때 하나 이상의 indexer.0.uri = <indexer_location>매개 변수를 수동으로 추가 하여 인덱서 위치를 지정합니다.

esIndependent Stream Forwarder 배포를위한 구성 템플릿 을 활성화하려면 :

로 이동하십시오 opt/streamfwd/bin.
다음 명령을 실행하십시오.
[root @ sr-centos2 bin] # ./streamfwd -c es
/ opt / streamfwd / configs / es에있는 구성 템플릿이 활성화됩니다. 
다시 시작하십시오 streamfwd.
indexer.<N>.uri = <indexer_location>인덱서 위치를 지정하려면 매개 변수를 추가하십시오 . 예를 들면 :
[streamfwd]
포트 = 8889
ipAddr = 127.0.0.1

configTemplateName = es
indexer.0.uri = http : // soln-perf110-1 : 8088
indexer.1.uri = http : // soln-perf11-2 : 8088

파일 추출 구성
파일 추출을 활성화하려면 Splunk Stream 배포를 원격 파일 서버에 매핑합니다. Splunk Stream Forwarder 및 Splunk Independent Stream Forwarder는 파일 서버를 사용하여 메타 데이터 스트림 정의에 따라 추출 된 파일을 저장합니다. 자세한 내용은 사용 파일 추출 인 Splunk 스트림의 사용 설명서 .

원격 파일 서버에 배포 매핑
에서 메타 데이터 스트림에 대한 파일 추출을 구성하기 전에 splunk_app_stream다음 구성 단계를 완료하십시오.

1. 파일 서버 설정 및 마운트
Splunk Stream Forwarder 및 Independent Stream Forwarder 배포를 위해 파일 서버를 마운트하십시오.

없는 경우 NFS (또는 유사한) 파일 서버 볼륨을 만듭니다. 자세한 내용 은 NFS 서버 설정을 참조 하십시오 .
streamfwd바이너리를 실행하는 호스트 시스템 에서 파일 서버 볼륨을 마운트하십시오.
2. 파일 서버 매개 변수 추가 streamfwd.conf
스탠자 local/streamfwd.conf에서 서버 매개 변수를 지정하도록 편집하십시오 [streamfwd].
fileServerId = <값>
fileServerMountPoint = <값>
예를 들면 :

[streamfwd]
fileServerId = 10.140.7.18:/StreamLoad
fileServerMountPoint = / streamload
Splunk를 다시 시작하십시오.
3. 검색 헤드에 파일 서버 마운트
를 실행하는 검색 헤드에서 splunk_app_stream마운트 지점을 만듭니다. 자세한 정보 는 NFS 클라이언트 설정을 참조하십시오 .

4. 파일 서버에 대한 마운트 지점 구성
에서 splunk_app_streamUI를 클릭 구성> 파일 서버 마운트 지점을 .
파일 서버 추가를 클릭합니다 .
파일 서버 및 마운트 지점을 지정하십시오. 생성을 클릭 합니다.
마운트 포인트 검색 head.png
splunk_app_stream검색 헤드 의 UI에서 지정한 마운트 지점 은에서 지정한 마운트 지점과 다릅니다 streamfwd.conf.

파일 추출 사용
Splunk Stream 배포를 원격 파일 서버에 매핑 한 후 메타 데이터 스트림에 대한 파일 추출을 구성 할 준비가되었습니다. 자세한 내용은 참조 를 사용하여 파일 추출 인 Splunk 스트림에서 수동 사용자 .

대상 패킷 캡처 구성
대상 패킷 캡처를 사용하여 전체 네트워크 패킷을 수집하려면 Splunk Stream 배포를 원격 파일 서버에 매핑하십시오. Splunk Stream Forwarder는 파일 서버를 사용하여 패킷 스트림 정의를 기반으로 생성되는 PCAP 파일을 저장합니다. 자세한 내용 은 Splunk Stream 사용 설명서 의 패킷 스트림 구성을 참조하십시오 .

원격 파일 서버에 배포 매핑
대상 패킷 캡처를 구성하려면 Splunk Stream 배포를 원격 파일 서버에 매핑합니다. Stream 용 Splunk 앱에서 새 패킷 스트림을 생성하기 전에 대상 패킷 캡처를 위해 Stream Forwarder 및 Splunk Independent Stream Forwarder 배포를 구성하십시오.

1. 파일 서버 설정 및 마운트
NFS (또는 유사한) 파일 서버 볼륨이 있는지 확인하십시오. 생성하려면 NFS 서버 설정을 참조 하십시오 .
streamfwd바이너리를 실행하는 호스트 머신에서 파일 서버 볼륨을 마운트합니다.
2. 파일 서버 매개 변수 추가 streamfwd.conf
에서 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/열려 local/streamfwd.conf.
[streamfwd]스탠자에 서버 정보를 추가하십시오 .
fileServerId = <값>
fileServerMountPoint = <값>
예를 들면 :

[streamfwd]
fileServerId = nfs : //192.168.6.1/packetcaptures
fileServerMountPoint = / usr / local / packetcaptures
Splunk를 다시 시작하십시오.
3. 검색 헤드에 파일 서버 마운트
를 실행하는 검색 헤드에서 splunk_app_stream마운트 지점을 만듭니다. 자세한 정보 는 NFS 클라이언트 설정을 참조하십시오 .

4. 파일 서버에 대한 마운트 지점 구성
Splunk App for Stream UI에서 Configuration > File Server Mount Points를 클릭 합니다.
파일 서버 추가를 클릭합니다 .
파일 서버 및 마운트 지점을 지정하십시오.
생성을 클릭 합니다.
새 패킷 스트림 생성
Splunk Stream 배포를 원격 파일 서버에 매핑 한 후 대상 패킷 캡처를 사용하여 새 패킷 스트림을 만들고 전체 네트워크 패킷을 수집 할 준비가되었습니다.

Splunk App for Stream에서 Configuration > Configure Streams를 클릭 합니다.
새 스트림 > 패킷 스트림을 클릭 합니다 .
워크 플로우 마법사의 단계에 따라 패킷 스트림을 구성합니다. 자세한 지침은 패킷 스트림 구성을 참조하십시오 .

10Gbps 네트워크 캡처 구성
대용량 10Gb 네트워크 인터페이스에서 패킷 손실을 최소화하면서 효율적인 데이터 캡처를 용이하게하기 위해 Splunk Stream Independent Forwarder를 사용하면 호환되는 네트워크 인터페이스에서 10Gbps 데이터 캡처를 지원하는 대체 Stream Forwarder를 배포 할 수 있습니다. Stream은 18.11.1 버전의 dpdk 라이브러리를 사용합니다.

이 페이지에서는 Linux 환경을 최적화하고 호환 장치에서 10Gbps 데이터 캡처를 사용하도록 Stream Forwarder를 구성하는 방법을 보여줍니다.

운영 체제 및 권한 요구 사항
전용 10Gb 캡처 모드는 64 비트 Linux 플랫폼 (커널 버전 3.10.0 이상)에서 지원됩니다. 지원되는 NICS에 대한 정보는 http://dpdk.org/doc/nics를 참조 하십시오 .
streamfwd전용 캡처 모드에서 실행하려면 Splunk 플랫폼 배포의 루트 사용자 여야 합니다.
전용 10Gb 캡처 모드는 CentOS / RHEL에서만 테스트되었습니다.

Linux 환경 최적화
전용 캡처 모드에서 최상의 결과를 얻으려면 hugepages다음과 같이에 대한 커널 부팅 매개 변수를 업데이트하십시오 .

편집 /etc/grub.conf.
커널 부트 라인에 다음 매개 변수를 추가하여 16 개의 1GB hugepages를 구성하십시오 : default_hugepagesz=1G hugepagesz=1G hugepages=16. 하드웨어 구성에 맞게 hugepage 수를 조정해야 할 수도 있습니다. 예를 들어, 이러한 매개 변수를 추가 한 후 커널 부트 라인은 다음과 같습니다.
커널 /vmlinuz-2.6.32-573.3.1.el6.x86_64 ro root = / dev / mapper / vg_cmload02-lv_root rd_LVM_LV = vg_cmload02 / lv_root rd_NO_LUKS LANG = en_US.UTF-8 rd_NO_MD SYSFONT = latarcyrheb-sun16 crashkernel = auto rd_LVM = latarcyrheb-sun16 vg_cmload02 / lv_swap KEYBOARDTYPE = pc KEYTABLE = us rd_NO_DM default_hugepagesz = 1G hugepagesz = 1G hugepages = 16 rhgb quiet
Linux 시스템을 재부팅하십시오.
전용 캡처 모드에 대한 커널 부팅 매개 변수 업데이트는 선택 사항이지만 적극 권장됩니다.

10Gb 전용 캡처 모드 구성
다음 구성 단계는 독립 스트림 전달자 streamfwd배포에 적용됩니다 . streamfwd전용 캡처 모드에서 실행하려면 루트 사용자 여야 합니다.

1 단계 : Splunk Stream Independent Forwarder에 대한 전용 캡처 모드 활성화
편집 local/streamfwd.conf.
추가 dedicatedCaptureMode = 1. 예를 들면 :
[streamfwd]
포트 = 8889
ipAddr = 127.0.0.1
dedicatedCaptureMode = 1
Splunk를 다시 시작하십시오.
2 단계 : 호환 가능한 인터페이스 식별
streamfwd --iflist명령을 사용하여 10Gbps 트래픽을 캡처하려는 인터페이스를 식별하십시오. 에 나열된 모든 인터페이스에서 10Gbps로 패킷을 캡처 할 수 있습니다 Dedicated capture mode compatible devices. 예를 들면 :

# ./linux_x86_64/bin/streamfwd --iflist
전용 캡처 모드 호환 장치
=======================================
0000 : 04 : 00.0 driver = uio_pci_generic if =
0000 : 04 : 00.1 driver = uio_pci_generic if =
0000 : 05 : 00.0 driver = uio_pci_generic if =
0000 : 05 : 00.1 driver = uio_pci_generic if =

전용 캡처 모드 비 호환 장치
===========================================
0000 : 02 : 00.0 driver = tg3 if = eth4 * 활성 *
0000 : 02 : 00.1 드라이버 = tg3 if = eth5
0000 : 02 : 00.2 드라이버 = tg3 if = eth6
0000 : 02 : 00.3 드라이버 = tg3 if = eth7
3 단계 : 네트워크 주소 지정 streamfwd.conf
편집 local/streamfwd.conf.
10Gbps 호환 장치의 네트워크 주소를 지정하십시오. 예를 들면 :
[streamfwd]
포트 = 8889
ipAddr = 127.0.0.1
dedicatedCaptureMode = 1
streamfwdcapture.0.interface = 0000 : 04 : 00.0
전용 캡처 모드에서는 PCI 버스 주소 표기법을 사용하여 네트워크 장치를 지정해야합니다.

4 단계 : UIO 드라이버 지정 streamfwd.conf
1. 편집 local/streamfwd.conf.

2. 사용할 UIO 드라이버를 지정합니다. 예를 들면 :

[streamfwd]
포트 = 8889
ipAddr = 127.0.0.1
dedicatedCaptureMode = 1
uioDriverModuleName = igb_uio
streamfwdcapture.0.interface = 0000 : 04 : 00.0
3. Splunk 플랫폼을 다시 시작합니다.

참고 : 전용 캡처 모드는 다음 세 가지 UIO 드라이버에 대한 바인딩을 지원합니다.

uio_pci_generic
vfio-pci
igb_uio
에 드라이버 이름이 지정되지 않은 경우 streamfwd.confStream은 uio_pci_generic드라이버를 사용합니다 . Splunk 플랫폼을 다시 시작하기 전에 사용중인 UIO 드라이버가로드되었는지 확인하십시오. modprobe또는 insmod명령을 사용하여 UIO를로드 할 수 있습니다 .

지원되는 네트워크 인터페이스 컨트롤러
지원되는 네트워크 인터페이스 컨트롤러는 http://dpdk.org/doc/nics를 참조 하십시오 .
Stream은 다음 네트워크 인터페이스 컨트롤러에서 테스트되었습니다.
Intel (R) 82599ES 10 기가비트 이더넷 컨트롤러
Intel Corporation 이더넷 컨트롤러 X710


