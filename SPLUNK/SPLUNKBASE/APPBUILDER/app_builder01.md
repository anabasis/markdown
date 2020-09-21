# APP BUILDER

## 시작하기

### 설치

#### 1. 설치요구사항

- 실제 운영환경에 설치하지 마세요.
- 검색헤드 또는 인덱스 환경에 설치하지 마세요.
- Splunk Light에는 설치하지 마세요.
- Splunk Enterprise 라이선스 필요하지 않으나 Splunk 개발자 라이선스 권장
- Mozilla Firefox, Google Chrome, or Microsoft Internet Explorer 11
- 관리자 권한 필요(관리자 권한이 없으면 추가 기능 프로젝트를 만들거나 데이터 수집을 위해 Input 모듈 만들고 실행할 수 없음)
- CIM매핑 및 적응형 응답 Alert 생성하려면 CIM 추가 기능 버전 4.6 이상이 필요
- <https://splunkbase.splunk.com/app/1621/>
- <http://docs.splunk.com/Documentation/CIM/4.8.0/User/Install>

#### 2. 성능

- 데이터 입력을 개발하는 동안 필요없는 입력을 비활성화
- 빈번한 간격으로 실행하도록 스케줄 된 다중 데이터 입력은 시스템 자원을 소비
- 개발 환경에서 10 개 이상의 애드온을 실행 성능 저하

#### 3. 설치

- App Console설치 : <https://splunkbase.splunk.com/app/2962>
- CLI 지원 : ./splunk install app \<path\>/\<packagefilename\>
- Zip Unpack : $SPLUNK_HOME/etc/apps /splunk_app_addon-builder

#### 데이터 수집

- 일반필드

|Field|Type|Description|Example value|
|:--:|:--:|:--:|:--|
|apiKey|string|The MINT API key for the Splunk Add-on Builder.|"4t2fk73n"|
|appRunningSate|string|Not used.|"NA "|
|appVersionCode|string|Not used.|"NA"|
|appVersionName|string|The version of the Splunk Add-on Builder.|"2.1.0"|
|browser|string|The browser name.|"chrome"|
|browserVersion|string|The browser version.|"47.0.2526.111"|
|carrier|string|Not used.|"NA"|
|connection|string|Not used.|"NA"|
|device|string|The type of device used.|"MacIntel"|
|extraData|object|The version of Splunk Enterprise.|{"splunk_version": "6.3.2"}|
|locale|string|The user locale set in the browser.|"en-US".|
|osVersion|string|The version of the operating system.|"OS X 10.11.2"|
|packageName|string|The package name of the Splunk Add-on Builder.|"splunk_app_addon-builder"|
|platform|string|The platform.|"web"|
|remoteIP|string|Not used.|"NA"|
|screenOrientation|string|Not used.|"NA"|
|sdkVersion|string|The version of the internal library.|"4.3"|
|session_id|string|A unique session identifier.|"a5026251"|
|state|string|Indicates whether the browser is online.|"CONNECTED" or "DISCONNECTED"|
|userIdentifier|string|Not used.|"NA"|
|uuid|UUID|A random identifier that tracks the user uniqueness.|"b1da8edd-6eb5-4620-8fed-6f2e01e2800f"|

- 추가필드

<table>
<tr><td rowspan=2>Event</td><td rowspan=2>Source Type</td><td rowspan=2>Description</td><td colspan=3>Data (along with common fields)</td></tr>
<tr><td>Field</td><td>Type</td><td>Description</td></tr>

<tr><td rowspan=2>Session start</td><td rowspan=2>mint:ping</td><td  rowspan=2>Each ping event indicates that a new session has started.</td><td>fsEncrypted</td><td>N/A</td><td>Not used, always "NA"</td></tr>
<tr><td>rooted</td><td>N/A</td><td>Not used, always false</td></tr>

<tr><td>Session end</td><td>mint:gnip</td><td>Each gnip event indicates that a session has ended.</td><td>ses_duration</td><td>int</td><td>How long the session lasted.</td></tr>

<tr><td rowspan=8>Page views</td><td rowspan=8>mint:view</td><td rowspan=8>Triggered once per page view in the app.</td><td>current</td><td>string</td><td>The URL of the current web page, without the hostname.</td></tr>
<tr><td>currentView</td><td>string</td><td>Not used. Hardcoded to "examples".</td></tr>
<tr><td>domProcessingTime</td><td>int</td><td>Time spent to process the domain.</td></tr>
<tr><td>domLookupTime</td><td>int</td><td>Time spent to look up the domain name.</td></tr>
<tr><td>elapsedTime</td><td>int</td><td>Time spent to render the page.</td></tr>
<tr><td>loadTime</td><td>int</td><td>Time spent to load the page.</td></tr>
<tr><td>previous</td><td>string</td><td>The referrer URL.</td></tr>
<tr><td>serverTime</td><td>int</td><td>Time spent to get a response from the server.</td></tr>
<tr><td rowspan=2>App performance and configuration</td><td rowspan=2>mint:log</td><td rowspan=2>Usage and performance logs for the Add-on Builder that track dashboard memory usage, dashboard loading times, the number of accounts, inputs, and regions configured in the app, and non-sensitive input configuration parameters.</td><td>level</td><td>int</td><td>Log level. For example, 60 means "error".</td></tr>
<tr><td>log_name</td><td>any</td><td>Log content. See examples below.</td></tr>
<tr><td rowspan=7>API calls</td><td rowspan=7>mint:network</td><td rowspan=7>XMLHTTPRequest calls, usually HTTP API calls from client side (browser) to the Splunk server.</td><td>failed</td><td>boolean</td><td>Indicates whether the request failed.</td></tr>
<tr><td>latency</td><td>int</td><td>Time spent before the response was received.</td></tr>
<tr><td>protocol</td><td>string</td><td>Network protocol (http or https).</td></tr>
<tr><td>requestLength</td><td>string</td><td>N/A. Not used.</td></tr>
<tr><td>responseLength</td><td>int</td><td>The size of the response.</td></tr>
<tr><td>statusCode</td><td>string</td><td>HTTP response code.</td></tr>
<tr><td>url</td><td>string</td><td>The request URL, without the hostname.</td></tr>

