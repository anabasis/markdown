배포 요구 사항
Splunk Stream을 설치하기 전에 시스템이 Splunk Stream 하드웨어 및 소프트웨어 요구 사항을 충족하는지 확인하십시오.

네트워크 프로토콜
Splunk Stream이 지원하는 네트워크 프로토콜 목록은 이 설명서의 지원되는 프로토콜 을 참조하십시오 .

하드웨어 요구 사항
Splunk Stream을 설치하기 전에 기본 Splunk Enterprise 배포가 Splunk Enterprise 용량 계획 매뉴얼 의 Splunk Enterprise 용량 계획 소개에 지정된 요구 사항을 충족하는지 확인하십시오 .

기본 Splunk Enterprise 하드웨어 요구 사항 은 Splunk Enterprise 용량 계획 매뉴얼 의 참조 하드웨어 를 참조 하십시오 . 캡처 및 인덱싱하려는 네트워크 데이터의 양에 따라 추가 리소스가 필요할 수 있습니다.

Splunk Stream 성능에 대한 자세한 내용 은이 설명서의 성능 테스트 결과 및 권장 사항 을 참조하십시오 .

지원되는 운영 체제
Splunk Stream 7.1.2 이상은 다음 운영 체제를 지원합니다.

리눅스
Linux 커널 버전 3.10 이상 (64 비트).
Red Hat Enterprise Linux 7.0 이상
CentOS 7.0 이상
Ubuntu 16.04 이상
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
Splunk Stream은 Windows에서만 로컬 시스템 및 관리자 계정을 지원합니다. 자세한 내용 은 Windows에서 시스템 계정을 사용하는 방법을 참조하십시오 .

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

NetFlow 요구 사항
NefFlow 데이터 수집에는 Splunk Stream 버전 7.0.0 이상이 필요합니다.
NetFlow 애플리케이션 ID 필드 디코딩에는 Splunk Stream 버전 7.2.0 이상이 필요합니다.
NetFlow 레코드 흐름 타임 스탬프를 기반으로하는 NetFlow 이벤트 타임 스탬프에는 Splunk Stream 버전 7.2.0 이상이 필요합니다.
NetFlow에 대한 자세한 내용은 Splunk Stream을 사용하여 Netflow 및 IPFIX 데이터 수집을 참조 하십시오 .


단일 인스턴스 배포에 Splunk Stream 설치
Splunk의 단일 인스턴스 배포에서 동일한 단일 Splunk Enterprise 인스턴스가 검색 헤드와 인덱서 역할을 모두 수행합니다. 모든 패키지는 단일 인스턴스에 설치됩니다.

전체 Splunk Stream 기능을 사용하려면 Splunk Enterprise 인스턴스에 세 가지 패키지를 다운로드하여 설치합니다.

Splunk App for Stream, 패키지 splunk_app_stream
Stream Forwarder 용 Splunk 애드온, Splunk_TA_stream
Stream Wire Data 용 Splunk 애드온, 패키지 Splunk_TA_stream_wire_data
Splunk Stream은 또한 <streamfwd>Splunk App for Stream 패키지에 바이너리 파일로 패키징 된 Independent Stream Forwarder를 제공 합니다.

Splunk Stream 구성 요소에 대한 자세한 내용 은이 설명서의 Splunk Stream 설치 패키지 개요 를 참조하십시오 .

Stream 용 Splunk 앱 설치
스트림 설치를위한 인 Splunk 앱을 배포하는 방법 splunk_app_stream에 $SPLUNK_HOME/etc/apps당신 인 Splunk 기업 인스턴스.

http://splunkbase.splunk.com/app/1809로 이동합니다 .
다운로드를 클릭하십시오. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
앱 관리 > 파일에서 앱 설치를 클릭 합니다 .
설치 프로그램 파일을 업로드하십시오.
메시지가 나타나면 Splunk Enterprise를 다시 시작하십시오.
Stream Wire Data 용 Splunk 추가 기능 설치
인 Splunk를 배포하려면 부가 기능 스트림 와이어 데이터 설치에 대한 Splunk_app_stream에 $SPLUNK_HOME/etc/apps당신 인 Splunk 기업 인스턴스.

