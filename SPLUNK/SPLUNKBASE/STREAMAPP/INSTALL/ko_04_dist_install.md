분산 배포 설치 및 구성 요구 사항
Splunk Stream을 설치하기 전에 다음 필수 구성 요소가 있는지 확인하십시오.

네트워크 프로토콜
Splunk Stream이 지원하는 네트워크 프로토콜 목록은 이 설명서의 지원되는 프로토콜 을 참조하십시오 .

하드웨어 요구 사항 및 권장 사항
기본 Splunk Enterprise 배포가 Splunk Enterprise 용량 계획 매뉴얼 의 Splunk Enterprise 용량 계획 소개에 지정된 요구 사항을 충족하는지 확인하십시오 .
기본 Splunk Enterprise 하드웨어 요구 사항 은 Splunk Enterprise 용량 계획 매뉴얼 의 참조 하드웨어 를 참조 하십시오 . 캡처 및 인덱싱하려는 네트워크 데이터의 양에 따라 추가 리소스가 필요할 수 있습니다.
용량 계획에 도움이되는 Splunk Stream 성능에 대한 정보 는이 매뉴얼의 성능 테스트 결과 및 권장 사항 을 참조하십시오 .
지원되는 운영 체제
Splunk Stream 7.1.2 이상은 다음 운영 체제를 지원합니다.

리눅스
Linux 커널 버전 3.10 이상 (64 비트)
Red Hat Enterprise Linux 7.0 이상
CentOS 7.0 이상
Ubuntu 16.04 이상
참고 : Stream Forwarder 용 Splunk 추가 기능은 64 비트 Linux (RHEL 및 Ubuntu)에서 지원됩니다.

대용량 패킷 캡처에는 기본 Linux 커널 설정이 충분하지 않습니다. 이러한 설정을 사용하면 패킷 누락 및 데이터 손실이 발생할 수 있습니다. 이 문제를 방지하려면 다음 커널 설정을 /etc/sysctl.conf파일에 추가 하십시오.

# 안정적인 패킷 캡처를 위해 커널 버퍼 크기를 늘립니다.
net.core.rmem_default = 33554432
net.core.rmem_max = 33554432
net.core.netdev_max_backlog = 10000
그런 다음 다음을 실행하여 설정을 다시로드하십시오.

/ sbin / sysctl -p
맥 OS X
Mac OSX 버전 10.11 이상.
윈도우
Windows Server 2012R2 이상 (64 비트)
Splunk Stream은 Windows에서 로컬 시스템 및 관리자 계정을 지원합니다. 자세한 내용 은 Windows에서 시스템 계정을 사용하는 방법을 참조하십시오 .

Splunk Enterprise 버전 요구 사항
Splunk Stream 버전 7.3.0은 Splunk Enterprise 8.0에서 지원됩니다. Splunk Enterprise를 다운로드합니다 .

지원되는 브라우저
Splunk Stream 6.2.x 이상은 다음 브라우저를 지원합니다.

Chrome (최신)
Safari (최신)
Firefox (최신) (버전 10.x는 지원되지 않음)
Internet Explorer 9 이상. Internet Explorer 버전 9는 호환 모드에서 지원되지 않습니다.
라이선스 요구 사항
Splunk Stream에는 별도의 라이선스가 필요하지 않습니다. 단일 Splunk Enterprise 라이선스로 Splunk Enterprise에 Splunk Stream을 설치하고 사용할 수 있습니다.

Splunk Enterprise 라이선스는 Splunk 인덱서가 하루에 저장하는 데이터 양을 기준으로합니다. 자세한 내용 은 Splunk Enterprise 관리자 매뉴얼 의 Splunk 라이선스 작동 방식 을 참조하십시오 .

대상 패킷 캡처 및 파일 추출 요구 사항
대상 패킷 캡처 및 파일 추출을 사용하려면 Splunk Stream 배포를 원격 파일 서버에 매핑하십시오. 지침 은이 설명서의 대상 패킷 캡처 구성 및 파일 추출 구성을 참조하십시오 .

