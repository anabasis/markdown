지원되는 프로토콜
Splunk Stream Forwarder는 심층 패킷 검사를 활용하여 유선에서 수집 된 패킷 데이터의 프로토콜 속성을 해석합니다.

지원되는 각 프로토콜에는 다양한 속성이 있습니다. 일반적인 속성은 protocol_stack디코딩되는 프로토콜에 적용되는 네트워크 계층 목록 인입니다.

Splunk Stream Forwarder는 네트워크 계층 인 세 번째 계층에서 프로토콜 해석을 시작합니다. 그러면 Splunk Stream Forwarder는 애플리케이션 계층 인 계층 7까지 프로토콜 계층을 해석 할 수 있습니다.

Stream이 프로토콜에 대한 모든 네트워크 계층을 해석 할 수없는 경우가 있습니다. 이 경우 protocol_stack필드에는 디코딩 할 수있는 레이어 만 포함됩니다. 빈 protocol_stack필드는 지원되지 않는 프로토콜 또는 잘못된 데이터가있는 프로토콜을 나타냅니다.

protocol_stackSplunk 네트워크 트래픽 의 예 는 다음과 같습니다.

IP:TCP:SSL:SPLUNK

mysql의 예 protocol_stack는 다음과 같습니다.

IP:TCP:MYSQL

필드 추출에 지원되는 프로토콜
프로토콜 필드 추출과 같은 특정 이벤트 유형에 대한 프로토콜 데이터를 파싱하여 bytes_in, bytes_out, status, src_ip, 및 time_taken. 필드 추출에 지원되는 모든 프로토콜을 스트림 구성 UI의 모든 스트림 구성에 추가 할 수 있습니다.

Stream은 필드 추출을 위해 다음 프로토콜을 지원합니다.

실험 계획안	기술
AMQP	고급 메시징 큐 프로토콜
DHCP	동적 호스트 구성 프로토콜
직경	
DNS	도메인 이름 서비스
FTP	파일 전송 프로토콜
HTTP	하이퍼 텍스트 전송 프로토콜
ICMP	인터넷 제어 메시지 프로토콜
IMAP	인터넷 메시지 액세스 프로토콜
IP	인터넷 프로토콜
IRC	인터넷 릴레이 채팅
LDAP	경량 디렉토리 액세스 프로토콜
MAPI	메시징 애플리케이션 프로그래밍 인터페이스
MySQL	MySQL 클라이언트 / 서버 프로토콜
NetBIOS	네트워크 기본 입 / 출력 시스템
NFS	네트워크 파일 시스템
POP3	우체국 프로토콜 v3
Postgres	PostgreSQL
반지름	원격 인증 다이얼 인 사용자 서비스
RTCP	실시간 전송 제어 프로토콜. RTP 프로토콜 (실시간 전송 프로토콜)과 함께 사용됩니다.
RTP	실시간 전송 프로토콜.
한모금	세션 시작 프로토콜
SMB	서버 메시지 블록
SMPP	짧은 메시지 피어 투 피어
SMTP	단순 메일 전송 프로토콜
SNMP	단순 네트워크 관리 프로토콜
TCP	전송 제어 프로토콜
TDS 테이블 형식 데이터 스트림-Sybase / MSSQL	테이블 형식 데이터 스트림-Sybase / MSSQL
TNS	투명한 네트워크 기판 (Oracle)
UDP	사용자 데이터 그램 프로토콜
XMPP	확장 가능한 메시징 및 프레즌스 프로토콜
탐지 용으로 만 지원되는 프로토콜
프로토콜 감지는 전송 계층에서만 프로토콜 분류를 의미합니다. 예를 들어, Tor 이벤트 유형은없고 app=torTCP 이벤트 의 필드 만 애플리케이션 계층에서 Tor 프로토콜을 나타냅니다.

감지에만 사용할 수있는 프로토콜은 스트림 구성 UI에서 선택할 수 없으며 스트림 구성에 추가 할 수 없습니다. 이러한 프로토콜을 감지하려면 적절한 소스 유형 및 프로토콜 분류를 사용하여 검색을 실행해야합니다.

프로토콜을 감지하는 방법
프로토콜을 감지하려면 tcp 스트림에서 프로토콜 분류를 지정하는 검색을 실행하십시오. 예를 들면 :

sourcetype=stream:tcp app=tor

tcp및 udp스트림 에서 모든 프로토콜 분류를 감지하려면 :

(sourcetype=stream:tcp OR sourcetype=stream:udp) | stats count by app

Stream은 탐지를 위해 다음 프로토콜 만 지원합니다.