http://splunkbase.com/app/5234로 이동합니다 .
다운로드를 클릭하십시오. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
앱 관리 > 파일에서 앱 설치를 클릭 합니다 .
설치 프로그램 파일을 업로드하십시오.
메시지가 나타나면 Splunk Enterprise를 다시 시작하십시오.
단일 인스턴스에 Stream Forwarder 용 Splunk 추가 기능 설치
, 스트림 전달자 인 Splunk 기업의 당신의 단일 인스턴스를 구성 설치하려면 Splunk_TA_stream에서 $SPLUNK_HOME/etc/apps당신 인 Splunk 기업 인스턴스.

http://splunkbase.com/app/5238로 이동합니다 .
다운로드를 클릭 합니다. 설치 패키지가 로컬 호스트로 다운로드됩니다.
Splunk Web에 로그인합니다.
앱 관리 > 파일에서 앱 설치를 클릭 합니다 .
설치 프로그램 파일을 업로드하십시오.
메시지가 나타나면 Splunk Enterprise를 다시 시작하십시오.
간편한 설정 스트리밍
Splunk Stream은 Splunk Enterprise의 단일 인스턴스에 배포하기 위해 로컬 컴퓨터에서 데이터 수집을 설정하고 구성하는 데 도움이되는 쉬운 설정 페이지를 제공합니다.

로컬 컴퓨터에서 데이터 수집 설정
유선 데이터 입력을 사용하여이 시스템에서 데이터 수집 확인란을 선택합니다 .
당신이 메시지가 표시되면 "Splunk_TA_stream가 제대로 구성되어 있지 않습니다"를 클릭 재검색을 . 대부분의 경우 이는 streamfwd바이너리가 네트워크 인터페이스에서 패킷을 캡처 할 수있는 적절한 권한을 설정합니다 .
Splunk가 바이너리를 감지 할 수없고 "Splunk_TA_stream이 올바르게 구성되지 않았습니다"라는 메시지가 계속 표시되는 경우 다음을 시도하십시오.
와이어 데이터 입력 확인을 클릭합니다 . 와이어 데이터 데이터 입력 페이지가 열립니다.
streamfwd 를 클릭 하여 데이터 입력을 확인합니다.
저장 을 클릭 하여 입력을 확인하십시오.
Splunk_TA_stream 로그 파일을 클릭 합니다. 검색 결과에서 오류를 조사하십시오.
그래도 구성 할 수없는 경우 자세히 알아보기 링크를 Splunk_TA_stream클릭하십시오 . 에 대한 적절한 권한을 설정하는 방법을 보여주는 문서로 이동합니다 .Splunk_TA_stream
간편한 설정 curl command.png

Splunk 단일 인스턴스 배포에서 Splunk Stream 마이그레이션
Verison 7.3부터 Splunk Stream은 세 가지 구성 요소로 패키징됩니다. 마이그레이션 후 구성 요소 관리 및 업그레이드가 더 쉬워지고 클러스터 환경을위한 Splunk 관리 도구를 사용하여 더 쉽게 작업 할 수 있습니다.

상품명	설치 패키지 이름	설치된 파일 이름
Stream 용 Splunk 앱	splunk_app_stream	splunk_app_stream/
Stream Forwarder 용 Splunk 애드온	Splunk_TA_stream	Splunk_TA_stream/
스트림 와이어 데이터 용 Splunk 애드온	Splunk_TA_stream_wire_data	Splunk_TA_stream_wire_data/
Independent Stream Forwarder는 <streamfwd>Splunk App for Stream 패키지에 바이너리 파일로 패키징됩니다 .

Splunk Stream 구성 요소에 대한 자세한 내용 은이 설명서의 Splunk Stream 설치 패키지 개요 를 참조하십시오 .

업그레이드
Splunk Stream 7.3으로 업그레이드하려면 Splunk App for Stream ( splunk_app_stream) 및 Splunk Add-on for Stream Forwarder ( Splunk_TA_ stream) 를 업그레이드 하고 Splunk Add-on for Stream Wire Data ( Splunk_TA_stream_wire_data)를 설치하십시오.