대상 패킷 캡처 및 파일 추출에는 Splunk Stream 버전 7.1.0 이상이 필요합니다.

NetFlow 요구 사항
NefFlow 데이터 수집에는 Splunk Stream 버전 7.0.0 이상이 필요합니다.
NetFlow 애플리케이션 ID 필드 디코딩에는 Splunk Stream 버전 7.2.0 이상이 필요합니다.
NetFlow 레코드 흐름 타임 스탬프를 기반으로하는 NetFlow 이벤트 타임 스탬프에는 Splunk Stream 버전 7.2.0 이상이 필요합니다.
NetFlow에 대한 자세한 내용은 Splunk Stream을 사용하여 Netflow 및 IPFIX 데이터 수집을 참조 하십시오 .

분산 배포 설치 및 구성 개요
Splunk Stream의 분산 배포에는 다음 구성 요소가 포함됩니다. 설치 단계 는 분산 배포에 Splunk Stream 설치를 참조하십시오.

구성 요소	용법
검색 헤드	splunk_app_stream및 Splunk_TA_wire_data검색 헤드에 필요합니다.
인덱서	Splunk_TA_wire_data 검색 및 구문 분석을 위해 모든 인덱서에 필요합니다.
유니버설 포워더	Splunk_TA_stream네트워크 데이터를 캡처하려는 위치의 유니버설 포워더에 필요합니다. 자세한 내용 은이 설명서의 네트워크 컬렉션 아키텍처 를 참조하십시오 .
무거운 포워더	
Splunk_TA_stream네트워크 데이터를 캡처하려는 위치에 설치하십시오 .
Splunk_TA_stream_wire_data해당 인덱스가 파이프 라인 처리를 수행 할 때마다 헤비 포워더에 설치 하십시오.
배포 서버	Splunk 배포 서버 를 사용하여 분산 배포 Splunk_TA_stream를 통해 유니버설 포워더에 배포합니다. 새 버전의 Splunk Stream으로 업그레이드 할 때 배포 서버가 새 버전을 감지하면 Splunk_TA_stream배포 클라이언트로 가입 한 모든 유니버설 포워더가 새 버전을 가져 와서 설치합니다. 자세한 내용은
프로비저닝 배포 서버 에서 업그레이드 인 Splunk 기업의 인스턴스. 과
Splunk Enterprise 용량 계획 매뉴얼 의 Splunk Enterprise 배포 구성 요소 .

분산 배포에 Splunk Stream 설치
Splunk Stream을 배포하려면 Splunk Enterprise 인스턴스 및 / 또는 호환되는 Linux 컴퓨터에 3 개의 Stream 구성 요소를 설치합니다.

상품명	설치 패키지 이름	설치된 파일 이름
Stream 용 Splunk 앱	splunk_app_stream	splunk_app_stream/
Stream Forwarder 용 Splunk 애드온	Splunk_TA_stream	Splunk_TA_stream/
스트림 와이어 데이터 용 Splunk 애드온	Splunk_TA_stream_wire_data	Splunk_TA_stream_wire_data/
Splunk Stream은 또한 <streamfwd>Splunk App for Stream 패키지에 바이너리 파일로 패키징 된 Independent Stream Forwarder를 제공 합니다.

Splunk Stream 구성 요소에 대한 자세한 내용 은이 설명서의 Splunk Stream 설치 패키지 개요 를 참조하십시오 .

검색 헤드에 Splunk App for Stream 설치
http://splunkbase.splunk.com/app/1809로 이동합니다 .
다운로드를 클릭 합니다. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
명령 줄로 이동하여 설치 파일의 압축을 SPLUNK_HOME/etc/apps/.
메시지가 표시되면 Splunk Enterprise를 다시 시작합니다. 그러면 Splunk App for Stream ( Splunk_app_stream)이 $SPLUNK_HOME/etc/apps.
Stream Wire Data 용 Splunk 추가 기능 설치
검색 헤드 및 인덱서에 Stream Wire Data 용 Splunk 추가 기능을 설치합니다.