</table>

#### 수집되지 않은 데이터

- 사용자 이름이나 암호와 같은 중요한 데이터.
- 주소, 전화 번호, IP 주소 또는 호스트 이름과 같은 정보 식별.
- Splunk 플랫폼 인스턴스의 인덱싱 된 데이터.

#### 데이터 수집을 선택 또는 해제하는 방법

애드온 빌더를 처음 실행할 때 애드온 빌더는 opt-in request을 표시
(Splunk Add-on Builder) Configuration> Make Add-on Builder Better 탭 언제든지 설정을 변경


### 빌드전에 알아야 할 것

- 데이터에 익숙하고 추출 할 데이터를 알고 있어야합니다.
- 데이터 수집 방법을 고려.(파일 모니터, 네트워크 수신기 또는 HTTP Event Collector를 사용하려는 경우 modular inputs 빌드할 필요가 없으며 input options requirement 생략)
- modular inputs을 만들려는 경우, 시스템계정정보를 알고 있어야 함
- Data model의 어느 부분에 데이터를 매핑할지 파악

애드온을 빌드하는 것은 데이터를 Splunk에 보내고, 데이터를 Splunk로 가져 와서 필요한 필드를 추출하고, 데이터를 데이터 모델에 매핑하고, 경고 작업을 생성하는 모듈 식 입력 생성을 포함 할 수 있습니다. 이러한 개념을 이해하면 추가 기능을 만드는 데 도움이됩니다.

#### modular inputs 이해하기

일반적으로 Python 또는 Java로 작성된 모듈 식 입력을 사용하면 기본 Splunk 데이터 수집 입력이 사용자의 요구를 충족시키지 못할 때 프로그래밍 방식으로 데이터를 Splunk로 가져 오는 방법을 만듬

#### 필드 추출 이해

Splunk Enterprise는 데이터를 인덱싱 할 때 데이터 스트림을 일련의 이벤트로 구문 분석합니다. 이 프로세스의 일부로 Splunk는 이벤트 데이터에 여러 필드를 추가합니다. 이 필드에는 자동으로 추가되는 기본 필드와 사용자가 지정한 사용자 정의 필드가 포함됩니다. Splunk Add-on Builder를 사용하면 데이터에 사용자 정의 필드를 추가하고 인덱스 및 / 또는 검색시 필드 매핑을 수행 할 수 있습니다.

필드 및 필드 추출에 대한 자세한 내용은 다음을 참조하십시오.

- Splunk Enterprise Getting Data In 매뉴얼에서 인덱스 시간에 사용자 정의 필드를 생성하십시오.
- Splunk Enterprise에서 구조화 된 데이터가있는 파일의 필드 추출하기 데이터 가져 오기 매뉴얼

#### 데이터 모델의 이해

버전 2.2.0부터 Splunk 애드온 빌더는 CIM (Common Information Model)을 비롯한 일반 데이터 모델과 일치하도록 다양한 소스 또는 공급 업체의 데이터를 표준화합니다.

- CIM에는 22 개의 미리 정의 된 데이터 모델이 있습니다. 데이터를 이러한 표준과 일치 시키려면 검색시 적용되는 데이터 모델 매핑을 데이터에 추가 할 수 있습니다. CIM에 대한 자세한 내용은 Splunk 일반 개요를 참조하십시오.

#### 경고 동작 이해

경고는 특정 이벤트를 실시간으로 또는 일정에 따라 모니터하고 응답합니다. 특정 조건이 충족되면 경고가 트리거되고 경고 조치가 실행됩니다. Splunk Enterprise에는 스크립트 실행, 전자 메일 전송 또는 경보에 대한 응답으로 이벤트 로깅과 같은 미리 구성된 여러 가지 경보 작업이 포함되어 있습니다. 애드온 빌더를 사용하면 애드온 사용자에게 Splunk Enterprise에서 사용할 수있는 고유 한 알림 작업을 만들 수 있습니다. 경고 작업은 타사 통합을 정의하거나, 사용자 지정 기능을 추가하거나, [Splunk Enterprise Security](http://docs.splunk.com/Documentation/ES/5.0.0/User/Overview)에 대한 적응 형 대응 작업을 포함 할 수 있습니다.

경고 작업 생성에 대한 자세한 내용은 Splunk 웹용 개발 뷰 및 앱 웹 설명서의 사용자 정의 경고 동작 개요를 참조하십시오.

엔터프라이즈 보안에 대한 적응 형 대응과 호환되는 경고 동작을 만드는 방법에 대한 자세한 내용은 다음을 참조하십시오.

- 일반 작업 모델을 사용하여 일반 정보 모델 추가 기능 설명서에서 사용자 지정 경고 작업 만들기
- Splunk 개발자 포털에서 적응 형 대응 작업 생성