나중에 필요할 경우를 대비하여 기존 구성을 별도의 서버 또는 디렉토리에 백업하는 것이 가장 좋습니다.

Stream 용 Splunk 앱 및 Stream Wire 데이터 용 Splunk 애드온 업그레이드
이 작업을 위해 파일을 다운로드하려면 :

http://splunkbase.com/app/5234Splunk_TA_stream_wire_data 에서 Stream Wire Data ( ) 용 Splunk 추가 기능을 다운로드 하십시오.
http://splunkbase.splunk.com/app/1809splunk_app_stream 에서 Splunk App for Stream ( )을 다운로드합니다 .
Splunk_TA_stream데이터 캡처 모드에서 Stream Forwarders 용 Splunk 애드온 ( )을 실행중인 경우 ) 파일 disabled = 1에서 Stream Forwarders 용 Splunk 애드온을로 설정하여 비활성화 하십시오.Splunk_TA_streamapp.conf
(선택 사항) Stream Forwarder 용 Splunk 애드온 ( Splunk_TA_stream) 및 Stream 용 Splunk 앱 ( ) 의 기존 버전을 splunk_app_stream별도의 디렉터리에 백업합니다 .
Splunk_TA_stream_wire_dataSplunk Enterprise 인스턴스에 Stream Wire Data 용 Splunk 애드온 ( )을 설치합니다.
2 단계에서 만든 백업을 사용하여 다음 파일을 Splunk_TA_stream_wire_data/local/.
distsearch.conf
tags.conf
props.conf
transforms.conf
eventtypes.conf
indexes.conf (인덱서 패키지에만 해당)
(선택 사항) 4 단계에서 파일을로 이동했으면 Splunk_TA_stream_wire_data/local/에서 삭제합니다 Splunk_TA_stream. 이렇게하면 설치가 깨끗하게 유지되고 향후 릴리스 변경과의 잠재적 충돌을 방지 할 수 있습니다.
splunk_app_streamSplunk Enterprise 인스턴스 에서 Splunk App for Stream ( )을 업그레이드합니다 . 설치 후 Splunk App for Stream을 비활성화하거나 삭제하지 마십시오.이 파일은 포워더 설치를위한 구성을 유지합니다.
파일을 다음과 같이 Splunk_TA_stream설정하여 Splunk Enterprise 인스턴스에서 Stream Forwarders ( ) 용 Splunk 애드온을 활성화합니다.app.confenabled = 0
Splunk_TA_streamSplunk Enterprise 인스턴스에서 Stream Forwarders 용 Splunk 애드온 ( )을 업그레이드하십시오 .
Splunk Enterprise 인스턴스를 다시 시작합니다.
모든 데이터가 대시 보드에서 예상대로 흐르는 지 확인합니다.
Stream Forwarder 용 Splunk 추가 기능 업그레이드
http://splunkbase.com/app/5238Splunk_TA_stream 에서 Stream Forwarders 용 Splunk 추가 기능 ( )을 다운로드 하십시오 .

기존 버전의 Splunk_TA_stream.
Splunk_TA_stream이전 버전 에서 Stream Forwarder 용 Splunk 추가 기능 ( ) 의 최신 버전을 추출하십시오 .
(선택 사항)이 항목의 "Stream 용 Splunk 앱 업그레이드 및 Stream Wire 데이터 용 Splunk 애드온 설치"의 4 단계에 나열된 모든 파일을 제거합니다.
Splunk Enterprise 인스턴스를 다시 시작합니다.
Splunk Stream은 WinPcap 드라이버를 사용하여 Windows 시스템에서 패킷을 캡처합니다. WinPcap 보안 모델의 결함으로 인해 Windows에 Stream을 설치하면 모든 로컬 사용자가 패킷 스니핑에 WinPcap을 사용할 수 있습니다. https://wiki.wireshark.org/CaptureSetup/CapturePrivileges를 참조하십시오. Windows 시스템에서 Splunk Stream은 관리자 역할 만 지원합니다.