http://splunkbase.com/app/5234 .
다운로드를 클릭하십시오. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
앱 관리 > 파일에서 앱 설치를 클릭 합니다 .
설치 프로그램 파일을 업로드하십시오.
메시지가 나타나면 Splunk Enterprise를 다시 시작하십시오.
배포 서버를 사용하여 Stream Forwarder 용 Splunk 추가 기능 배포
http://splunkbase.com/app/5238로 이동합니다 .
다운로드를 클릭 합니다. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
명령 줄로 이동하여 설치 패키지를 SPLUNK_HOME/etc/apps/.
메시지가 표시되면 Splunk Enterprise를 다시 시작합니다. 그러면 Splunk Add-on for Stream Forwarder ( Splunk_TA_stream)가 $SPLUNK_HOME/etc/deployment-apps디렉터리에 설치됩니다. 이것은 배포 서버를 사용하여 전달자에게 배포 할 수있는 Stream Forwarder 용 Splunk 추가 기능의 사전 구성된 복사본입니다.
Splunk_TA_stream권한을 설정 합니다.
Linux 및 OSX에서는 디렉토리 에서 set_permissions.sh스크립트를 실행하십시오 Splunk_TA_stream.
<class = 샘플 코드>
cd $ SPLUNK_HOME / etc / apps / Splunk_TA_stream
sudo chmod + x ./set_permissions.sh
sudo ./set_permissions.sh
Windows에서는 Splunk Enterprise를 관리자로 실행하거나 WinPcap을 대상 컴퓨터에 독립 실행 형 패키지로 설치합니다. 이 페이지의 Windows 설치 고려 사항을 참조하십시오.
SSL 인증서 유효성 검사 활성화
SSL 연결에 대한 인증서 유효성 검사를 활성화 Splunk_TA_stream하여 splunk_app_stream서버 의 ID를 확인 합니다. 인증서 유효성 검사를 활성화하려면에서 매개 변수를 편집합니다 inputs.conf.

편집 $SPLUNK_HOME/etc/apps/Splunk_TA_stream/local/inputs.conf하여 다음 매개 변수를 설정하십시오.
sslVerifyServerCert = true 클라이언트 (streamfwd) 측에서 서버 (splunk_app_stream) 인증서 유효성 검사를 활성화합니다.
rootCA = <path>: 루트 CA 인증서 파일의 파일 이름을 가리 킵니다.
sslCommonNameToCheck = <commonName>: 인증서 CN과 비교할 공통 이름 값을 재정의 할 수 있습니다.
스트림 데이터 용 인덱서 수신 포트 구성
인덱서 탭에서 설정 > 전달 및 수신으로 이동합니다 .
수신 구성을 클릭합니다 .
새로 만들기를 클릭 합니다 .
수신 포트 번호를 입력하십시오. 예 : 포트 9997.
저장을 클릭 합니다.
Independent Stream Forwarder 설치 및 구성
구성과 함께 작동하도록 독립 스트림 전달자를 구성하려면 이 설명서 의 독립 스트림 전달자 설치를 참조하십시오 .

간편한 설정 스트리밍
Splunk Stream은 로컬 및 원격 시스템에서 데이터 수집을 설정하고 구성하는 데 도움이되는 쉬운 설정 페이지를 제공합니다.

로컬 컴퓨터에서 데이터 수집 설정
유선 데이터를 사용하여이 컴퓨터에서 데이터 수집 확인란을 선택합니다 .

당신이 볼 경우 "Splunk_TA_stream가 제대로 구성되어 있지 않습니다"를 클릭 재검색을 . 대부분의 경우 이는 streamfwd바이너리가 네트워크 인터페이스에서 패킷을 캡처 할 수있는 적절한 권한을 설정합니다 .
여전히 "Splunk_TA_stream이 올바르게 구성되지 않았습니다."라는 메시지가 표시되면 다음 단계에 따라 문제를 해결하십시오.
와이어 데이터 입력 확인을 클릭합니다 . 와이어 데이터 데이터 입력 페이지가 열립니다.
streamfwd 를 클릭 하여 데이터 입력을 확인합니다.
저장 을 클릭 하여 입력을 확인하십시오.
Splunk_TA_stream 로그 파일을 클릭 합니다. 검색 결과에서 오류를 조사하십시오.
그래도 구성 할 수없는 경우 자세히 알아보기 링크를 Splunk_TA_stream클릭하십시오 . 에 대한 적절한 권한을 설정하는 방법을 보여주는 문서로 이동합니다 .Splunk_TA_stream
간편한 설정 curl command.png