실험 계획안	기술
flashplugin_update	Flash는 Adobe 서버와 플러그인 버전 번호를 교환합니다.
adobe_update	Adobe Update Manager는 Adobe Acrobat Reader 소프트웨어의 최신 버전을 유지합니다.
aim_express	AOL Instant Messaging Express는 AIM에 포함 된 많은 표준 기능을 지원하지만 파일 전송, 오디오 채팅 또는 화상 회의와 같은 고급 기능은 제공하지 않습니다.
aim_transfer	AIM은 인스턴트 메시징 프로토콜입니다.
모든 음악	올 뮤직은 온라인 음악 가이드 서비스 웹 사이트입니다. 이 플러그인은 AllMusic 웹 서비스의 탐색 및 MP3 음악 재생을 분류합니다. 비디오 클립 스트리밍은 YouTube에서 처리합니다.
알티리스	Altiris는 IT 인프라 관리를위한 서비스 지향 관리 솔루션을 제공합니다.
amazon_adsystem	이 프로토콜 플러그인은 Amazon 광고 서비스와 관련된 트래픽을 분류합니다.
amazon_cloud_drive	Amazon Cloud Drive는 사진과 동영상을 저장할 수있는 클라우드 애플리케이션입니다.
아마존	이 프로토콜 플러그인은 Amazon 서비스와 관련된 일반 웹 트래픽을 분류합니다.
amazon_mp3	Amazon MP3는 Amazon.com이 소유하고 운영하는 온라인 뮤직 스토어입니다.
amazon_video	Amazon Video는 Amazon.com이 소유하고 운영하는 주문형 온라인 비디오 서비스입니다.
amazon_aws	Amazon AWS는 Amazon에서 제공하는 클라우드 컴퓨팅 플랫폼입니다. 여기에는 Amazon Elastic Compute Cloud (EC2) 및 Amazon Simple Storage Service (S3)가 포함됩니다.
android_cnxmgr	Android 연결 관리자는 Android 기기에서 주기적으로 인터넷 연결을 확인하고 관리하는 데 사용됩니다.
AOL	이 프로토콜 플러그인은 AOL 포털과 관련된 트래픽을 분류합니다.
목표	AIM (원래 AOL Instant Messenger)은 인스턴트 메시징 응용 프로그램입니다. 프로토콜 이름은 OSCAR (Open System for CommunicAtion in Realtime)이며 ICQ 및 AIM 서비스 모두에서 사용됩니다. [aim은 oscar라고도합니다.] 참고 : Basic-DPI에서는 http에 대한 부분 분류입니다.
apple_airplay	Apple airplay는 동일한 사설 네트워크에 연결된 장치에서 연결된 TV로 사진과 비디오를 표시하는 프로토콜입니다.
apple_airport	Apple Airport는 무선 장치를 구성하는 데 도움이되는 프로토콜입니다.
apple_airprint	Apple Airprint는 Apple 시스템의 네트워크 인쇄 기능입니다. Dns Service Discovery 프로토콜 및 IPP (URF 형식 지원 필요)를 기반으로합니다. 참고 : Basic-DPI에서는 http / ipp를 통한 부분 분류입니다.
앱 스토어	Apple App Store는 Apple Inc.에서 개발 및 유지 관리하는 iOS 용 디지털 애플리케이션 배포 플랫폼입니다.
화상 통화	FaceTime은 iOS 기반 모바일 장치에서 실행되는 Apple 화상 통화 소프트웨어입니다. 참고 : Basic-DPI에서 SIP 오디오 통화 세션의 부분 분류.
사과	이 프로토콜 플러그인은 Apple의 웹 포털 및 콘텐츠 전달 서비스와 관련된 일반 트래픽을 분류합니다.
apple_hls	HTTP Live Streaming IETF 초안의 Apple 구현. Apple iOS 기기에서 사용됩니다.
apple_location	Apple 위치는 Apple 장치의 위치에 대한 정보를 제공하는 데 사용됩니다.
apple_maps	Apple Maps는 iOS 6 기기를위한 독점지도 애플리케이션입니다.
애플 뮤직	Apple Music은 Apple의 주문형 음악 스트리밍 서비스입니다.
apns	Apple 푸시 알림 서비스는 타사 애플리케이션 서버에서 iOS 장치로 알림을 전달하는 Apple 서비스입니다.
apple_siri	일부 Apple iPhone 장치에서 사용되는 고급 음성 인식 시스템.
apple_update	Apple_update는 Apple 소프트웨어 업데이트에 사용되는 프로토콜입니다.
asproxy	ASProxy는 사용자가 익명으로 인터넷을 서핑 할 수있는 무료 오픈 소스 웹 프록시입니다. 이 플러그인은 웹 브라우징을위한이 프록시의 사용을 다른 인식 된 애플리케이션 / 프로토콜에 대한 대체로 분류합니다.
아 틀라 시안	Atlassian은 소프트웨어 개발자 및 프로젝트 관리자를 대상으로하는 제품을 개발하는 호주 엔터프라이즈 소프트웨어 회사입니다.
비트	BITS (Background Intelligent Transfer Service)는 클라이언트와 서버간에 파일 (다운로드 또는 업로드)을 전송하고 전송과 관련된 진행 정보를 제공합니다.
바이두 플레이어	BaiduPlayer는 로컬, 온라인 및 주문형 비디오를 재생할 수있는 비디오 플레이어입니다.
baidu_wallet	Baidu 지갑은 돈 관리 응용 프로그램입니다.
바이두	Baidu는 웹 사이트, 오디오 파일 및 이미지에 대한 중국어 검색 엔진입니다. 참고 : Basic-DPI에서 이미지 및 비디오 검색의 부분 분류.
bet365	온라인 베팅 사이트 ( http://www.bet365.com )
비트 코인	비트 코인은 분산 결제 시스템입니다.
비트 토런트	BitTorrent는 P2P 프로토콜입니다. [bittorrent는 kadmelia라고도합니다.] 참고 : Basic-DPI에서 암호화를 사용하는 특정 파일 다운로드 세션에 대한 부분 분류.
bittorrent_application	BitTorrent 애플리케이션에서 BitTorrent 앱 웹 액세스.
호출 장치	Bleep은 BitTorrent 팀에서 만든 완전히 암호화되고 분산 된 인스턴트 메시징 프로토콜입니다. 이 프로토콜 플러그인은 텍스트 및 음성 토론을 모두 지원합니다.
blackberry_locate	이 프로토콜은 Wi-Fi를 통한 현지화에 대한 모든 Blackberry 모바일 장치 통신을 나타냅니다.
bbm	BBM은 블랙 베리 용 메신저 / voip / 비디오 프로토콜입니다. 이 플러그인은 BlackBerry Messenger의 오디오 및 비디오 데이터 흐름을 분류합니다.
bbm_audio	bbm_audio는 블랙 베리 메신저의 VoIP 레이어입니다. 참고 : Basic-DPI에서는 stun / bbm에 대한 부분 분류입니다.
bbm_video	BBM_video는 블랙 베리 메신저의 비디오 레이어입니다. 참고 : Basic-DPI에서는 기절에 대한 부분 분류입니다.
블랙 베리	이 프로토콜은 Wi-Fi를 통한 모든 Blackberry 모바일 장치 통신을 의미합니다. 여기에는 BlackBerry Messenger의 채팅 흐름이 포함됩니다.
bgp	BGP (Border Gateway Protocol)는 대부분의 ISP에서 사용하는 자율 시스템 간 라우팅 프로토콜입니다.
카보나이트	Carbonite는 온라인 백업을 관리하는 서비스입니다.
ccproxy	CCProxy는 Windows 기반 소프트웨어 프록시입니다.
chat_on	챗온은 삼성 전자가 도입 한 글로벌 이동 통신 서비스입니다.
챗 룰렛	Chatroulette는 온라인 채팅 웹 사이트입니다. ( http://chatroulette.com )
chrome_update	Chrome 업데이트는 Google 크롬 브라우저의 업데이트를위한 프로토콜입니다.
cdp	CDP (Cisco Discovery Protocol)는 링크에있는 다른 Cisco 네트워크 장비를 검색하기 위해 Cisco 네트워크 장비에서 사용하는 레이어 2 프로토콜입니다.
모임 장소	MeetingPlace는 음성, 웹 및 화상 회의 제품의 Cisco Unified MeetingPlace 제품군에서 사용하는 프로토콜입니다.
넷 플로우	NetFlow는 소스 / 대상 IP 주소, 프로토콜 등을 사용하여 거의 실시간 트래픽 모니터링, 집계 및 통계 평가, 다중 기준 데이터 흐름 선택을 제공하는 Cisco 프로토콜입니다.
컵	CUPS (Common Unix Printer System) 프로토콜은 UNIX 환경을위한 크로스 플랫폼 인쇄 솔루션입니다. "인터넷 인쇄 프로토콜"을 기반으로하며 Microsoft 운영 체제 Windows 2000 이상과 호환됩니다.
딱딱 거리다	crackle은 무료 영화, TV 쇼 및 오리지널 프로그램을 배포하는 엔터테인먼트 네트워크 및 스튜디오입니다.
크레이그리스트	미국과 캐나다에서 주로 사용되는 온라인 분류 광고
dsi	DSI (Data Stream Interface)는 TCP (Transmission Control Protocol)를 통해 Apple Filing Protocol 트래픽을 전달하는 세션 계층입니다.
db2	DB2는 IBM의 관계형 모델 데이터베이스 서버입니다. IBM 메인 프레임에서 실행되며 Linux / Unix / Windows에서도 사용할 수 있습니다.
debian_update	Debian / Ubuntu 패킷 관리자 인 APT의 업데이트 프로토콜입니다.
드롭 박스	Dropbox는 웹 및 스마트 애플리케이션 인터페이스를 모두 제공하는 무료 서비스입니다.
dropbox_download	Dropbox의 파일 다운로드 서비스입니다.
dropbox_upload	Dropbox의 파일 업로드 서비스입니다.
이베이	온라인 경매 및 쇼핑 웹 사이트.
에 돈키	Edonkey는 P2P 프로토콜입니다. 프로토콜 난독 화 기능이 활성화 된 경우 분류가 보장되지 않습니다 (이 기능은 eMule 버전 0.47b에 나타남). [edonkey는 kadmelia 및 emule이라고도합니다.]
Evernote	웹 기반 메모 도구.
Everquest	Everquest는 Windows 플랫폼 용 3D 멀티 플레이어 온라인 롤 플레잉 게임 (MMORPG)입니다.
페이스 북	Facebook은 소셜 네트워크입니다.
facebook_messenger	Facebook Messenger는 모바일 장치 용 문자 및 음성 메시징 응용 프로그램입니다.
Farmville	FarmVille은 Zynga에서 개발 한 농업 시뮬레이션 소셜 네트워크 게임입니다.
find_my_iphone	분실 한 iOS 기기를 찾기 위해 Apple에서 개발 한 애플리케이션입니다.
firefox_update	브라우저 및 플러그인을위한 Mozilla Firefox 업데이트 프로토콜. 이는 브라우저에서 작성된 업데이트에만 적용됩니다. 수동으로 다운로드 한 업데이트에는 적용되지 않습니다.
플리커	소셜 및 블로깅 서비스와 이미지 호스팅 및 공유 웹 사이트.
gre	GRE (Generic Routing Encapsulation Protocol)는 일반적으로 한 프로토콜을 다른 프로토콜로 캡슐화하는 데 사용됩니다.
github	오픈 소스 소프트웨어 개발을위한 웹 기반 코드 저장소.
gmail_basic	Gmail 기본은 Google 웹 메일 서비스의 HTML 버전입니다. 암호화 된 트래픽은 Gmail로 분류됩니다.
gmail_drive	GMAIL Drive는 Gmail을 저장 매체로 사용할 수 있도록 Google Mail 계정 주위에 가상 파일 시스템을 만드는 Shell Namespace Extension입니다. GMAIL 드라이브는 https가 아닌 http로만 분류됩니다.
gmail_mobile	휴대 전화 용 Google 웹 메일. 이 프로토콜은 암호화되지 않은 버전 만 디코딩합니다.
그누 넷	주로 익명 파일 공유에 사용되는 안전한 피어-투-피어 네트워킹을위한 프레임 워크입니다. GNU 프로젝트의 일부입니다.
그누텔라	Gnutella는 P2P 프로토콜입니다. [gnutella는 kadmelia라고도합니다.] 참고 : Basic-DPI에서 Android에서 파일 다운로드 중 부분 분류.
google_accounts	Google 계정 서버에 대한 SSL 액세스를 감지합니다.
google_analytics	Google Analytics는 웹 사이트 트래픽 및 마케팅 효과에 대한 풍부한 통찰력을 제공하는 엔터프라이즈 급 웹 분석 솔루션입니다.
google_appengine	Google App Engine은 Google 관리 데이터 센터에서 웹 애플리케이션을 개발하고 호스팅하기위한 PaaS (Platform as a Service) 클라우드 컴퓨팅 플랫폼입니다.
google_cache	Google 캐시는 Google 검색 엔진에서 찾은 웹 페이지의 사본을 저장합니다.
google_calendar	Google 캘린더는 무료 온라인 캘린더입니다.
gmail_chat	Google 채팅은 온라인 메시징 도구입니다.
gcm	타사 서버 애플리케이션과 Android 클라이언트 애플리케이션 간의 데이터 교환 서비스입니다. 이 플러그인은 CCS 타사 서버와 GCM 클라우드 서버간에 교환되는 메시지와 GCM 클라우드 서버와 클라이언트 Android 장치간에 교환 된 메시지를 분류합니다.
gcs	Google의 애플리케이션을위한 온라인 파일 저장소 웹 서비스입니다. 이 플러그인은 보안되지 않은 클라이언트 -Google 서버 웹 통신 만 분류합니다.
구글 문서	온라인 파일 저장 및 공유 웹 서비스. 대부분의 트래픽은 일반 Google 인증서로 암호화되며 분류 할 수 없습니다. 프록시 및 일부 제한된 워크 플로의 트래픽에 대해 분류가 정확합니다. [google_docs는 google_drive라고도합니다.]
구글 어스	Google Earth는 가상 지구를 3D로 보는 데 사용되는 프로그램입니다.
google_gen	이 프로토콜은 모든 Google 프로토콜의 기반으로 사용되는 일반 레이어입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
google_groups	Google 그룹
gstatic	GStatic은 정적 리소스 (예 : CSS) 또는 Google 웹 애플리케이션 용 스크립트를 제공하는 다운로드 서버입니다.
gtalk	Google 행 아웃은 데스크톱 및 휴대 기기에서 사용할 수있는 인스턴트 메시징 서비스입니다. 이전 Google Talk 버전은 XMPP를 사용하며 문자 및 음성 통신을 모두 제공합니다. 이 플러그인은 또한 DNS 캐싱을 사용하여 Google 행 아웃의 RTP 오디오 / 비디오 스트림을 분류합니다. [gtalk는 google_hangouts라고도합니다.]
Gmail	Gmail은 Google 웹 메일 서비스입니다. Basic-DPI에서는 gmail때때로 gmail_chat.
구글지도	사용자가 경로를 계산하고지도를 볼 수있는 웹 서비스입니다. 암호화 된 트래픽은로 분류됩니다 google.
google_picasa	Google Picasa는 웹에서 사진이나 동영상을 편집하고 동기화하는 데 사용되는 디지털 사진 및 동영상 정리 도구입니다.
google_play_music	Google Play 뮤직은 음악 스트리밍 서비스이자 온라인 음악 보관함입니다.
구글 플레이	Google Play 스토어 (이전의 Android Market)는 Google에서 Android OS 기기 용으로 개발 한 온라인 소프트웨어 스토어입니다.
구글 플러스	Google Plus는 소셜 네트워크입니다. 외부 링크에서 공유 할 때 분류됩니다. 기타 트래픽은 google또는 로 분류됩니다 google_cache.
google_safebrowsing	Google 세이프 브라우징은 웹 페이지를 위협으로부터 확인하기위한 웹 서비스 및 API입니다. 이 서명은 Google Safebrowse 제출을 감지합니다.
google_tags	Google 태그 관리자는 웹 사이트 및 모바일 애플리케이션을위한 태그 관리자입니다.
구글 툴바	Google 툴바는 검색 창, 팝업 차단기 및 번역기를 포함하는 기능을 제공하는 Internet Explorer 및 Mozilla Firefox 용 확장 프로그램입니다.
구글 번역	Google 번역은 Google 번역 도구입니다.
구글	이 프로토콜은 사용자 쿼리를 Google 검색 엔진에 보내는 데 사용됩니다.
gotodevice	GoToDevice는 원격 제어 및 관리 도구입니다.
미팅에 가다	GoToMeeting은 Citrix에서 개발 한 온라인 회의 서비스입니다. Basic-DPI에서 https를 통한 부분 분류.
gotomypc	Citrix GoToMyPC는 사용자가 웹 브라우저에서 PC / MAC를 제어 할 수있는 안전한 웹 기반 원격 액세스 솔루션입니다.
gtp	GPRS 터널링 프로토콜 (GTP)은 이동 통신사 네트워크의 SGSN과 GGSN 사이에 터널을 생성하여 이동국 데이터를 전송하는 데 사용됩니다.
gtpv2	GPRS 터널링 프로토콜 (GTP) 버전 2는 G4 모바일 네트워크 (LTE)에서 사용됩니다. MME, SGW 및 PGW간에 제어 메시지를 교환합니다.
반감기	Half-Life와 Half-Life 2는 Valve Corporation에서 개발 한 1 인칭 슈팅 게임입니다.
hi5	Hi5는 소셜 네트워킹 웹 사이트입니다.
high_entropy	높은 엔트로피는 tcp 및 udp를 통해 알 수없는 세션에 대해 잠재적으로 암호화 된 페이로드를 감지하는 가상 프로토콜입니다. 이 계층의 분류는 ixEngine 프레임 워크의 4.18.0 버전부터 유효합니다. 분류는 엔트로피 값 계산 및 인쇄 가능한 문자열 감지의 두 가지 방법을 기반으로합니다.
hsrp	Cisco HSRP (Hot Standby Router Protocol)를 사용하면 네트워크에서 라우터 이중화를 관리 할 수 ​​있습니다.
제트 다이렉트	Jetdirect 프로토콜은 HP 네트워크 프린터에서 사용됩니다.
훌루	Hulu는 무료 주문형 비디오 및 비디오 공유 서비스입니다.
http2	HTTP / 2는 World Wide Web에서 사용하는 HTTP 네트워크 프로토콜의 두 번째 주요 버전입니다.
i2p	I2P (Invisible Internet Project)는 네트워크 내의 네트워크 인 익명의 오버레이 네트워크입니다. 이는 ISP와 같은 제 3 자에 의한 드래그 넷 감시 및 모니터링으로부터 통신을 보호하기위한 것입니다.
인포믹스	Informix는 관계형 데이터베이스 관리 시스템 제품군입니다. IBM 메인 프레임에서 실행되며 Linux / Unix / Windows에서도 사용할 수 있습니다.
lotus_sametime	IBM Lotus Sametime은 기업을위한 실시간 통합 커뮤니케이션 및 협업을 제공하는 클라이언트-서버 애플리케이션 및 미들웨어 플랫폼입니다.
로터스 라이브	Lotus live (현재 IBM SmartCloud)는 메일, 파일 전송, 회의 및 양식을 포함하는 엔터프라이즈 용 웹 기반 협업 애플리케이션 제품군입니다.
mq	Mq (IBM Websphere MQ)는 애플리케이션 간 통신 프로토콜입니다.
icloud	iCloud는 사용자가 데이터를 저장하고 공유 할 수 있도록 Apple Inc.에서 개발 한 클라우드 컴퓨팅 서비스입니다.
iheartradio	iHeartRadio는 iHeartMedia가 소유 한 인터넷 라디오 서비스입니다.
imessage_file_download	iMessage 애플리케이션을 통해 두 iOS 기기간에 전송 된 비디오 메시지를 검색하는 Apple 웹 서비스입니다. 이 서명은 메시지 수신 장치에서 다운로드 한 비디오 만 분류합니다. 발신자가 업로드 한 동영상은 APN (Apple Push Notification)으로 분류됩니다.
Imgur	무료 온라인 이미지 호스팅 서비스입니다.
ica	ICA (Independent Computing Architecture)는 Citrix Company의 통신 프로토콜이자 자산입니다. Basic-DPI에서 http를 통한 부분 분류.
인스 타 그램	Instagram은 온라인 모바일 사진 공유 및 소셜 네트워킹 서비스입니다.
igmp	IGMP (Internet Group Management Protocol)를 사용하면 IP 호스트가 멀티 캐스트 그룹 구성원을 라우터에보고 할 수 있습니다.
ipp	IPP (Internet Printing Protocol)는 인터넷 도구 및 기술을 사용하는 원격 인쇄를위한 표준입니다.
Isakmp	ISAKMP (Internet Security Association and Key Management Protocol)는 SA (보안 연결)를 설정, 협상, 수정 및 삭제하기위한 절차 및 패킷 형식을 정의합니다.
iscsi	RFC3720에 설명 된 iSCSI (Internet Small Computer Systems Interface).
ios_ota_update	iOS OTA 업데이트는 무선으로 iOS 업데이트에 사용되는 프로토콜입니다.
ipcomp	ipcomp 프로토콜 (IP Payload Compression Protocol)은 IP 계층 (IANA 프로토콜 번호 : 108)에서 찾을 수 있습니다.
ip_in_ip	ip_in_ip 프로토콜 (IP_within_IP 캡슐화 프로토콜)은 IP 계층 (IANA 프로토콜 번호 : 94)에서 찾을 수 있습니다.
ipsec	IPSec 프로토콜은 호스트 통신 보안을위한 서비스를 제공합니다. IPsec은 보낸 사람의 인증을 허용하는 AH (인증 헤더)와 보낸 사람의 인증 및 데이터 암호화를 모두 허용하는 ESP (Encapsulating Security Payload)의 두 가지 보안 서비스를 제공합니다.
irc_transfer	이 프로토콜은 IRC 파일 전송에서 데이터를 전송합니다.
아이튠즈	iTunes는 디지털 음악 및 비디오 파일을 재생하고 구성하는 데 사용되는 Apple 독점 디지털 미디어 플레이어 응용 프로그램입니다.
jabber_transfer	Jabber 전송은 두 Jabber 클라이언트간에 파일을 전송하기위한 개방형 표준입니다.
java_update	Java Update는 JVM (Java Virtual Machine) 업데이트를위한 프로토콜입니다.
제다이	JEDI는 CITRIX 스트리밍 연결 프로토콜의 이름입니다. 참고 : Basic-DPI에서 https를 통한 부분 분류.
카자	KaZaA는 P2P 프로토콜입니다. [kazaa는 fasttrack이라고도합니다.]
kik	KIK Messenger는 중국어 인스턴트 메시징 서비스입니다.
왕	King은 모바일 게임 편집기입니다. 이 플러그인은 King 게임 콘텐츠 전송 트래픽과 King.com 웹 사이트 액세스를 처리합니다.
링크드 인	LinkedIn은 전문적인 소셜 네트워크입니다.
livemail_mobile	이제 Outlook으로 명명 된 Livemail_mobile은 휴대폰 용 웹 메일입니다. 암호화 된 트래픽은 windowslive 또는 live_hotmail로 분류됩니다.
거물	이 프로토콜 플러그인은 호스트 livestream.com 및 a749.g.akamai.net에 대한 http 트래픽을 분류합니다. 또한 SSL 트래픽을 일반 이름 livestream.com으로 분류합니다.
logmein_rescue	전용 플러그인을 사용하여 웹 브라우저에서 액세스 할 수있는 원격 PC 지원 소프트웨어.
마술 꾼	MagicJack은 가정 및 업무용 VoIP 서비스로 모바일 애플리케이션으로도 사용할 수 있으며 독점 ​​장치 (magicJack PLUS)와 함께 사용할 수도 있습니다.
mailru_agent	Mail.ru Agent는 텍스트, 오디오 및 비디오를 지원하는 크로스 플랫폼 모바일 메시징 응용 프로그램입니다. Mail.ru가 추천합니다.
막툼	Maktoob은 웹 메일 프로토콜입니다.
mgcp	MGCP 프로토콜은 음성 IP 애플리케이션을위한 신호 프로토콜입니다.
msrp	MSRP (Message Session Relay Protocol)는 RFC 4975에서 정의한 인스턴트 메시지 전송을위한 프로토콜입니다.
activesync	Microsoft ActiveSync는 Microsoft에서 개발 한 모바일 데이터 동기화 기술 및 프로토콜입니다.
Lync	Microsoft Lync IM, VoIP 및 데스크톱 공유 서비스 (Lync Server 및 Lync Online이 지원됨).
lync_online	온라인 버전의 Microsoft Lync IM 및 VoIP 서비스 (Office 365에 포함됨).
office365	Office 365는 인터넷에서 Office 응용 프로그램에 액세스 할 수있는 Microsoft 온라인 서비스입니다.
msrpc	MSRPC (Microsoft Remote Procedure Call)는 DCE RPC 메커니즘의 Microsoft 구현입니다.
svcctl	이 프로토콜은 Windows 서비스를 원격으로 제어하는 ​​데 사용됩니다. MS-SCMR (서비스 제어 관리자 원격 프로토콜)이라고도합니다. 자세한 내용은 https://msdn.microsoft.com/en-us/library/cc245832.aspx를 참조 하십시오 .
공유 지점	SharePoint는 콘텐츠 관리 및 문서 관리 시스템과 같은 여러 웹 응용 프로그램을 중앙 집중식으로 대체하도록 설계된 웹 응용 프로그램 플랫폼입니다.
sharepoint_admin	SharePoint는 콘텐츠 관리 및 문서 관리 시스템과 같은 여러 웹 응용 프로그램을 중앙 집중식으로 대체하도록 설계된 웹 응용 프로그램 플랫폼입니다. 이 플러그인은 SharePoint의 관리 백엔드를 분류합니다. 참고 : Basic-DPI에서 http / sharepoint를 통한 부분 분류.
sharepoint_blog	SharePoint는 콘텐츠 관리 및 문서 관리 시스템과 같은 여러 웹 응용 프로그램을 중앙 집중식으로 대체하도록 설계된 웹 응용 프로그램 플랫폼입니다. 이 플러그인은 SharePoint의 블로그 관리 모듈을 분류합니다.
sharepoint_calendar	SharePoint는 콘텐츠 관리 및 문서 관리 시스템과 같은 여러 웹 응용 프로그램을 중앙 집중식으로 대체하도록 설계된 웹 응용 프로그램 플랫폼입니다. 이 플러그인은 SharePoint의 달력 관리 모듈을 분류합니다.
sharepoint_document	SharePoint는 콘텐츠 관리 및 문서 관리 시스템과 같은 여러 웹 응용 프로그램을 중앙 집중식으로 대체하도록 설계된 웹 응용 프로그램 플랫폼입니다. 이 플러그인은 SharePoint의 문서 관리 모듈을 분류합니다. 참고 : Basic-DPI에서 http / sharepoint를 통한 부분 분류.
mpls_in_ip	mpls_in_ip 프로토콜 (Multi Protocol Label Switching 데이터 전달 메커니즘)은 IP 계층 (IANA 프로토콜 번호 : 137)에서 찾을 수 있습니다.
nrdp	Nagios 원격 데이터 프로세서 (NDRP)는 Nagios를위한 유연한 데이터 전송 메커니즘 및 프로세서입니다.
nrpe	Nagios Remote Plugin Executor (NRPE)는 원격 시스템에서 호스팅되는 스크립트를 사용하여 원격 시스템 모니터링을 허용하는 Nagios 에이전트입니다.
nspi	이름 서비스 공급자 인터페이스는 Exchange에서 사용하는 프로토콜입니다.
넷플릭스	Netflix는 Silverlight 프로토콜을 사용하여 비디오를 스트리밍하는 사이트입니다. 참고 : Basic-DPI에서 Netflix는 때때로 http로 분류됩니다.
netmeeting_ils	Netmeeting ILS는 Netmeeting과 ILS (Internet Locator Server)간에 사용되는 프로토콜입니다. Netmeeting은 여러 버전의 Microsoft Windows에 포함 된 VoIP 및 다 지점 화상 회의 클라이언트입니다. ILS (Internet Locator Server)는 다른 사용자를 찾고 랑데뷰를 용이하게하는 데 사용되는 디렉토리입니다.
ntp	NTP (Network Time Protocol)는 인터넷 네트워크를 통한 컴퓨터 시계의 시간 동기화 시스템입니다.
wfc	Wi-Fi 연결 (WFC)은 Wii 및 DS 비디오 게임 시스템을위한 Nintendo 온라인 게임 서비스입니다.
Sonmp	Nortel / SynOptics 네트워크 관리 프로토콜은 독점적 인 Nortel Networks 관리 프로토콜입니다.
okcupid	OkCupid는 온라인 데이트 웹 사이트입니다. 이 플러그인은 검색 및 파일 업로드 워크 플로를 모두 분류합니다.
ocsp	이 네트워크 프로토콜은 인증서를 확인하는 데 사용됩니다.
우부	oovoo는 오디오 / 비디오를 지원하는 인스턴트 메신저 응용 프로그램입니다.
ospf	OSPF (Open Short Path First)는 대규모 자율 시스템 네트워크 내에서 사용되는 링크 상태 라우팅 프로토콜입니다.
opera_update	Opera Update는 Opera 브라우저의 업데이트에 사용되는 프로토콜입니다. 참고 : Basic-DPI에서 https를 통한 부분 분류.
orkut	Orkut은 Facebook 또는 Twitter와 경쟁하는 소셜 네트워킹 웹 사이트로, 브라질에서 인기가 있으며 현재 Google Inc.에서 소유 및 운영하고 있습니다.
시야	Office 365 생산성 제품군의 온라인 Microsoft Outlook 암호화 서비스.
오와	Outlook Web App은 Microsoft Outlook 데스크톱 응용 프로그램에 액세스 할 때 전자 메일 (S / MIME 지원 포함), 일정, 연락처, 작업, 문서 (SharePoint 또는 2010 Office Web Apps에서 사용) 및 기타 사서함 콘텐츠에 액세스하는 데 사용됩니다. 사용할 수 없습니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
팔 토크	Paltalk는 인스턴트 메시징 프로토콜입니다.
paltalk_audio	Paltalk가 음성 채팅에서 사용하는 독점 프로토콜입니다.
paltalk_transfer	Paltalk는 인스턴트 메시징 프로토콜입니다.
paltalk_video	비디오에서 Paltalk에서 사용하는 독점 프로토콜입니다.
판도라	Pandora는 미국에서 사용자 지정 가능한 음악 스트리밍 서비스입니다.
페이스트 빈	pastebin은 누구나 일반 텍스트를 저장할 수있는 웹 응용 프로그램 유형입니다. 인터넷 릴레이 채팅을 통해 코드 검토를 위해 짧은 소스 코드 스 니펫을 공유하는 데 가장 일반적으로 사용됩니다.
pastebin_posting	Pastebin_posting은 pastebin.com 웹 사이트의 게시 워크 플로를 분류하는 데 사용됩니다.
pcanywhere	PCAnywhere는 원격 제어 솔루션입니다. Windows 및 Linux 시스템을 모두 관리 할 수 ​​있습니다. 향상된 비디오 성능과 내장 된 AES 256 비트 암호화를 통해 빠르고 안전하게 통신 할 수 있습니다. PCAnywhere는 강력한 파일 전송 기능도 제공합니다.
포토버킷	데스크톱 및 모바일 장치에서 사용할 수있는 고급 편집 기능이있는 사진 공유 웹 서비스입니다.
Pinterest	사용자가 인터넷 핀 보드에 ​​개인 요소를 첨부 할 수있는 온라인 서비스입니다.
psn	PSN (PlayStation Network)은 Sony에서 만든 콘솔 용 온라인 게임 서비스입니다.
다수의 물고기	주로 캐나다, 영국, 호주 및 미국에서 인기있는 무료 온라인 데이트 사이트입니다. 이 플러그인은 검색 및 파일 업로드 워크 플로를 모두 분류합니다.
qik_video	QIK는 웹에서 라이브 및 VOD 스트리밍이 가능한 PC / 스마트 폰 애플리케이션입니다. 영상 채팅 추가 기능은 아직 지원되지 않습니다.
qq	QQ는 중국에서 가장 인기있는 무료 인스턴트 메시징 컴퓨터 프로그램입니다. 참고 : Basic-DPI에서 https를 통한 부분 분류.
qq_transfer	QQ를 통한 파일 전송
qq_games	게임 리뷰, 포럼, 뉴스를 제공하는 Tencent 게임 포털.
qq_mail	Tencent 웹 메일.
qq_weibo	QQ WeiBo는 중국의 Twitter와 유사한 마이크로 블로깅 웹 사이트입니다. Tencent의 QQ의 일부입니다.
qq_web	QQ.com은 Tencent에서 호스팅하는 다중 서비스 중국어 웹 포털입니다. Wechat 트래픽이 QQ 웹에 나타날 수 있습니다.
qqdownload	QQDownload는 중국어 다운로드 관리자입니다. 그 목적은 HTTP 또는 BitTorrent 프로토콜을 사용하여 파일을 빠르게 다운로드하는 것입니다.
qqlive	QQLive는 Peer-to-Peer 모드로 TV를 시청하기위한 애플리케이션입니다.
qqmusic	QQMusic은 오디오 파일을 다운로드하고 스트리밍하기위한 중국어 P2P 파일 공유 소프트웨어입니다.
qqstream	QQStream은 중국 P2P 파일 공유 소프트웨어입니다. QQstream은 QQLive 및 QQMusic의 데이터 스트림을 포함하는 메타 프로토콜입니다.
지진	Quake는 Quake 클라이언트와 Quake 서버 간의 통신을 허용하는 프로토콜입니다.
퀵	QUIC는 웹 콘텐츠 전송을 위해 주로 Google에서 개발 한 개방형 네트워킹 프로토콜입니다.
qvod	QVOD는 P2P 기반 Video-On-Demand 플레이어입니다.
Rapidshare	RapidShare는 파일을 저장, 전송 및 공유하는 온라인 솔루션입니다.
RTSP	RTSP (Real Time Streaming Protocol)는 실시간 속성으로 데이터 전달을 제어하기위한 애플리케이션 수준 프로토콜입니다. RTSP는 오디오 및 비디오와 같은 실시간 데이터의 제어 된 주문형 제공을 가능하게하는 확장 가능한 프레임 워크를 제공합니다.
rdp	터미널 서버의 핵심 구성 요소는 원격 데스크톱 프로토콜로, 씬 클라이언트가 네트워크를 통해 터미널 서버와 통신 할 수 있도록합니다. 이 프로토콜은 현재 Microsoft NetMeeting 회의 소프트웨어 제품에서 사용되는 국제 표준 다중 채널 회의 프로토콜 인 ITU (International Telecommunications Union)의 T.120 프로토콜을 기반으로합니다. 고 대역폭 엔터프라이즈 환경에 맞게 조정되었으며 암호화 된 세션도 지원합니다.
rpc	RPC (Remote Procedure Call)는 분산 컴퓨팅의 클라이언트-서버 모델을 구현하기위한 패러다임입니다. 제공된 인수를 사용하여 지정된 프로 시저를 실행하기위한 요청이 원격 시스템으로 전송되고 결과가 호출자에게 반환됩니다.
retroshare	Retroshare는 안전하고 분산 된 통신 및 파일 공유 오픈 소스 플랫폼입니다.
rip1	RIP1 (Routing Information Protocol 버전 ​​1)은 Inter Autonomous Systems에서 사용되는 Distance Vector 라우팅 프로토콜입니다.
rip2	RIP2 (Routing Information Protocol 버전 ​​2)는 프로토콜 버전 1의 개선 사항입니다. 주요 차이점은 브로드 캐스트 대신 멀티 캐스트를 사용하고 서브넷이 이제 업데이트 내부로 전송되기 때문에 가변 길이 서브넷 마스크 네트워크를 지원한다는 것입니다.
ripng1	RIPng (RIP New Generation)는 라우터가 IPv6 기반 네트워크를 통해 컴퓨팅 경로에 대한 정보를 교환 할 수 있도록합니다. RIPng는 거리 벡터 프로토콜입니다. IPv6는 라우터 검색을위한 다른 메커니즘을 제공하므로 RIPng는 라우터에서만 구현해야합니다.
Rovio	Rovio는 모바일 게임 편집기입니다. 이 플러그인은 Rovio 게임 콘텐츠 전송 트래픽 및 Rovio 웹 사이트 액세스를 처리합니다.
rss	RSS는 자주 업데이트되는 저작물을 표준화 된 형식으로 게시하는 데 사용되는 웹 피드 형식입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
영업	Salesforce는 온라인 고객 관계 관리 웹 제품입니다.
수액	SAP는 대부분의 회사에서 사용하는 프로토콜이자 ERP 애플리케이션의 이름입니다.
세컨드 라이프	Secondlife는 사용자가 모션 아바타를 통해 서로 상호 작용할 수있는 인터넷 기반 가상 세계입니다.
ssh	Secure Socket Shell이라고도하는 SSH (Secure Shell)는 UNIX 기반 명령 인터페이스이자 원격 컴퓨터에 대한 보안 액세스를 얻기위한 프로토콜입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
충격	STUN (Session Traversal Utilities for NAT)을 사용하면 NAT 뒤의 클라이언트가 두 호스트간에 UDP 터널을 설정할 수 있습니다.
sharepoint_online	Microsoft Sharepoint 서비스의 온라인 버전 (Office 365에 포함됨).
은빛	Silverlight는 프로그래밍 가능한 애니메이션을 렌더링하고 비디오를 스트리밍하도록 설계된 Microsoft 웹 브라우저 플러그인입니다. Adobe Flash와 유사합니다 : 애니메이션 벡터 그래픽, H264 비디오 스트리밍. 이 플러그인은 HTTP를 통한 Silverlight 응용 프로그램 다운로드와 이러한 응용 프로그램에서 스트리밍되는 HTTP 비디오 (Microsoft Smooth Streaming이라고 함)를 분류합니다.
비누	SOAP는 분산 된 분산 환경에서 구조화 된 정보를 교환하기위한 XML 기반의 경량 프로토콜입니다. 참고 :이 프로토콜은 HTTP 요청에서 찾을 수 있지만 일부 알려진 웹 애플리케이션 또는 서비스가 대신 분류 된 경우 분류되지 않습니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
sccp	SCCP (Skinny Client Control Protocol)는 Cisco Call Manager와 Cisco VOIP 전화간에 사용되는 Cisco 독점 프로토콜입니다. 다른 공급 업체에서도 지원합니다.
병역 기피자	Slacker Radio는 웹 브라우저 및 모바일 애플리케이션에서 사용할 수있는 온라인 음악 스트리밍 서비스입니다.
슬링 박스	Slingbox는 가정용 기기에서 수신 한 TV 프로그램을 시청하고 제어하는 ​​데 사용되는 인터넷 스트리밍 프로토콜입니다.
스냅 챗	Snapchat은 사진 / 동영상 공유 서비스입니다.
양말 5	Socks 5는 인증 프로토콜입니다.
Somud	SoMud는 BitTorrent 클라이언트입니다. 이 서명은 SoMud 클라이언트와 관련된 http를 통해 BitTorrent 추적기 스트림을 분류합니다. 데이터 스트림은 비트 토렌트로만 분류됩니다.
사운드 클라우드	SoundCloud는 사용자가 자신의 사운드를 업로드, 홍보 및 다른 사람들과 공유 할 수있는 온라인 오디오 배포 플랫폼입니다.
Sourceforge	Sourceforge는 오픈 소스 소프트웨어 개발을위한 웹 기반 코드 저장소입니다.
스파이	SPDY는 웹 콘텐츠 전송을 위해 주로 Google에서 개발 한 개방형 네트워킹 프로토콜입니다. 참고 : Basic-DPI에서 https를 통한 부분 분류.
스포티 파이	Spotify는 음악 스트리밍의 응용 프로그램입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
다람쥐	SquirrelMail은 PHP 스크립팅 언어로 작성된 웹 기반 이메일 애플리케이션입니다.
증기	Steam은 Valve Corporation에서 개발 한 디지털 배포, 디지털 권한 관리, 멀티 플레이어 및 커뮤니케이션 플랫폼입니다.
norton_update	Symantec Norton 안티 바이러스에 대한 바이러스 정의 및 엔진 업데이트.
syslog	Syslog 프로토콜은 클라이언트와 서버 사이의 네트워크를 통해 이벤트 알림 메시지를 전송하는 데 사용됩니다.
Sna	SNA (Systems Network Architecture)는 IBM의 메인 프레임 네트워크 표준입니다.
팀	독점 TeamSpeak2 프로토콜은 게이머와 지향 TeamSpeak2 VoIP 소프트웨어에서 사용됩니다.
teamspeak_v3	TeamSpeak 3는 원래 TeamSpeak 통신 시스템의 유산을 이어갑니다. TeamSpeak 3는 이전 제품의 확장이 아니라 독점 프로토콜과 핵심 기술을 C ​​++로 완전히 재 작성한 것입니다.
팀 뷰어	TeamViewer는 유지 관리 작업을 수행하기 위해 원격 컴퓨터에 연결할 수있는 응용 프로그램입니다. 현재 디스플레이를 원격 컴퓨터에 표시하고, 파일을 전송하고, VPN 터널을 생성 할 수도 있습니다.
텔넷	Telnet은 상당히 일반적인 양방향 8 비트 바이트 지향 통신 기능을 제공합니다. 주요 목표는 터미널 장치와 터미널 지향 프로세스 간의 인터페이스 표준 방법을 제공하는 것입니다.
Teredo	Teredo 프로토콜은 UDP를 통한 IPv6 터널링, NAT 통과 및 최소 오버 헤드를 지원합니다.
tacacs_plus	TACACS + (Terminal Access Controller Access-Control System Plus)는 하나 이상의 중앙 집중식 서버를 통해 라우터, 네트워크 액세스 서버 및 기타 네트워크 컴퓨팅 장치에 대한 액세스 제어를 제공하는 Cisco Systems 독점 프로토콜입니다.
tibcordv	이 프로토콜은 은행 부문에서 사용됩니다.
tor2web	Tor2web은 인터넷 사용자가 Tor 브라우저를 사용할 필요없이 Tor Onion 서비스에 액세스 할 수 있도록하기위한 프로젝트입니다.
텀블러	Tumblr는 사용자가 블로그 게시물과 멀티미디어 콘텐츠를 게시 할 수있는 소셜 네트워킹 및 마이크로 블로깅 플랫폼입니다.
경련	Twitch.tv는 비디오 게임에 초점을 맞춘 라이브 비디오 스트리밍 서비스입니다.
트위트	Twitter 전용 사진 공유 웹 서비스입니다. 이 서비스를 통해 사용자는 웹 브라우저 및 모바일 장치에서 Twitter 팔로어와 사진을 공유 할 수 있습니다.
트위터	사용자가 짧은 텍스트 기반 메시지를 읽고 보낼 수있는 온라인 마이크로 블로깅 서비스입니다.
ustream	Ustream은 PC 및 모바일 플랫폼에서 사용할 수있는 라이브 비디오 방송 웹 서비스입니다.
토렌트	uTorrent는 비공개 소스 BitTorrent 클라이언트입니다. 이 플러그인은 소프트웨어 회사에 대한 트래픽을 분류합니다. 이 소프트웨어에 의해 생성 된 트래픽은 비트 토렌트로 분류됩니다.
utp	BitTorrent 전송 계층.
uusee	Uusee는 BitTorrent P2P 기술을 사용하는 P2P TV 소프트웨어입니다. 네트워크 코딩 기술을 사용합니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
Vevo	VEVO는 Google, Universal Music Group 및 Sony Music Entertainment에서 후원하는 뮤직 비디오 스트리밍 플랫폼입니다.
Viber	Viber는 스마트 폰용 무료 내장형 VoIP 애플리케이션입니다.
Vimeo	Vimeo는 웹 브라우저 또는 모바일 애플리케이션에서 액세스 할 수있는 고화질 비디오 스트리밍 플랫폼입니다.
덩굴	Vine은 짧은 형식의 동영상 공유 서비스입니다.
vrrp	VRRP (Virtual Router Redundancy Protocol)는 고정 기본 라우팅 환경에 내재 된 단일 장애 지점을 제거하도록 설계된 프로토콜입니다. VRRP는 가상 라우터에 대한 책임을 LAN의 VRRP 라우터 중 하나에 동적으로 할당하는 선택 프로토콜을 지정합니다.
VMware	VMWare는 VMWare 응용 프로그램에서 사용하는 프로토콜로, 네트워크 인터페이스와 가상 머신에 대한 원격 액세스를 허용합니다.
vmware_horizon_view	Vmware Horizon View는 VMware에서 개발 한 상업용 데스크톱 가상화 제품입니다. 이 플러그인은 가상 머신과 Mac / Windows 클라이언트간에 UDP를 통해 pcoip 스트림을 분류합니다.
Waze	Waze는 커뮤니티 기반 매핑, 교통 및 내비게이션 앱입니다.
webex	WebEx는 온라인 회의, 화상 회의 및 협업 응용 프로그램입니다.
whatsapp	WhatsApp Messenger는 사용자가 SMS 비용을 지불하지 않고도 메시지를 교환 할 수있는 크로스 플랫폼 인스턴트 모바일 메시징 애플리케이션입니다. WhatsApp Messenger는 iPhone, BlackBerry, Android 및 Nokia에서 사용할 수 있습니다.
후이즈	WHOIS는 도메인 이름, IP 주소 블록 또는 자율 시스템과 같은 인터넷 리소스의 등록 된 사용자 또는 양수인을 저장하는 데이터베이스와 더 광범위한 기타 정보를 쿼리하는 데 널리 사용되는 쿼리 및 응답 프로토콜입니다.
wiiconnect24	WiiConnect24는 Nintendo Wii 게임 시스템에 구현 된 비동기 통신 프로토콜입니다. 콘솔에 내장 된 일부 정보 채널 및 서비스 및 일부 게임에서 사용됩니다.
위키 백과	Wikipedia는 인터넷에서 가장 큰 다국어 무료 콘텐츠 백과 사전입니다.
windows_azure	이 프로토콜 플러그인은 ssl 트래픽을 일반 이름 msecnd.net으로 분류합니다.
승리	WINS (Windows Internet Naming Service)는 NetBIOS 컴퓨터 이름에 대한 이름 서버 및 서비스 인 NetBIOS 이름 서비스 (NBNS)를 Microsoft에서 구현 한 것입니다. 이 플러그인은 서버 간의 복제 흐름을 분류합니다. 클라이언트-서버 흐름은 nbns 플러그인에 의해 처리됩니다.
live_storage	Windows Live 파일 저장소는 MSN 및 Skydrive와 같이 저장소가 필요할 수있는 다른 Microsoft 웹 서비스에서 사용하도록 설계된 Microsoft 웹 서비스입니다.
live_groups	Windows Live 그룹은 사용자가 공유, 토론 및 조정을위한 소셜 그룹을 만들 수있는 Microsoft의 온라인 서비스입니다.
live_hotmail	Windows Live Hotmail은 Microsoft에서 운영하는 무료 웹 메일 서비스입니다. 이제 서비스 이름은 Outlook입니다.
livemail_attach	Windows Live 메일 파일 첨부 파일 업로드 감지.
스카이 드라이브	이 프로토콜 플러그인은 ssl 트래픽을 일반 이름 live.com.nsatc.net, Skydrive, skydrive.wns.windows.com, skydrivesync.policies.live.net, gateway.edge.messenger.live.com, skyapi로 분류합니다. .live.net, skydrive.live.com, onedrive.live.com 및 storage.live.com.
skydrive_login	Microsoft 소유의 온라인 파일 저장 서비스입니다.
windows_marketplace	Windows Marketplace는 Windows Phone 7/8 및 Microsoft Windows 8 플랫폼을위한 Microsoft의 서비스로, 사용자가 타사에서 개발 한 응용 프로그램을 찾아보고 다운로드 할 수 있습니다. Microsoft Store 소매점 웹 사이트도 분류됩니다.
윈도우 업데이트	Windows_update는 Windows 시스템 업데이트에 사용되는 프로토콜입니다.
워드 프레스	WordPress는 인기있는 블로그 시스템입니다. 이 플러그인은 온라인 서비스를 호스팅하는 Wordpress.com 블로그의 사용을 분류합니다.
와	WOW는 온라인 롤 플레잉 게임입니다.
엑스 박스 라이브	Microsoft Corporation에서 만들고 운영하는 온라인 멀티 플레이어 게임 및 디지털 미디어 전달 서비스입니다.
xboxlive_marketplace	Xbox Live 마켓 플레이스는 사용자가 게임과 멀티미디어를 구매하고 다운로드 할 수있는 서비스입니다.
xbox_music	Xbox Music은 음악을위한 온라인 서비스입니다.
xbox_video	Microsoft 영화 및 TV는 영화, TV 프로그램 및 시리즈를 시청하는 온라인 서비스입니다.
Xhamster	포르노 비디오 스트리밍 플랫폼.
yahoo_groups	야후! 그룹스는 무료 메일 링리스트, 사진 및 파일 공유, 그룹 캘린더 등을 제공합니다.
ymail_classic	야후! Mail Classic은 Yahoo!의 원래 인터페이스였습니다. 우편.
ymail2	이 프로토콜은 웹 메일 Yahoo의 ajax 기반 버전입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
ymsg	Yahoo Messenger는 Yahoo Instant Messenger 응용 프로그램에서 사용자간에 인스턴트 메시지, 파일 및 이메일을 보내는 데 사용됩니다.
ymsg_conf	버전 11.5.0부터 음성 통화가 지원되지 않으므로 rtp 상속은 더 이상 사용되지 않습니다.
ymsg_transfer	이 프로토콜은 ymsg를 통한 파일 전송에 사용됩니다.
ymsg_video	(10.0.0.270 이전 버전)이 프로토콜은 Yahoo Messenger에서 화상 대화를 위해 사용합니다.
yahoo_search	이 프로토콜은 Yahoo 검색 엔진에 쿼리를 보내는 데 사용됩니다.
ymail_mobile_new	Yahoo Mail Mobile_new는 모바일에 적합한 새로운 yahoo.com 웹 메일입니다.
ymsg_webmessenger	야후 웹 메신저.
야후	Yahoo는 Yahoo와 관련된 일반 웹 서비스를 분류하는 의사 프로토콜입니다. 참고 : Basic-DPI에서 http를 통한 부분 분류.
ypbind	ypbind 유틸리티는 NIS 바인딩 정보를 유지하는 프로세스입니다. 시작시 네트워크 브로드 캐스트를 사용하여 시스템의 기본 도메인 (domainname (1) 명령으로 설정 됨) 서비스를 담당하는 NIS 서버를 검색합니다.
yppasswd	Yellow Page Password 프로토콜을 사용하면 네트워크 인터페이스 시스템 카드의 로그인 및 암호를 수정할 수 있습니다.
ypserv	Yellow Pages Server는 NIS 도메인 내의 클라이언트 시스템에 NIS 데이터베이스를 배포하는 데 사용되는 프로토콜입니다.
유튜브	Youtube는 사용자가 동영상을 보내거나 보는 웹 사이트입니다.


입증
Splunk App for Stream은 Linux, Mac 및 Windows에서 이러한 인증 프로토콜의 캡처를 지원합니다. 자세한 내용은 Stream 용 Splunk 앱 사용 설명서의 스트림 구성을 참조하십시오 .

직경
DIAMETER 프로토콜

이름	기술	기간
바이트	전송 된 총 바이트 수입니다.	flow.bytes
src_ip	클라이언트 IP 주소	flow.c-ip
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
src_port	클라이언트 포트 번호	flow.c-port
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
dest_ip	서버 IP 주소	flow.s-ip
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
dest_port	서버 포트 번호	flow.s-port
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
걸린 시간	최종 사용자 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
수송	전송 수준 프로토콜	flow.transport
acct_input_octets	제공된 서비스 과정에서 포트에서 수신 된 옥텟 수를 나타냅니다.	diameter.acct-input-octets
acct_multi_session_id	여러 회계 세션 간 연결	diameter.acct-multi-session-id
acct_output_octets	서비스를 제공하는 과정에서 포트로 전송 된 옥텟 수를 나타냅니다.	diameter.acct-output-octets
acct_record_number	세션 내 하나의 레코드에 대한 고유 식별자	diameter.acct-record-number
acct_record_type	기록 유형	diameter.acct-record-type
acct_session_id	회계 세션 ID	diameter.acct-session-id
acct_sub_session_id	하위 세션 식별자	diameter.acct-sub-session-id
application_id	메시지를 적용 할 수있는 응용 프로그램을 식별합니다.	diameter.application-id
auth_request_type	요청 된 인증 유형	diameter.auth-request-type
called_station_id	사용자가 DNIS (Dialed Number Identification) 또는 유사한 기술을 사용하여 전화를 건 전화 번호	diameter.called-station-id
calling_station_id	클라이언트 ID	diameter.calling-station-id
command_code	Diameter 요청과 관련된 명령	diameter.command-code
command_flags	다음과 같이 한 바이트에서 명령의 일부 속성을 정의하는 비트 필드 : [RPE .....] ( 'R'equest / answer,'P'roxiable, 'E'rror)	diameter.command-flags
destination_host	현재 메시지의 대상 지름 호스트	diameter.destination-host
end_to_end_id	중복 메시지 감지에 사용	diameter.end-to-end-id
framed_ip	IP 주소	diameter.framed-ip
hop_by_hop_id	Diameter 요청 및 응답 메시지를 일치시키는 데 사용	diameter.hop-by-hop-id
로그인	사용자의 로그인 문자열	diameter.login
nas_id	NAS 원본 액세스 요청의 고유 식별자	diameter.nas-id
nas_ip	NAS 발신 액세스 요청의 IP 주소	diameter.nas-ip
nas_port	NAS에있는 사용자의 물리적 포트 번호	diameter.nas-port
nas_port_id	NAS 식별	diameter.nas-port-id
nas_port_type	NAS가 사용자를 인증하는 데 사용하는 물리적 포트 유형을 나타냅니다.	diameter.nas-port-type
origin_host	현재 메시지의 소스 지름 호스트	diameter.origin-host
결과 _ 코드	특정 Diameter 요청이 성공적으로 완료되었는지 여부를 나타냅니다.	diameter.result-code
session_id	현재 사용자 세션을 고유하게 식별합니다.	diameter.session-id
종료 _ 원인	세션이 종료 된 방법을 나타냅니다.	diameter.terminate-cause
LDAP
경량 디렉토리 액세스 프로토콜 RFC 1777

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
ack_packets_in	클라이언트에서 서버로 전송 된 승인 패킷 수	flow.cs-ack-packets
ack_packets_out	서버에서 클라이언트로 전송 된 확인 패킷 수	flow.sc-ack-packets
missing_packets_in	요청 내에서 감지 된 누락 된 패킷 간격 수	flow.cs-missing-packets
missing_packets_out	응답 내에서 감지 된 누락 된 패킷 간격 수	flow.sc-missing-packets
duplicate_packets_in	클라이언트에서 서버로 전송 된 중복 패킷 수	flow.cs-duplicate-packets
duplicate_packets_out	서버에서 클라이언트로 전송 된 중복 패킷 수	flow.sc-duplicate-packets
data_packets_in	클라이언트에서 서버로 전송 된 데이터 패킷 수	flow.cs-data-packets
data_packets_out	서버에서 클라이언트로 전송 된 데이터 패킷 수	flow.sc- 데이터 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
request_time	클라이언트가 요청을 보내는 데 걸린 시간 (마이크로 초)	flow.cs-send-time
request_ack_time	서버가 요청 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.cs-ack-time
회신 _ 시간	서버가 요청에 응답하기 시작하는 데 걸린 시간 (마이크로 초)	flow.sc- 응답 시간
응답 시간	서버가 응답을 보내는 데 걸린 시간 (마이크로 초)	flow.sc-send-time
response_ack_time	클라이언트가 응답 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.sc-ack-time
ssl_time	SSL 핸드 셰이크를 협상하는 데 걸린 시간 (마이크로 초)	flow.ssl-time
ssl_version	암호화에 사용되는 SSL 프로토콜 버전 또는 암호화되지 않은 경우 정의되지 않음	flow.ssl-version
data_center_time	마지막 요청 패킷에서 마지막 응답 패킷까지의 시간 (마이크로 초)	flow.data-center-time
client_rtt	클라이언트에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.cp-rtt
server_rtt	서버에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.ps-rtt
client_rtt_sum	클라이언트에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.cp-rtt-sum
server_rtt_sum	서버에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.ps-rtt-sum
client_rtt_packets	클라이언트에서 캡처 지점까지의 왕복 측정 횟수	flow.cp-rtt- 패킷
server_rtt_packets	서버에서 캡처 지점까지의 왕복 측정 횟수	flow.ps-rtt- 패킷
거절	서버에서 거부 한 요청 수	flow.refused
취소 된	클라이언트가 조기에 취소 한 HTTP 응답 수	flow.canceled
연결	TCP 세션 서버 끝점 (IP 주소 및 TCP 포트)	flow.connection
tcp_status	TCP 핸드 셰이크 상태 (0 = OK, 1 = RESET, 2 = IGNORED)	flow.tcp-status
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 계층 프로토콜 (udp 또는 tcp)	flow.transport
assertion_value	어설 션 값인 필터 표현식 두 번째 피연산자	ldap.assertion-value
assertion_description	속성 설명 인 필터 표현식 첫 번째 피연산자	ldap.attribute-description
contains_sasl	SASL 메커니즘을 사용하여 인증이 수행되었는지 여부를 나타냅니다.	ldap.contains-sasl
호스트 이름	CLDAP searchRequest에 대한 로그온 응답에서 추출 된 호스트 이름	ldap.hostname
message_id	메시지 식별	ldap.message-id
message_type	메시지 유형	ldap.message-type
집단	LDAP 요소; 중첩 된 요소가있는 이름-값 쌍을 포함하는 맵	ldap.elements
반지름
사용자 서비스 RFC 2865에서 원격 인증 다이얼

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 수준 프로토콜	flow.transport
신분증	패킷 식별자	radius.id
암호	반경 메시지 코드	radius.code
상태	상태	radius.status
로그인	사용자 로그인 문자열	radius.login
login_ipv6_host	사용자를 연결할 시스템을 나타냅니다.	radius.login-ipv6-host
세션 타임 아웃	최대 세션 시간 (초)	radius.session-timeout
idle_timeout	세션의 최대 유휴 기간 (초)	radius.idle-timeout
nas_id	NAS 원본 액세스 요청의 고유 식별자	radius.nas-id
nas_ip	NAS 발신 액세스 요청의 IP 주소	radius.nas-ip
nas_ipv6	NAS 발신 액세스 요청의 IPV6 주소	radius.nas-ipv6
nas_port	NAS에있는 사용자의 물리적 포트 번호	radius.nas-port
nas_port_id	NAS 식별	radius.nas-port-id
nas_port_type	NAS가 사용자를 인증하는 데 사용하는 물리적 포트 유형	radius.nas-port-type
시작 시간	사용자 서비스의 시작	radius.start-time
stop_time	사용자 서비스의 끝	radius.stop-time
종료 _ 원인	세션이 종료 된 방법	radius.terminate-cause
framed_ip	사용자에 대해 구성 할 IP 주소	radius.framed-ip
framed_ipv6_route	NAS에서 사용자를 위해 구성 할 라우팅 정보	radius.framed-ipv6-route
framed_ipv6_pool	사용자에게 IPv6 접두사를 할당하는 데 사용해야하는 할당 된 풀의 이름	radius.framed-ipv6-pool
콜백 _ 번호	콜백에 사용할 전화 걸기 문자열	radius.callback-number
called_station_id	사용자가 전화 한 전화 번호	radius.called-station-id
vendor_id	공급 업체의 SMI 네트워크 관리 사설 기업 코드	radius.vendor-id
acct_session_id	회계 세션 ID	radius.account-session-id
sgsn_address	SGSN의 IP 주소	radius.sgsn-ip
sgsn_mcc_mnc	SGSN MCC 및 MNC	radius.sgsn-mcc


데이터 베이스
Splunk App for Stream은 Linux, Mac 및 Windows에서 이러한 데이터베이스 프로토콜의 캡처를 지원합니다. 자세한 내용은 Stream 용 Splunk 앱 사용 설명서의 스트림 구성을 참조하십시오 .

MYSQL
내 구조적 쿼리 언어

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
ack_packets_in	클라이언트에서 서버로 전송 된 승인 패킷 수	flow.cs-ack-packets
ack_packets_out	서버에서 클라이언트로 전송 된 확인 패킷 수	flow.sc-ack-packets
missing_packets_in	요청 내에서 감지 된 누락 된 패킷 간격 수	flow.cs-missing-packets
missing_packets_out	응답 내에서 감지 된 누락 된 패킷 간격 수	flow.sc-missing-packets
duplicate_packets_in	클라이언트에서 서버로 전송 된 중복 패킷 수	flow.cs-duplicate-packets
duplicate_packets_out	서버에서 클라이언트로 전송 된 중복 패킷 수	flow.sc-duplicate-packets
data_packets_in	클라이언트에서 서버로 전송 된 데이터 패킷 수	flow.cs-data-packets
data_packets_out	서버에서 클라이언트로 전송 된 데이터 패킷 수	flow.sc- 데이터 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자의 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
request_time	클라이언트가 요청을 보내는 데 걸린 시간 (마이크로 초)	flow.cs-send-time
request_ack_time	서버가 요청 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.cs-ack-time
회신 _ 시간	서버가 요청에 응답하기 시작하는 데 걸린 시간 (마이크로 초)	flow.sc- 응답 시간
응답 시간	서버가 응답을 보내는 데 걸린 시간 (마이크로 초)	flow.sc-send-time
response_ack_time	클라이언트가 응답 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.sc-ack-time
ssl_time	SSL 핸드 셰이크를 협상하는 데 걸린 시간 (마이크로 초)	flow.ssl-time
ssl_version	암호화에 사용되는 SSL 프로토콜 버전 또는 암호화되지 않은 경우 정의되지 않음	flow.ssl-version
sql_error_code	SQL 오류 코드	database.sql-error-code
client_rtt	클라이언트에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.cp-rtt
server_rtt	서버에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.ps-rtt
client_rtt_sum	클라이언트에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.cp-rtt-sum
server_rtt_sum	서버에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.ps-rtt-sum
client_rtt_packets	클라이언트에서 캡처 지점까지의 왕복 측정 횟수	flow.cp-rtt- 패킷
server_rtt_packets	서버에서 캡처 지점까지의 왕복 측정 횟수	flow.ps-rtt- 패킷
거절	서버에서 거부 한 요청 수	flow.refused
취소 된	클라이언트가 조기에 취소 한 HTTP 응답 수	flow.canceled
연결	TCP 세션 서버 끝점 (IP 주소 및 TCP 포트)	flow.connection
tcp_status	TCP 핸드 셰이크 상태 (0 = OK, 1 = RESET, 2 = IGNORED)	flow.tcp-status
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 계층 프로토콜 (udp 또는 tcp)	flow.transport
dbname	데이터베이스 이름	mysql.dbname
로그인	사용자의 로그인 문자열	mysql.login
질문	쿼리 문자열	mysql.query
Postgres
PostgreSQL

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
ack_packets_in	클라이언트에서 서버로 전송 된 승인 패킷 수	flow.cs-ack-packets
ack_packets_out	서버에서 클라이언트로 전송 된 확인 패킷 수	flow.sc-ack-packets
missing_packets_in	요청 내에서 감지 된 누락 된 패킷 간격 수	flow.cs-missing-packets
missing_packets_out	응답 내에서 감지 된 누락 된 패킷 간격 수	flow.sc-missing-packets
duplicate_packets_in	클라이언트에서 서버로 전송 된 중복 패킷 수	flow.cs-duplicate-packets
duplicate_packets_out	서버에서 클라이언트로 전송 된 중복 패킷 수	flow.sc-duplicate-packets
data_packets_in	클라이언트에서 서버로 전송 된 데이터 패킷 수	flow.cs-data-packets
data_packets_out	서버에서 클라이언트로 전송 된 데이터 패킷 수	flow.sc- 데이터 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
request_time	클라이언트가 요청을 보내는 데 걸린 시간 (마이크로 초)	flow.cs-send-time
request_ack_time	서버가 요청 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.cs-ack-time
회신 _ 시간	서버가 요청에 대한 응답을 시작하는 데 걸린 시간 (마이크로 초)	flow.sc- 응답 시간
응답 시간	서버가 응답을 보내는 데 걸린 시간 (마이크로 초)	flow.sc-send-time
response_ack_time	클라이언트가 응답 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.sc-ack-time
ssl_time	SSL 핸드 셰이크를 협상하는 데 걸린 시간 (마이크로 초)	flow.ssl-time
ssl_version	암호화에 사용되는 SSL 프로토콜 버전 또는 암호화되지 않은 경우 정의되지 않음	flow.ssl-version
data_center_time	마지막 요청 패킷에서 마지막 응답 패킷까지의 시간 (마이크로 초)	flow.data-center-time
client_rtt	클라이언트에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.cp-rtt
server_rtt	서버에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.ps-rtt
client_rtt_sum	클라이언트에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.cp-rtt-sum
server_rtt_sum	서버에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.ps-rtt-sum
client_rtt_packets	클라이언트에서 캡처 지점까지의 왕복 측정 횟수	flow.cp-rtt- 패킷
server_rtt_packets	서버에서 캡처 지점까지의 왕복 측정 횟수	flow.ps-rtt- 패킷
거절	서버에서 거부 한 요청 수	flow.refused
취소 된	클라이언트가 조기에 취소 한 HTTP 응답 수	flow.canceled
연결	TCP 세션 서버 끝점 (IP 주소 및 TCP 포트)	flow.connection
tcp_status	TCP 핸드 셰이크 상태 (0 = OK, 1 = RESET, 2 = IGNORED)	flow.tcp-status
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 계층 프로토콜 (udp 또는 tcp)	flow.transport
auth_type	서버에서 요청한 인증 방법	postgres.auth-type
dbname	데이터베이스 이름	postgres.dbname
오류	에러 메시지	postgres.error
로그인	사용자의 로그인 문자열	postgres.login
암호	사용자의 비밀번호 문자열	postgres.password
proto_version	프로토콜 버전	postgres.proto-version
질문	쿼리를 보냈습니다.	postgres.query
서버 버전	서버 버전	postgres.server-version
sql_error_code	SQL 오류 코드	database.sql-error-code
TDS (Sybase / MS SQL Server)
테이블 형식 데이터 스트림

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
ack_packets_in	클라이언트에서 서버로 전송 된 승인 패킷 수	flow.cs-ack-packets
ack_packets_out	서버에서 클라이언트로 전송 된 확인 패킷 수	flow.sc-ack-packets
missing_packets_in	요청 내에서 감지 된 누락 된 패킷 간격 수	flow.cs-missing-packets
missing_packets_out	응답 내에서 감지 된 누락 된 패킷 간격 수	flow.sc-missing-packets
duplicate_packets_in	클라이언트에서 서버로 전송 된 중복 패킷 수	flow.cs-duplicate-packets
duplicate_packets_out	서버에서 클라이언트로 전송 된 중복 패킷 수	flow.sc-duplicate-packets
data_packets_in	클라이언트에서 서버로 전송 된 데이터 패킷 수	flow.cs-data-packets
data_packets_out	서버에서 클라이언트로 전송 된 데이터 패킷 수	flow.sc- 데이터 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자의 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
request_time	클라이언트가 요청을 보내는 데 걸린 시간 (마이크로 초)	flow.cs-send-time
request_ack_time	서버가 요청 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.cs-ack-time
회신 _ 시간	서버가 요청에 응답하기 시작하는 데 걸린 시간 (마이크로 초)	flow.sc- 응답 시간
응답 시간	서버가 응답을 보내는 데 걸린 시간 (마이크로 초)	flow.sc-send-time
response_ack_time	클라이언트가 응답 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.sc-ack-time
ssl_time	SSL 핸드 셰이크를 협상하는 데 걸린 시간 (마이크로 초)	flow.ssl-time
ssl_version	암호화에 사용되는 SSL 프로토콜 버전 또는 암호화되지 않은 경우 정의되지 않음	flow.ssl-version
data_center_time	마지막 요청 패킷에서 마지막 응답 패킷까지의 시간 (마이크로 초)	flow.data-center-time
client_rtt	클라이언트에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.cp-rtt
server_rtt	서버에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.ps-rtt
client_rtt_sum	클라이언트에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.cp-rtt-sum
server_rtt_sum	서버에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.ps-rtt-sum
client_rtt_packets	클라이언트에서 캡처 지점까지의 왕복 측정 횟수	flow.cp-rtt- 패킷
server_rtt_packets	서버에서 캡처 지점까지의 왕복 측정 횟수	flow.ps-rtt- 패킷
거절	서버에서 거부 한 요청 수	flow.refused
취소 된	클라이언트가 조기에 취소 한 HTTP 응답 수	flow.canceled
연결	TCP 세션 서버 끝점 (IP 주소 및 TCP 포트)	flow.connection
tcp_status	TCP 핸드 셰이크 상태 (0 = OK, 1 = RESET, 2 = IGNORED)	flow.tcp-status
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 계층 프로토콜 (udp 또는 tcp)	flow.transport
신청	데이터베이스에 연결하는 데 사용되는 애플리케이션의 이름	tds.application
dbname	사용 된 데이터베이스의 이름	tds.dbname
호스트 이름	SQL 서버와 통신하는 워크 스테이션의 이름	tds.hostname
언어	사용자 로케일	tds.language
도서관	사용 된 네트워크 동적 연결 라이브러리의 이름	tds.library
로그인	사용자의 로그인 문자열	tds.login
암호	사용자의 비밀번호 문자열	tds.password
질문	사용자가 보낸 SQL 쿼리	tds.query
섬기는 사람	SQL Server를 호스팅하는 서버의 이름	tds.server
sql_result_codes	서버에서보고 한 SQL 결과 코드 목록	database.sql_error-code-int
TNS (Oracle)
투명한 네트워크 기판

이름	기술	기간
src_ip	클라이언트 IP 주소	flow.c-ip
dest_ip	서버 IP 주소	flow.s-ip
src_port	클라이언트 포트 번호	flow.c-port
dest_port	서버 포트 번호	flow.s-port
src_mac	16 진수 형식의 클라이언트 패킷 MAC 주소	flow.c-mac
dest_mac	16 진수 형식의 서버 패킷 MAC 주소	flow.s-mac
packets_in	클라이언트에서 서버로 보낸 총 패킷 수	flow.cs-packets
packets_out	서버에서 클라이언트로 보낸 총 패킷 수	flow.sc- 패킷
ack_packets_in	클라이언트에서 서버로 전송 된 승인 패킷 수	flow.cs-ack-packets
ack_packets_out	서버에서 클라이언트로 전송 된 확인 패킷 수	flow.sc-ack-packets
missing_packets_in	요청 내에서 감지 된 누락 된 패킷 간격 수	flow.cs-missing-packets
missing_packets_out	응답 내에서 감지 된 누락 된 패킷 간격 수	flow.sc-missing-packets
duplicate_packets_in	클라이언트에서 서버로 전송 된 중복 패킷 수	flow.cs-duplicate-packets
duplicate_packets_out	서버에서 클라이언트로 전송 된 중복 패킷 수	flow.sc-duplicate-packets
data_packets_in	클라이언트에서 서버로 전송 된 데이터 패킷 수	flow.cs-data-packets
data_packets_out	서버에서 클라이언트로 전송 된 데이터 패킷 수	flow.sc- 데이터 패킷
bytes_in	클라이언트에서 서버로 보낸 바이트 수	flow.cs-bytes
bytes_out	서버에서 클라이언트로 보낸 바이트 수	flow.sc- 바이트
바이트	전송 된 총 바이트 수	flow.bytes
걸린 시간	최종 사용자의 관점에서 플로우 이벤트를 완료하는 데 걸린 시간 (마이크로 초)	flow.time-taken
request_time	클라이언트가 요청을 보내는 데 걸린 시간 (마이크로 초)	flow.cs-send-time
request_ack_time	서버가 요청 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.cs-ack-time
회신 _ 시간	서버가 요청에 응답하기 시작하는 데 걸린 시간 (마이크로 초)	flow.sc- 응답 시간
응답 시간	서버가 응답을 보내는 데 걸린 시간 (마이크로 초)	flow.sc-send-time
response_ack_time	클라이언트가 응답 수신을 확인하는 데 걸린 시간 (마이크로 초)	flow.sc-ack-time
ssl_time	SSL 핸드 셰이크를 협상하는 데 걸린 시간 (마이크로 초)	flow.ssl-time
ssl_version	암호화에 사용되는 SSL 프로토콜 버전 또는 암호화되지 않은 경우 정의되지 않음	flow.ssl-version
data_center_time	마지막 요청 패킷에서 마지막 응답 패킷까지의 시간 (마이크로 초)	flow.data-center-time
client_rtt	클라이언트에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.cp-rtt
server_rtt	서버에서 캡처 지점까지의 평균 왕복 시간 (마이크로 초)	flow.ps-rtt
client_rtt_sum	클라이언트에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.cp-rtt-sum
server_rtt_sum	서버에서 캡처 지점까지의 모든 왕복 시간 측정의 합계	flow.ps-rtt-sum
client_rtt_packets	클라이언트에서 캡처 지점까지의 왕복 측정 횟수	flow.cp-rtt- 패킷
server_rtt_packets	서버에서 캡처 지점까지의 왕복 측정 횟수	flow.ps-rtt- 패킷
거절	서버에서 거부 한 요청 수	flow.refused
취소 된	클라이언트가 조기에 취소 한 HTTP 응답 수	flow.canceled
연결	TCP 세션 서버 끝점 (IP 주소 및 TCP 포트)	flow.connection
tcp_status	TCP 핸드 셰이크 상태 (0 = OK, 1 = RESET, 2 = IGNORED)	flow.tcp-status
실험 계획안	레벨 7 프로토콜 이름 (http, ftp 등)	flow.protocol
수송	전송 계층 프로토콜 (udp 또는 tcp)	flow.transport
dbname	액세스 한 데이터베이스 이름	tns.dbname
client_hostname	클라이언트 컴퓨터 호스트 이름	tns.client-hostname
client_os	클라이언트 시스템 운영 체제	tns.client-os
client_program_name	클라이언트 프로그램 이름	tns.client- 프로그램-이름
client_program_path	클라이언트 프로그램 절대 경로	tns.client-program-path
로그인	사용자의 로그인 문자열	tns.login
암호	사용자의 비밀번호 문자열	tns.password
질문	데이터베이스 쿼리	tns.query
호스트 이름	데이터베이스 서버 호스트 이름	tns.server-hostname
server_os	데이터베이스 서버 운영 체제	tns.server-os
버전	Oracle 서버의 버전 번호	tns.version
sql_error_code	SQL 오류 코드	database.sql-error-code