원격 컴퓨터에서 데이터 수집 설정
1. 확인 다른 시스템에서 수집 된 데이터를 .

"HTTP 이벤트 콜렉터 streamfwd 토큰 구성이 사용 가능함"이 표시되면 HTTP 이벤트 콜렉터 엔드 포인트가 데이터를 수신하도록 구성된 것입니다. 2 단계로 진행합니다.
"HTTP 이벤트 수집기 streamfwd 토큰 구성이 비활성화되었습니다 . "가 표시되면 구성보기를 클릭 합니다. HTTP 이벤트 콜렉터 페이지가 열립니다. streamfwd 입력에 대해 사용 을 클릭 하여 streamfwd 데이터 입력에 HTTP 이벤트 콜렉터를 사용하십시오 .
2. 설치하려는 Linux 시스템의 명령 줄에서 제공된 curl 스크립트를 복사하고 실행합니다 streamfwd.

스크립트는 Stream Forwarder streamfwd를 /opt/streamfwd.

3. 사용 sudo service streamfwd start | stop | restart | status서비스를 제어하는 명령을 사용합니다.

예를 들면 :

sudo 서비스 streamfwd 시작
참고 : 독립 Stream Forwarder 설치는 필요하지 않습니다. splunk_app_streamUI 의 Distributed Forwarder Management 페이지에서 언제든지 Independent Stream Forwarder를 배포 할 수 있습니다 .

구성 전달자 스트림에 대한 자세한 내용은 다음을 참조하십시오 구성 스트림 전달자 이 설명서입니다.


분산 배포에서 Splunk Stream 마이그레이션
Verison 7.3부터 Splunk Stream은 세 가지 구성 요소로 패키징됩니다. 마이그레이션 후 구성 요소 관리 및 업그레이드가 더 쉬워지고 클러스터 환경을위한 Splunk 관리 도구를 사용하여 더 쉽게 작업 할 수 있습니다.

상품명	설치 패키지 이름	설치된 파일 이름
Stream 용 Splunk 앱	splunk_app_stream	splunk_app_stream/
Stream Forwarder 용 Splunk 애드온	Splunk_TA_stream	Splunk_TA_stream/
스트림 와이어 데이터 용 Splunk 애드온	Splunk_TA_stream_wire_data	Splunk_TA_stream_wire_data/
Independent Stream Forwarder는 <streamfwd>Splunk App for Stream 패키지에 바이너리 파일로 패키징됩니다 .

Splunk Stream 구성 요소에 대한 자세한 내용 은이 설명서의 Splunk Stream 설치 패키지 개요 를 참조하십시오 .

Stream 용 Splunk 앱 업그레이드 및 Stream Wire 데이터 용 Splunk 애드온 설치
Splunk App for Stream 및 Splunk Add-on for Stream Forwarders가 포함 된 이전 버전의 Splunk Stream에서 마이그레이션합니다.

검색 헤드 클러스터 및 인덱서 클러스터에 앱 및 추가 기능을 배포하는 방법에 대한 자세한 내용은 Splunk Enterprise 관리자 매뉴얼 의 앱 배포 개요 를 참조하십시오 .

이 작업을 위해 파일을 다운로드하려면 :

http://splunkbase.com/app/5234 에서 Stream Wire Data 용 Splunk 추가 기능을 다운로드 하십시오 .
http://splunkbase.splunk.com/app/1809 에서 Stream 용 Splunk 앱을 다운로드합니다 .
인덱서 또는 검색 헤드에서 데이터 캡처 모드로 실행중인 경우 Stream Forwarder 용 Splunk 추가 기능 ( Splunk_TA_stream) 을 비활성화하십시오 . 인 Splunk 부가 기능 스트림 포워더로 설정하여이 작업을 수행 disabled = 1에서 app.conf.
(선택 사항) 배포자를 사용하는 경우 Stream Forwarder 용 Splunk 애드온 ( Splunk_TA_stream) 및 Stream 용 Splunk 앱 ( ) 의 기존 버전을 백업하십시오 splunk_app_stream.
Splunk_TA_stream_wire_data검색 헤드 및 인덱서 에 Splunk Add-on for Stream Wire Data ( )를 설치합니다.
Splunk_TA_stream/local/이전 설치 에서 다음 파일을 보관 한 경우 Splunk_TA_stream_wire_data/local/추가 기능을 클러스터에 푸시 하기 전에 2 단계에서 만든 백업을 사용하여 이동 합니다.
distsearch.conf
tags.conf
props.conf
transforms.conf
eventtypes.conf
indexes.conf (인덱서 패키지에만 해당)
당신이 네 단계에있는 파일을 이동 한 경우 (선택 사항) Splunk_TA_stream_wire_data/local/에서 삭제를 Splunk_TA_stream. 이렇게하면 설치가 깨끗하게 유지되고 향후 릴리스 변경과의 잠재적 충돌을 방지 할 수 있습니다.
(선택 사항) 검색 헤드 및 인덱서에서 네트워크 데이터를 계속 수집하려면 검색 헤드 및 인덱서에서 Splunk Add-on for Stream Forwarder ( Splunk_TA_stream)를 업그레이드 할 수 있습니다 .
splunk_app_stream검색 헤드 에서 Splunk App for Stream ( )을 업그레이드합니다 . 설치 후 Splunk App for Stream을 비활성화하거나 삭제하지 마십시오.이 파일은 모든 포워더 구성을 유지합니다.
인 Splunk 부가 기능 스트림 포워더 (대한 사용 Splunk_TA_stream)을 설정하여 app.conf에 파일을 enabled = 0.
검색 헤드와 인덱서를 다시 시작하십시오.
대시 보드를 확인하여 데이터가 예상대로 흐르는 지 확인하십시오.
Stream Forwarder 용 Splunk 추가 기능 업그레이드
모든 포워더에서 쉽고 일관된 구현을 위해 배포 서버를 사용합니다. Stream 배포에 배포 서버에없는 추가 포워더가 포함되어 있거나 배포 서버를 사용하지 않는 경우 Splunk_TA_stream각 포워더에서 Splunk Add-on for Stream Forwarder ( )를 수동으로 업그레이드해야합니다 .

검색 헤드 클러스터 및 인덱서 클러스터에 앱 및 추가 기능을 배포하는 방법에 대한 자세한 내용은 Splunk Enterprise 관리자 매뉴얼 의 앱 배포 개요 를 참조하십시오 .

http://splunkbase.com/app/5238 에서 Stream Forwarders 용 Splunk 추가 기능을 다운로드 하십시오 .

전달자를 위해 배포 서버에 ssh를 추가합니다.
기존 버전의 Splunk_TA_stream.
Splunk_TA_stream이전 버전 에서 Stream Forwarder 용 Splunk 추가 기능 ( ) 의 최신 버전을 추출하십시오 .
(선택 사항)이 항목의 "Stream 용 Splunk 앱 업그레이드 및 Stream Wire 데이터 용 Splunk 애드온 설치"의 4 단계에 나열된 모든 파일을 제거합니다.
배포 서버를 다시로드하여 Stream Forwarder 용 Splunk Add-on의 새 버전을 포워더 Splunk_TA_stream로 푸시합니다 .
Windows의 경우 Splunk Stream은 WinPcap 드라이버를 사용하여 Windows 시스템에서 패킷을 캡처합니다. WinPcap 보안 모델의 결함으로 인해 Windows에 Stream을 설치하면 모든 로컬 사용자가 패킷 스니핑에 WinPcap을 사용할 수 있습니다. https://wiki.wireshark.org/CaptureSetup/CapturePrivileges를 참조하십시오. Windows 시스템에서 Splunk Stream은 관리자 역할 만 지원합니다.

