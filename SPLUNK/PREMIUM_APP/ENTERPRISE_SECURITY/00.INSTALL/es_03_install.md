# 설치

## Splunk Enterprise 설치

Splunk Enterprise Security를 사내 검색 헤드에 설치합니다. Splunk Cloud 고객은 Splunk 서포트와 협력하여 Enterprise Security 검색 헤드에 액세스해야 합니다.

### 설치 전제 조건

- Splunk Enterprise Security 사용을 위한 Splunk 플랫폼 요구 사항을 검토합니다. 배포 계획을 참조하십시오.
- Splunk Enterprise Security에 포함된 앱이나 추가 기능을 배포 서버에서 관리하는 경우, 배포 서버에 대한 참조가 포함된 deploymentclient.conf 파일을 제거하고 Splunk 서비스를 다시 시작합니다. 이 작업을 수행하지 않으면 설치가 완료되지 않습니다.
- 사용자 계정에 관리자 역할 및 edit_local_apps 기능이 있어야 합니다. 관리자 역할에는 기본적으로 이 기능이 배정됩 니다.

#### 1 단계. Splunk Enterprise Security 다운로드

1. Splunk.com 사용자 이름과 암호를 사용하여 splunk.com에 로그인합니다.
2. 최신 Splunk Enterprise Security 제품을 다운로드합니다. 라이선스가 있는 Enterprise Security 고객만 제품을 다운로드할 수 있습니다.
3. 다운로드를 다운로드 클릭하고 Splunk Enterprise Security 제품 파일을 데스크톱에 저장합니다.
4. 검색 헤드에 관리자 권한으로 로그인합니다.

#### 2 단계. Splunk Enterprise Security 설치

1. Splunk 도구 모음에서 앱 > 앱 관리관리를 선택하고 파일에서 파일에서 앱 설치설치를 클릭합니다.
2. 파일파일 선택선택을 클릭하고 Splunk Enterprise Security 제품 파일을 선택합니다.
3. 업로드를 업로드 클릭하여 설치를 시작합니다.
4. 지금지금 설치설치를 클릭하여 Splunk Enterprise Security를 설치하기 시작합니다.

#### 3 단계. Splunk Enterprise 보안 설치

1. 시작시작을 클릭합니다.
2. 설치 단계가 진행됨에 따라 Splunk Enterprise Security 설치 후 설정 페이지에 상태가 표시됩니다.
3. 선택된 추가 기능을 설치에서 제외할지 설치하고 비활성화할지 선택합니다. 설치가 완료되면 Splunk 플랫폼 서비스를 다시 시작하라는 메시지가 페이지에 표시됩니다.
4. Splunk 다시 시작을 클릭하여 설치를 마칩니다.

Enterprise Security를 설치하면 검색 헤드에서 SSL이 활성화됩니다. ES 설치 후에
https를 사용하여 검색 헤드에 액세스 하려면 Splunk Web URL을 변경해야 합니다.

설치 완료 후 \<code\>\<font size="2"\>$SPLUNK_HOME/var/log/splunk/essinstaller2.log\</font\>\</code\> 설치 로그를 검토합니다 $SPLUNK_HOME/var/log/splunk/essinstaller2.log

#### 4 단계. Enterprise Security 설정

Splunk Enterprise Security를 계속 설정하려면 다음을 참조하십시오.

1. Splunk Enterprise Security에 포함된 추가 기능 배포
2. 인덱스 설정 및 배포
3. 사용자 및 역할 설정
4. 데이터 모델 설정

Enterprise Security 데이터 원본 및 컬렉션 고려 사항의 개요는 데이터 원본 계획을 참조하십시오.

### 명령줄에서 명령줄에서 Splunk Enterprise Security 설치

Splunk 소프트웨어 명령줄을 사용하여 Splunk Enterprise Security를 설치합니다. Splunk 소프트웨어 명령줄에 대한 자세 한 내용은 CLI를 참조하십시오.

1. 1 단계: Splunk Enterprise Security 다운로드의 설명에 따라 Splunk Enterprise Security를 다운로드하고 검색 헤드에 저장합니다.
2. 검색 헤드에서 설치 프로세스를 시작합니다. 2 단계: Splunk Enterprise Security 설치의 설명을 따르거나 REST 호출을 수행하여 서버 명령줄에서 설치를 시작합니다.
    예: curl -k -u admin:password https://localhost:8089/services/apps/local -d filename="true" -d name="\<file name. and directory\>" -d update="true" -v
3. 검색 헤드에서 Splunk 소프트웨어 명령줄을 사용하여 다음 명령어어를 실행합니다.
    splunk search '| essinstall' -auth admin:password Splunk Web에서 이 검색 명령어어를 실행하고 검색 결과로 설치 진행 상태를 확인할 수도 있습니다.
    | essinstall
4. (선택 사항) 추가 옵션을 사용하여 설치할 추가 기능을 지정하거나 설치를 건너뛰거나 설치 후 비활성화할 수 있습니다.
    | essinstall --install-ta \<ta-name\>+ --skip-ta \<ta-name\>+ --disable-ta <ta-name>+

    설치하거나 건너뛰거나 비활성화할 추가 기능의 이름을 지정하거나 *를 와일드카드로 사용합니다. 설치할 여러 추가 기능을 지정하려면 +를 사용하십시오.

### 검색 헤드 클러스터에 설치

Splunk Enterprise Security에는 검색 헤드 클러스터링 구현에 관한 구체적인 요구 사항과 프로세스가 있습니다.

- 검색 헤드 클러스터링에 대한 개요는 분산 검색 매뉴얼에서 검색 헤드 클러스터링 아키텍처를 참조하십시오.
- 검색 헤드 클러스터링 요구 사항이 나열된 전체 리스트는 분산 검색 매뉴얼에서 검색 헤드 클러스터에 대한 시스템 요 구 사항 및 기타 배포 고려 사항을 참조하십시오.

스테이징 인스턴스를 사용하여 주 배포노드의 Enterprise Security를 준비합니다. 사용 가능한 스테이징 인스턴스가 없는 경우, 다른 앱이 설치되어 있지 않은 테스트 또는 QA Splunk Enterprise 인스턴스를 사용할 수 있습니다. 스테이징 인스턴 스를 프로덕션 인덱서 또는 검색 피어에 연결하면 안 됩니다. 스테이징 인스턴스를 설정 변경 및 업그레이드에 사용하십시오.

Enterprise Security를 검색 헤드 클러스터에 설치하는 방법:

1. 스테이징 인스턴스를 준비합니다.
2. Enterprise Security를 스테이징 인스턴스에 설치합니다.
3. Enterprise Security 설치본을 주 배포노드로 마이그레이션합니다. Splunk Enterprise Security Suite와 연결된 앱,SA, DA, TA를 스테이징 인스턴스의 $SPLUNK_HOME/etc/apps에서 주 배포노드의 $SPLUNK_HOME/etc/shcluster/apps로 복사합니다. 검색 앱 같은 기본앱이 포함되면 안되므로 전체 폴더를 복사하지 마십시오.
4. 주 배포노드를 사용하여 Enterprise Security를 클러스터 구성원에 배포합니다.

#### 검색 헤드 클러스터에서 설정 변경 사항 관리

일부 시스템 설정 변경 사항은 주 배포노드를 사용하여 배포해야 합니다.

1. 변경 사항을 검색 헤드 클러스터 구성원에서 적용하지 말고, 스테이징 인스턴스에서 대신 적용합니다.
2. 스테이징 인스턴스에서 설정 변경 사항을 테스트합니다.
3. 필요한 파일을 검색 헤드 클러스터 주 배포노드로 마이그레이션합니다.
4. 업데이트된 설정을 검색 헤드 클러스터에 배포합니다.

주 배포노드를 사용하여 배포해야 하는 설정 변경 사항:

|설정 변경 사항|수정되는 파일|
|:--:|:--:|
|일반 설정 페이지에서 인덱싱된 실시간 검색 활성화 또는 비활성화|inputs.conf|
|일반 설정 페이지에서 인덱싱된 실시간 디스크 동기화 지연 수정|inputs.conf|
|UBA 설정 페이지에서 주요 이벤트를 Splunk UBA로 전송|outputs.conf|

검색 헤드 클러스터에서 변경하는 대부분의 설정은 다른 검색 헤드 클러스터 구성원에 자동으로 복제됩니다. 예:

- 위협 인텔리전스 소스 추가, 수정, 비활성화
- 자산 및 ID 소스 리스트 추가, 수정, 비활성화
- 사용자 인터페이스 변경
- 검색 변경

분산 검색 매뉴얼에서 설정 변경 사항이 전체 검색 헤드 클러스터에 적용되는 방법을 참조하십시오.

#### 기존 배포 마이그레이션

Enterprise Security 검색 헤드나 검색 헤드 풀 구성원을 검색 헤드 클러스터에 바로 추가할 수 없습니다. 검색 헤드나 검색 헤드 풀 구성원을 검색 헤드 클러스터로 마이그레이션하려면 새 검색 헤드 클러스터를 만들고 최신 Enterprise Security 버 전을 이 클러스터에 배포해야 합니다.

검색 헤드 클러스터가 Enterprise Security를 실행하게 된 후에는 이전 Enterprise Security 설치본의 사용자 지정 설정을 수 동으로 검토하고 새 검색 헤드 클러스터의 주 배포노드로 마이그레이션하여 변경 사항을 클러스터 구성원에 복제해야 합니다.

자세한 내용은 Splunk Enterprise 분산 검색 매뉴얼에서 독립형 검색 헤드에서 검색 헤드 클러스터로 마이그레이션 항목을 참조하십시오.

Splunk Enterprise Security 배포 마이그레이션을 계획하는 데 도움이 필요한 경우 Splunk 프로페셔널 서비스에 문의하십시오.

## Splunk Enterprise Security에 포함된 추가 기능 배포

Splunk Enterprise Security 패키지에는 추가 기능 세트가 포함되어 있습니다.

- 이름에 "SA-" 또는 "DA-"가 있는 추가 기능은 Splunk Enterprise Security 프레임워크를 구성합니다. 이런 추가 기능 의 설치 및 설정은 Splunk Enterprise Security 설치 프로세스의 일부로 처리되므로, 해당 기능을 배포하거나 설정하 기 위해 추가 작업을 수행하지 않아도 됩니다. Splunk Enterprise Security 프레임워크를 구성하는 추가 기능을 비활 성화하지 마십시오.
- 나머지 추가 기능은 이름에 "TA-"가 있는 기술별 추가 기능으로, 해당 원본 데이터를 Enterprise Security에 통합하기 위해 필요한 CIM 호환 지식을 제공합니다.

다양한 유형의 추가 기능이 Splunk Enterprise Security와 상호 작용하는 방법에 대한 자세한 내용은 Splunk 개발자 포털에 서 ES 솔루션 아키텍처를 참조하십시오. 기술별 추가 기능은 Splunk Enterprise Security 프레임워크를 구성하는 추가 기 능과 다르게 지원됩니다. 릴리스 노트 매뉴얼의 Splunk Enterprise Security 및 제공된 추가 기능 지원을 참조하십시오.

기술 추가 기능을 배포하는 방법은 Splunk 플랫폼 배포 아키텍처에 따라 다릅니다.

전제 조건

Splunk Enterprise Security를 검색 헤드 또는 검색 헤드 클러스터에 설치하십시오. Enterprise Security 설치를 참조하십 시오 Splunk Enterprise Security를 분산 환경에 설치하는 경우, 설치 프로그램에서는 Enterprise Security 패키지에 포함 된 추가 기능을 검색 헤드나 검색 헤드 클러스터에 설치하고 활성화합니다.

절차

1. 포워더에 설치할 추가 기능 결정
2. 포워더에 추가 기능 배포
3. 인덱서에 추가 기능 배포

### 포워더에 설치할 추가 기능 결정

포워더에서 데이터를 수집하는 추가 기능을 설치합니다. 추가 기능에 대한 매뉴얼을 검토하여 포워더에 설치할 추가 기능과 각 추가 기능에 필요한 포워더 설정 유형을 결정합니다.

대부분의 추가 기능에는 특정 데이터 원본에 대한 입력 설정이 포함되어 있습니다. 추가 기능에 포함된 inputs.conf를 검토하 고 필요에 따라 추가 기능을 포워더에 배포합니다. 일부 추가 기능은 데이터 원본 시스템에 직접 설치된 포워더에 배포해야 합니다. 나머지 추가 기능에는 헤비 포워더가 필요합니다. 구체적인 설명은 각 추가 기능에 대한 매뉴얼이나 README 파일 을 참조하십시오.

- 웹 기반 매뉴얼이 있는 추가 기능의 경우, 아래 링크를 사용하여 설치 및 설정 위치를 확인하십시오.
- 웹 기반 매뉴얼이 없는 추가 기능의 경우, 추가 기능의 루트 폴더에 있는 README 파일을 참조하십시오.

### 포워더에 추가 기능 배포

Splunk 추가 기능 매뉴얼에서 분산 Splunk Enterprise 배포에 추가 기능 설치를 참조하십시오.

#### Enterprise Security와 함께 제공되는 기술별 추가 기능

Splunk Enterprise Security에는 보안과 관련된 다음과 같은 CIM 호환 기술 추가 기능이 포함되어 있습니다.

- Blue Coat ProxySG용 Splunk 추가 기능
- Bro IDS용 Splunk 추가 기능
- McAfee용 Splunk 추가 기능
- Juniper용 Splunk 추가 기능
- Microsoft Windows용 Splunk 추가 기능
- Tenable용 Splunk 추가 기능
- NetFlow용 Splunk 추가 기능
- Oracle Database용 Splunk 추가 기능
- OSSEC용 Splunk 추가 기능
- RSA SecurID용 Splunk 추가 기능
- Sophos용 Splunk 추가 기능
- FireSIGHT용 Splunk 추가 기능
- Symantec Endpoint Protection용 Splunk 추가 기능
- UBA용 Splunk 추가 기능
- Unix 및 Linux용 Splunk 추가 기능
- Websense Content Gateway용 Splunk 추가 기능
- TA-airdefense
- TA-alcatel
- TA-cef
- TA-fortinet
- TA-ftp
- TA-nmap
- TA-tippingpoint
- TA-trendmicro

### 인덱서에 추가 기능 배포

Splunk는 Splunk 지원 추가 기능을 전체 Splunk 플랫폼 배포에 설치한 다음 필요한 경우에만 입력을 활성화하고 설정할 것 을 권장합니다. 자세한 내용은 Splunk 추가 기능 매뉴얼에서 Splunk 추가 기능 설치 위치를 참조하십시오.

인덱서에 추가 기능을 배포하는 데 사용하는 절차는 Splunk 플랫폼 배포에 따라 다를 수 있습니다. 상황이나 기호에 맞는 옵 션을 선택하십시오.

<table>
<tr><td></td></tr>
배포 상황 절차
Splunk Enterprise Security가 Splunk Cloud에서 실행 중인 경우
Splunk 서포트에 연락하여 필수 추가 기능을 인덱서에 설치해 달라고 요청합 니다.
추가 기능을 인덱서에 수동으로 배포하는 방법을 선호하는 경우
분산 Splunk Enterprise 배포에 추가 기능 설치를 참조하십시오.
인덱서가 클러스터링되고, 클러스터 마스터를 사용하여 추가 기능을 사내 Splunk 플랫폼 설치본의 클러스터 피어에 배포하고, 배포에 복잡한 문제가 더 없는 경우
Splunk_TA_ForIndexers를 만들고 수 동으로 배포 관리
인덱서가 클러스터링되지 않고, 배포 서버를 사용하여 사내 Splunk 플랫폼 설 치본의 인덱서 설정을 관리하고, 배포에 복잡한 문제가 더 없는 경우
Splunk_TA_ForIndexers를 만들고 자 동 배포 설정
Splunk Enterprise Security가 예를 들어 모두 동일한 인덱서 집합을 사용하는 인덱서에 추가 기능을 배포하는 데 도
13
Enterprise Security 검색 헤드와 나머지 검색에 사용되는 검색 헤드 하나로 구 성된 복잡한 배포 환경에서 실행되는 경우
움이 필요한 경우 Splunk 프로페셔널 서비스에 문의하십시오.
Splunk_TA_ForIndexers를 만들고 만들고 수동으로 수동으로 배포배포 관리관리
이 절차는 Splunk Cloud가 아닌 Splunk Enterprise에서 Splunk Enterprise Security가 실행 중이고, 인덱서가 클러스터링 되고, 배포에 복잡한 문제가 더 없는 경우에만 사용하십시오. 이 절차가 배포 상황에 맞지 않는 경우 인덱서에 필수 추가 기능 배포를 참조하여 다른 배포 방법을 선택하십시오.
분산 설정 관리에서는 인덱스 시간 설정과 기본 인덱스 정의를 Splunk_TA_ForIndexers 패키지에 수집하여 추가 기능 설정 을 사내 인덱서에 배포하는 작업을 간소화합니다. Splunk_TA_ForIndexers는 검색 헤드에 활성화되어 있는 모든 앱과 추가 기능의
indexes.conf
및 인덱스 시간
props.conf
및
transforms.conf
설정을 모두 포함하고, 단일
indexes.conf
,
props.conf
및
transforms.conf
파일로 병합하고, 파일을 다운로드할 수 있게 추가 기능 하나에 저장합니다. 이 작동 방식은
./splunk cmd
btool <conf_file_prefix> list
출력과 비슷합니다.
이 절차에서는 검색 헤드에 활성화되어 있는 모든 추가 기능을 인덱서에 배포합니다. 인덱서에 배포하는 추가 기능을 인덱서 에 반드시 필요한 부분 집합으로만 제한하려면 이 절차를 시작하기 전에 앱 > 앱 관리관리를 선택하고 인덱서에 필요하지 않은 추가 기능을 모두 비활성화한 다음 절차를 마친 후에 다시 활성화하십시오.
Splunk_TA_ForIndexers를 배포하기 전에, 인덱서에 설치된 기존 추가 기능이 Splunk_TA_ForIndexers 패키지에 포함되 어 있지 않은지 확인하십시오. 같은 추가 기능을 두 번 배포하면 특히 추가 기능의 버전이 서로 다른 경우 설정이 충돌할 수 있습니다.
1. Enterprise Security 메뉴 모음에서 설정설정 > 일반일반 > 분산분산 설정설정 관리관리를 선택합니다. 2. 패키지 패키지 다운로드를 다운로드 클릭합니다. 3. 패키지의 콘텐츠를 선택합니다. 패키지를 다운로드하려면 다음 옵션 중 하나 이상을 선택해야 합니다.
1. (선택 사항)
props.conf
및
transforms.conf
파일을 패키지에 포함하려면 인덱스 인덱스 시간시간 특성특성 포함포함 체크박스를 선 택합니다. 2. (선택 사항)
indexes.conf
파일을 패키지에 포함하려면 인덱스 인덱스 정의정의 포함포함 체크박스를 선택합니다. 4. 패키지 패키지 다운로드를 다운로드 클릭하여
Splunk_TA_ForIndexers
를 만들고 다운로드합니다. 5. 추가 기능을 다운로드한 후에 패키지의 콘텐츠를 수정할 수 있습니다.
예를 들어 사이트 보존 설정 및 기타 스토리지 옵션을 준수하도록
indexes.conf
를 수정합니다. 6. 클러스터 마스터를 사용하여 Splunk_TA_ForIndexers나 추가 기능을 클러스터 피어에 배포합니다. 인덱서 및 인덱
서 클러스터 관리에서 모든 피어에서 공통 설정 관리와 모든 피어에서 앱 배포 관리를 참조하십시오.
Enterprise Security와 함께 사용할 새 추가 기능을 설치하는 경우, 위 절차를 반복하여 업데이트된
Splunk_TA_ForIndexers 버전을 만드십시오.
Splunk_TA_ForIndexers를 만들고 만들고 자동자동 배포배포 설정설정
이 절차는 Splunk Enterprise Security가 Splunk Enterprise에서 실행 중이고, 인덱서가 클러스터링되지 않고, 배포에 복잡 한 문제가 더 없는 경우에만 사용하십시오. 이 절차가 배포 상황에 맞지 않는 경우 인덱서에 필수 추가 기능 배포를 참조하여 다른 배포 방법을 선택하십시오.
분산 설정 관리에서는 인덱스 시간 설정과 기본 인덱스 정의를 Splunk_TA_ForIndexers 패키지에 수집하여 추가 기능 설정 을 사내 인덱서에 배포하는 작업을 간소화합니다. 자동 배포 옵션을 선택하면 분산 설정 관리에서 검색 헤드에 활성화된 모 든 앱과 추가 기능의 인덱스 시간
props.conf
및
transforms.conf
설정을 모두 포함시키고, 단일
props.conf
및 transforms.conf
파일로 병합하고, 파일을 Splunk_TA_ForIndexers에 저장하여 자동 배포합니다. 모든 인덱서의 인덱서 저 장 및 보존 설정이 같은 경우,
indexes.conf
설정을 패키지에 추가할 수 있습니다.
이 절차에서는 검색 헤드에 활성화되어 있는 모든 추가 기능을 인덱서에 배포합니다. 인덱서에 배포하는 추가 기능을 인덱서 에 반드시 필요한 부분 집합으로만 제한하려면 이 절차를 시작하기 전에 '''앱 > 앱 관리'''를 선택하고 인덱서에 필요하지 않은 추가 기능을 모두 비활성화한 다음 절차를 마친 후에 다시 활성화하십시오.
Splunk_TA_ForIndexers를 배포하기 전에, 인덱서에 설치된 기존 추가 기능이 Splunk_TA_ForIndexers 패키지에 포함되 어 있지 않은지 확인하십시오. 같은 추가 기능을 두 번 배포하면 특히 추가 기능의 버전이 서로 다른 경우 설정이 충돌할 수 있습니다.
1. Splunk Enterprise Security 검색 헤드를 배포 서버의 배포 클라이언트로 설정합니다. Splunk Enterprise 인스턴스
업데이트에서 배포 클라이언트 설정을 참조하십시오. 2. Enterprise Security 메뉴 모음에서 설정설정 > 일반일반 > 분산분산 설정설정 관리관리를 선택합니다. 3. 자동자동 배포를 배포를 사용하시겠습니까?에서 사용하시겠습니까 예를 선택합니다. 4. 새 자격자격 증명증명 추가추가를 선택하여 배포 서버에 사용할 Splunk 관리 계정을 추가합니다. 관리 계정에는 배포 서버 관리자
역할이 있어야 합니다.
1. 계정 사용자와 사용자 암호암호를 입력합니다. 2. 애플리케이션을 애플리케이션 SplunkEnterpriseSecuritySuite로 설정합니다. 3. 계정 자격 증명을 저장합니다. 5. 자격자격 증명증명 선택선택을 클릭하고 4 단계에 추가한 자격 증명을 선택합니다. 6.
Splunk_TA_ForIndexers
추가 기능을 수신할 수 있는 인덱서를 선택합니다.
14
7. (선택 사항) Splunk 인덱서 인덱서 선택선택 필드에 인덱서 이름을 더 입력하여 추가합니다. 8. (선택 사항) indexes.conf 푸시푸시 체크박스를 선택하여
indexes.conf
설정을
Splunk_TA_ForIndexers
추가 기능 패키지에 포함시킵니다. 인덱스 설정에는 저장소별 설정이 필요할 수 있으므로
indexes.conf
가 패키지에 기본적으로 포함되어 있지 않습니다.
indexes.conf
를
Splunk_TA_ForIndexers
와 함께 배포하지 않는 경우, 인덱스 설정을 수동으로 관리하십 시오. 9. 저장저장을 클릭하여
Splunk_TA_ForIndexers
추가 기능을 만듭니다.
자동 배포를 설정한 후에
Splunk_TA_ForIndexers
자동 배포를 비활성화하면
Splunk_TA_ForIndexers
추가 기능이 배포 서버에 계속 남아 있습니다. 추가 기능과 serverclass를 수동으로 제거하십시오.
Splunk Enterprise Security로 앱과앱과 추가추가 기능기능 가져오기 가져오기 앱과 추가 기능으로 Splunk Enterprise Security의 기능을 확장할 수 있습니다. SplunkBase에서 앱과 추가 기능을 다운로 드하거나 Splunk 추가 기능 빌더 같은 도구를 사용하여 추가 기능을 직접 만드십시오. Splunk Cloud 고객이 추가 기능을 검 색 헤드에 설치하려면 Splunk 서포트와 협력해야 합니다.
업데이트 업데이트 ES 모듈식 모듈식 입력입력 사용사용
Splunk Enterprise Security는 같은 검색 헤드에 설치된 앱과 추가 기능의 설정을 통합합니다. 업데이트 ES 모듈식 입력은 정규식 필터와 일치하는 모든 앱과 추가 기능을 가져오는 역할을 수행합니다. 필터는 앱 경로인
SplunkEnterpriseSecuritySuite/default/inputs.conf
에서 정의됩니다.
모듈식 모듈식 입력입력 함수함수
app_imports_update://update_es 추가 기능 지원에 대한 메타데이터를 가져오고 업데이트합니다.
app_imports_update://update_es_da 도메인 추가 기능에 대한 메타데이터를 가져오고 업데이트합니다.
app_imports_update://update_es_main
SplunkEnterpriseSecuritySuite에 니다.
대한 메타데이터를 가져오고 업데이트합
가져오기는 가져오기는 전이적입니다. 전이적입니다 앱 가져오기는 전이적입니다. 즉, 다른 앱(B)을 가져오는 앱(A)은 다른 앱이 가져오는 모든 앱(C)도 가져옵니다.
1. 앱 A가 B를 가져오고, 2. 앱 B가 C를 가져오는 경우, 3. A가 C를 가져옵니다.
지원 추가 기능은 서로 가져오므로, 업데이트된
local.meta
파일이 있는 지원 추가 기능이 하나만 보일 수 있습니다. 이 기능 은
SA-AccessProtection
입니다. 앱 리스트의 첫 지원 추가 기능이기 때문입니다.
기존기존 앱 가져오기 가져오기 보기보기
|rest
검색 명령어어를 사용하여 기존 앱 가져오기를 봅니다. 이 명령어어를 실행하려면 Splunk 관리자 권한이 있어야 합니 다. 예를 들어 관리자 사용자로 인증되어 있는 동안
SplunkEnterpriseSecuritySuite
앱의 가져오기를 보려면 다음 명령어어를 사용하십시오.
| rest /servicesNS/admin/system/apps/local/SplunkEnterpriseSecuritySuite/import splunk_server=local | fields import
앱 및 추가추가 기능기능 가져오기 가져오기 명명명명 규칙규칙
모듈식 입력은 다음이 앞에 오는 앱과 추가 기능을 자동으로 가져옵니다.
DA-ESS-
,
SA-
,
TA-
,
Splunk_SA_
,
Splunk_TA_
, 및
Splunk_DA-ESS_
.
명명명명 규칙이 규칙이 다른다른 추가추가 기능기능 가져오기 가져오기 사용자 지정 추가 기능에 일반적인 ES 명명 규칙이 사용되지 않는 경우, 이름이나 명명 규칙을 가져오기 모듈식 입력에 추가 해야 합니다.
1. Enterprise Security 도구 모음에서 설정설정 > 일반일반으로 이동하여 앱 가져오기 가져오기 업데이트를 업데이트 선택합니다. 2.
update_es
입력을 편집합니다. 3. 정규식을 사용해 추가 기능의 명명 규칙을 지원되는 명명 규칙 리스트에 추가하여 애플리케이션 애플리케이션 정규식 정규식 필드를 업데
이트합니다.
1. 예를 들어 이름이
My_datasource
인 새로운 추가 기능을 가져오려면 애플리케이션 애플리케이션 정규식 정규식 필드를
(appsbrowser)|(search)|([ST]A-.*)|(Splunk_[ST]A_.*)|(DA-ESS-.*)|(Splunk_DA-ESS_.*)|(My_datasource)
로 업데 이트합니다.
15
2. 애플리케이션 애플리케이션 정규식 정규식 필드를 변경하는 경우, 항상 기본 정규식을 추가합니다. 그러지 않으면 기존 앱 가져오기
가 실패합니다. 4. 저장합니다. 5. 변경 사항을 미리 봅니다.
|rest services/data/inputs/app_imports_update | table title app_regex app_exclude_regex updated
6. Splunk Enterprise 서비스를 다시 시작하여 변경 사항을 통합합니다.
앱 가져오기에서 가져오기에서 추가추가 기능기능 제거제거
추가 기능을 앱 가져오기 프로세스에서 제외합니다.
1. Enterprise Security 도구 모음에서 설정설정 > 일반일반으로 이동하여 앱 가져오기 가져오기 업데이트를 업데이트 선택합니다. 2.
update_es
입력을 편집합니다. 3. 정규식을 사용해 추가 기능의 명명 규칙을 지원되는 명명 규칙 리스트에 추가하여 애플리케이션 애플리케이션 제외제외 정규식 정규식 필드를
업데이트합니다.
1. 예를 들어 이름이
TA_new_test
인 새로운 추가 기능을 제외하려면 애플리케이션 애플리케이션 제외제외 정규식 정규식 필드를
|TA_new_test
로 업데이트합니다. 4. 저장합니다. 5. 변경 사항을 미리 봅니다.
|rest services/data/inputs/app_imports_update | table title app_regex app_exclude_regex updated
6. Splunk Enterprise 서비스를 다시 시작하여 변경 사항을 통합합니다.
Splunk Enterprise Security와 Splunk Stream 통합통합
Enterprise Security는 Splunk Stream과 통합되어 네트워크 트래픽 데이터를 캡처하고 분석합니다. Splunk Stream에는 검색 헤드에 설치하는 앱(splunk_app_stream)과 두 가지 전달 옵션이 포함되어 있습니다.
1. Enterprise Security 검색 헤드에 Splunk App for Stream을 설치합니다.
Splunk Enterprise 배포의 경우, Splunk Stream 설치 및 설정 매뉴얼에서 Splunk Stream 설치를 참조하십시 오. Splunk Cloud 배포의 경우, Splunk Stream 설치 및 설정 매뉴얼에서 Splunk Cloud에 Splunk Stream 배포를 참조하십시오. 2. 사용하는 Splunk Stream 포워더에서 Splunk Enterprise Security 설정 템플릿을 활성화합니다. Stream용 Splunk
추가 기능(Splunk_TA_stream)이나 독립형 Stream 포워더를 사용할 수 있습니다. Stream 설정 템플릿 사용을 참조 하십시오.
Enterprise Security에서에서 Stream 사용사용
Splunk Stream을 설정한 후 상관(correlation)검색의 결과로서 Stream 캡처 작업을 시작할 수 있습니다. Splunk Enterprise Security 관리에서 Splunk Stream을 사용하여 스트림 캡처 시작을 참조하십시오. 인시던트 검토 대시보드의 주 요 이벤트에서 스트림 캡처 작업을 시작할 수도 있습니다. Splunk Enterprise Security 사용에서 Stream 캡처 시작을 참조 하십시오.
프로토콜 인텔리전스 대시보드에서 Splunk Enterprise Security에 캡처된 Stream 데이터 이벤트를 보고 분석할 수 있습니 다. Splunk Enterprise Security 사용에서 프로토콜 인텔리전스 대시보드를 참조하십시오.
인덱스 인덱스 설정설정 및 배포배포
Splunk Enterprise Security는 이벤트를 저장할 사용자 지정 인덱스를 구현합니다. 인덱스는 Splunk Enterprise Security 와 함께 제공되는 여러 앱에서 정의됩니다.
단일 인스턴스 배포에서, Enterprise Security를 설치하면 인덱스를 기본 데이터 저장 경로에 만듭니다. Splunk Cloud 배포에서, 고객은 Splunk 서포트와 협력하여 클라우드 인덱스 매개변수를 설정 및 관리하고 유지합니 다. Splunk Cloud 사용자 매뉴얼에서 Splunk Cloud 인덱스 관리를 참조하십시오. 분산 배포에서, 모든 Splunk 플랫폼 인덱서나 검색 피어에 인덱스를 만듭니다.
인덱스 인덱스 설정설정
Splunk Enterprise Security에서 정의된 인덱스는 다음에 관한 구성 설정을 제공하지 않습니다.
여러 저장 경로 가속된 데이터 모델 데이터 보존 버킷 크기 지정
16
용량 매개변수 사용
자세한 인덱스 설정 예는 Splunk Enterprise 관리자 매뉴얼에서 indexes.conf.example을 참조하십시오.
앱별앱별 인덱스
인덱스 앱 컨텍스트 컨텍스트 인덱스 인덱스 설명설명
DA-ESS-AccessProtection
gia_summary
액세스 이상 대시보드의 지리적으로 가능성이 매우 낮은 액세스 패널 에서 사용되는 요약 인덱스.
ioc
이 릴리스에서는 사용되지 않음 DA-ESS-ThreatIntelligence
threat_activity
위협 리스트 일치의 결과로 발생하는 이벤트 포함
SA-EndpointProtection
endpoint_summary
Endpoint 보호 요약 인덱스
SA-ThreatIntelligence
notable
주요 이벤트 포함
notable_summary
일부 대시보드에서 사용되는 주요 이벤트의 통계 요약 포함
risk
위험 한정자 이벤트 포함
SA-NetworkProtection
whois
WHOIS 데이터 인덱스
cim_summary
이 릴리스에서는 사용되지 않음 Splunk_SA_CIM
cim_modactions
Adaptive response 작업 이벤트 포함
Splunk_SA_ExtremeSearch
xtreme_contexts
극단 검색을 위한 컨텍스트 포함
추가 기능에는
indexes.conf
파일에서 정의된 사용자 지정 인덱스가 포함될 수 있습니다.
인덱스 인덱스 배포배포
Splunk Enterprise Security에는 검색 헤드에 활성화되어 있는 모든 앱과 추가 기능의
indexes.conf
와 인덱스 시간 props.conf
및
transforms.conf
설정을 수집하고 추가 기능 하나로 모으는 도구가 있습니다. 자세한 내용은 이 매뉴얼에서 Splunk Enterprise Security에 포함된 추가 기능 배포를 참조하십시오.
사용자 사용자 및 역할역할 설정설정
Splunk Enterprise Security는 Splunk 플랫폼과 통합된 액세스 제어 시스템을 사용합니다. Splunk 플랫폼 권한을 사용하여 사용자를 추가하고, 사용자를 역할역할에 배정하고, 해당 역할을 사용자 지정 기능기능에 배정하여 세분화된 역할 기반 액세스 제어 를 조직에 제공할 수 있습니다.
Splunk Enterprise Security는 관리자 사용자에 의존하여 저장된 검색을 실행합니다. 관리자 사용자를 삭제할 예정이면 삭 제하기 전에 해당 사용자 소유의 knowledge object를 업데이트하십시오.
Splunk Enterprise의 경우 지식 관리자 매뉴얼의 새 소유자에게 하나 이상의 공유된 knowledge object 재배정을 참 조하십시오. Splunk Cloud의 경우 지식 관리자 매뉴얼의 새 소유자에게 하나 이상의 공유된 knowledge object 재배정을 참조하 십시오.
사용자 사용자 역할역할 설정설정
Splunk Enterprise Security는 Splunk 플랫폼에서 제공하는 기본 역할에 역할을 3개 추가합니다. 새 규칙을 통해 Splunk 관리자는 사용자의 액세스 요구 사항에 따라 ES의 특정 기능에 대한 액세스 권한을 배정할 수 있습니다. Splunk 플랫폼 관 리자는 사용자가 Splunk Enterprise Security에서 수행하고 관리할 작업에 가장 적합한 역할에 사용자 그룹을 배정할 수 있 습니다. 사용자는 3개 범주로 나뉩니다.
사 용 자
Splunk ES 역할역할
보 안 감 독
설명설명
주로 보안 포스처, 보호 센터 및 감사 대시보드를 검토하여 조직의 현재 보안 포스처를 이해하려고 합 니다. 보안 감독은 제품을 설정하거나 인시던트를 관리하지 않습니다.
ess_user
17
보 안 애 널 리 스 트
보안 포스처 및 인시던트 리뷰 대시보드를 사용하여 보안 인시던트를 관리하고 조사합니다. 보안 애널 리스트는 보호 센터를 검토하고 무엇이 보안 인시던트를 구성하는지에 대한 방향을 제시하는 책임도 집니다. 상관(correlation)검색과 대시보드에 사용되는 임계값도 정의합니다. 보안 애널리스트는 상 관(correlation) 검색을 편집하고 제거 필터를 만들 수 있어야 합니다.
ess_analyst
솔 루 션 관 리 자
Splunk 플랫폼 설치본과 Splunk 앱을 설치하고 유지관리합니다. 이 사용자는 워크플로 설정, 새 데이 터 원본 추가, 애플리케이션 튜닝 및 문제 해결을 책임집니다.
또는 sc_admin
각 Splunk Enterprise Security 사용자 지정 역할은 Splunk 플랫폼 역할에서 상속하고 Splunk ES 고유 기능을 추가합니다. 세 가지의 Splunk ES 사용자 지정 역할을 모두 사용자에게 배정할 수 있는 것은 아닙니다.
Splunk ES 역할역할
admin
Splunk 플 랫폼랫폼 역할에 역할에 서 상속상속
추가되는 추가되는 Splunk ES 기능기능 사용자에게 사용자에게 배정배정 가능가능
ess_user
예 ES 사용자의
user
역할을 대 체합니다.
ess_analyst
사용자 실시간 검색, 검색 헤드 클러스터링 나열
user, ess_user, power
ess_user
를 상속하며 주요 이벤트를 만들고 편집하고 소 유하고 모든 전환을 수행하고, 글래스 테이블을 편집하 고, 조사를 만들고 수정할 수 있는 기능을 추가합니다.
예 ES 사용자의
power
역할을 대 체합니다.
ess_admin
user, ess_user, power, ess_analyst
아니요. 아니요 Splunk 플랫폼 관리자 역할을 사용하여 Enterprise Security 설치본을 관리해야 합 니다.
기본적으로 각 역할에 배정되는 기능에 대한 자세한 내용은 Splunk Enterprise Security 관련 기능을 참조하십시오.
Splunk 플랫폼
admin
ess_analyst
를 상속하고 여러 기능을 추가합니다.
역할은 모든 고유 ES 기능을 상속합니다. Splunk Cloud 배포에서, Splunk 플랫폼 관리자 역할의 이름 은
sc_admin
입니다.
admin
또는
sc_admin
역할을 사용하여 Enterprise Security 설치본을 관리하십시오.
Splunk 플랫폼 플랫폼 역할역할 다음다음 역할에서 역할에서 상속상속 추가되는 추가되는 기능기능 사용자 사용자 배정배정 승인승인
admin
user, ess_user, power, ess_analyst, ess_admin 모두 예
sc_admin
user, ess_user, power, ess_analyst, ess_admin 모두 예
역할역할 상속상속
모든 역할 상속은 Enterprise Security에서 미리 설정됩니다. 역할의 기능이 변경되면 다른 상속 역할에 변경 사항이 적용됩 니다. 역할에 대한 자세한 내용은 Splunk 플랫폼 매뉴얼을 참조하십시오.
Splunk Enterprise의 경우, Splunk Enterprise 보안에서 역할 추가 및 편집을 참조하십시오. Splunk Cloud의 경우, Splunk Cloud 사용자 매뉴얼에서 Splunk Cloud 역할 관리를 참조하십시오.
역할에 역할에 기능기능 추가추가
기능은 역할이 Splunk Enterprise Security의 다양한 기능에 액세스할 수 있는 수준을 제어합니다. Enterprise Security의 권한권한 페이지를 사용하여 역할에 배정된 기능을 검토하고 변경하십시오.
1. Splunk Enterprise Security 메뉴 모음에서 설정설정 > 일반일반 > 권한권한을 선택합니다. 2. 업데이트할 역할을 찾습니다. 3. 추가할 ES 구성구성 요소요소를 찾습니다. 4. 역할의 구성 요소에 해당하는 체크박스를 선택합니다. 5. 저장합니다.
Splunk Enterprise Security 관련관련 기능기능
Splunk Enterprise Security는 사용자 지정 기능을 사용하여 Splunk Enterprise Security 관련 기능에 대한 액세스를 제어 합니다.
18
Splunk Enterprise Security의 권한 페이지에서 기능을 추가하여 적절한 액세스 제어 리스트(ACL)가 업데이트되도록 하십 시오. 권한 페이지에서 ACL이 자동으로 변경됩니다. Splunk 플랫폼 설정 페이지에서 이런 사용자 지정 기능을 추가하는 경 우, ACL을 직접 업데이트해야 합니다.
ES의 기능기능 설명설명 기능기능 ess_user ess_analyst
새 주요 이벤 트 만들기
검색 결과로부터 임시 주 요 이벤트 만들기 Splunk Enterprise Security에서 주요 이벤트 수동으로 만 들기를 참조하십시오.
edit_notable_events X
고급 검색 예 약 설정 편집
콘텐츠 관리에서 상관 (correlation) 검색의 예약 우선 순위와 예약 창을 편 집합니다.
edit_search_schedule_priority edit_search_schedule_window
상관 (correlation) 검색 편집
콘텐츠 관리에서 상관 (correlation) 검색을 편집 합니다. Splunk Enterprise Security에서 상관(correlation)검색 설 정을 참조하십시오. 이 기 능을 보유한 사용자는 콘 텐츠 관리에서 콘텐츠를 앱으로 내보낼 수도 있습 니다. Splunk Enterprise Security에서 콘텐츠를 앱으로 내보내기를 참조 하십시오.
edit_correlationsearches schedule_search
분산 설정 관 리 편집
분산 설정 관리를 사용합 니다. Splunk Enterprise Security에 포함된 추가 기능 배포를 참조하십시 오.
edit_modinput_es_deployment_manager
ES 탐색 편 집
Enterprise Security 탐 색을 변경합니다. Splunk Enterprise Security의 메뉴 모음 사용자 지정을 참조하십시오.
edit_es_navigation
글래스 테이 블 편집
글래스 테이블을 만들고 수정합니다. 글래스 테이 블 만들기를 참조하십시 오.
edit_glasstable X
ID 룩업 설정 편집
ID 룩업 설정을 관리하고 자산 및 ID 상관을 제한합 니다. Splunk Enterprise Security에 자산 및 ID 데 이터 추가와 Splunk Enterprise Security에서 자산 및 ID 상관 설정을 참조하십시오.
edit_identitylookup
인시던트 검 토 편집
인시던트 검토 설정을 변 경합니다. Splunk Enterprise Security에서 인시던트 검토 사용자 지 정을 참조하십시오.
edit_log_review_settings
룩업 편집
룩업 테이블 파일을 만들 고 변경합니다. Splunk Enterprise Security에서 룩업 작성 및 관리를 참조 하십시오.
edit_lookups, edit_managed_configurations
19
조사 및 주요 이벤트를 위 해 선택 가능한 주요 이벤 상태 편집
트 상태를 변경합니다. 주
edit_reviewstatuses 요 이벤트 상태 관리를 참 조하십시오.
주요 이벤트 제거 편집
주요 이벤트 제거를 만들 고 편집합니다. 주요 이벤 트 제거 만들기 및 관리를 참조하십시오.
edit_suppressions
주요 이벤트 편집
주요 이벤트를 변경합니 다. 예를 들어 주요 이벤 트를 배정하고 주요 이벤 트의 상태를 전환합니다. 또한 transition_reviewstatus-
X_to_Y
edit_notable_events transition_reviewstatus-X_to_Y
X
패널별 필터 편집
기능을 사용하면 애널리스트가 조사 상태 간을 전환할 수도 있습니 다. Splunk Enterprise Security의 인시던트 검 토에서 주요 이벤트 분류 를 참조하십시오.
역할에 대시보드에서 패 널별 필터를 업데이트할 수 있도록 허용합니다. Splunk Enterprise Security에서 패널별 필 터링 설정을 참조하십시 오.
edit_per_panel_filters
인텔리전스 다운로드 편 집
인텔리전스 다운로드 설 정을 변경합니다. 인터넷 에서 Splunk Enterprise Security로 위협 인텔리 전스 피드 다운로드 및 인 터넷에서 Splunk Enterprise Security로 인텔리전스 피드 다운로 드를 참조하십시오.
edit_modinput_threatlist edit_modinput_threat_intelligence_manager
위협 인텔리 전스 컬렉션 편집
위협 인텔리전스를 업로 드하고 REST API를 사 용하여 위협 인텔리전스 컬렉션에 CRUD 작업을 수행합니다. Splunk Enterprise Security에서 사용자 지정 위협 인텔리 전스 CSV 파일 업로드 및 위협 인텔리전스 API 조회를 참조하십시오.
edit_threat_intel_collections
모든 조사 관 리
역할이 모든 조사를 보고 변경할 수 있게 허용합니 다. Splunk Enterprise Security에서 보안 조사 관리를 참조하십시오.
manage_all_investigations
주요 이벤트 소유
역할이 주요 이벤트의 소 유자가 될 수 있게 허용합 니다. 주요 이벤트 배정을 참조하십시오.
can_own_notable_events X
검색 기반 룩 업
검색에 의해 채워질 수 있 는 룩업 테이블을 만듭니 다. Splunk Enterprise Security에서 검색 기반 룩업 만들기를 참조하십
edit_managed_configurations schedule_search
20
시오.
조사 관리
조사를 만들고 편집합니 다. 이 기능이 있는 역할 은 협력 인원 권한이 있는 조사를 변경할 수 있습니 다. Splunk Enterprise Security의 조사를 참조 하십시오.
edit_timelines X
자격 증명 관 리자
Splunk Enterprise Security와 기타 앱의 자 격 증명 및 인증서를 관리 합니다. 권한 페이지에서 설정할 수 없습니다. Splunk Enterprise Security에서 자격 증명 관리를 참조하십시오.
admin_all_objects list_storage_passwords list_app_certs edit_app_certs delete_app_certs
역할의 역할의 동시동시 검색검색 조정조정
Splunk 플랫폼은 기본적으로
user
및
power
역할의 동시 실행 검색에 대해 제한을 정의합니다. 이런 동시 검색을 일부 역할에 대해 변경해야 할 수 있습니다.
1. Splunk Enterprise Security 메뉴 모음에서 설정설정 > 일반일반 > 일반일반 설정설정을 선택합니다. 2. 역할의 제한을 검토하고 원하는 대로 변경합니다.
항목항목 설명설명
검색 디스크 배정량(관리 자)
관리자 역할을 보유한 사용자가 검색 작업 결과를 저장하는 데 사용할 수 있는 최대 디스크 공 간(MB)
검색 작업 배정량(관리자) 관리자 역할을 보유한 사용자의 최대 동시 검색 수
검색 작업 배정량(고급) 파워 역할을 보유한 사용자의 최대 동시 검색 수
admin
및
power
가 아닌 역할의 제한을 변경하려면
authorize.conf
파일을 편집하여 기본 검색 배정량을 업데이트합니다. Splunk Enterprise 설치 매뉴얼에서 authorize.conf.example을 참조하십시오.
여러여러 인덱스를 인덱스를 검색하도록 검색하도록 역할역할 설정설정
Splunk 플랫폼은 수집된 데이터 원본을 여러 인덱스에 저장합니다. 데이터를 여러 인덱스에 분산시키면 역할 기반 액세스 제어를 사용하고 데이터 원본 보존 정책을 각기 다르게 설정할 수 있습니다. Splunk 플랫폼은 기본적으로
main
인덱스만 검 색하도록 모든 역할을 설정합니다. 역할을 사용한 작업에 대한 자세한 내용은 Splunk 플랫폼 매뉴얼을 참조하십시오.
Splunk Enterprise의 경우, Splunk Enterprise 보안 매뉴얼에서 역할 기반 사용자 액세스 설정을 참조하십시오. Splunk Cloud의 경우, Splunk Cloud 사용자 매뉴얼에서 Splunk Cloud 사용자 및 역할 관리를 참조하십시오.
Splunk Enterprise Security 역할에 추가 인덱스 검색을 허용하려면 관련 보안 데이터가 포함된 인덱스를 관련 역할에 배정 하십시오.
1. 설정설정 > 액세스 액세스 제어제어를 선택합니다. 2. 역할역할을 클릭합니다. 3. 추가 인덱스 검색을 허용할 역할 이름을 클릭합니다. 4. 원하는 기본적으로 기본적으로 검색되는 검색되는 인덱스와 인덱스 이 역할이 검색할 수 있는 인덱스를 인덱스 선택합니다. 요약 인덱스를 포함시키지
마십시오. 검색 및 요약 인덱스 루프가 발생할 수 있습니다. 5. 변경 사항을 저장합니다. 6. 필요한 경우 추가 역할마다 반복합니다.
올바른 인덱스로 역할을 업데이트하지 않으면 배정되지 않은 인덱스의 데이터에 의존하는 검색과 기타 knowledge object 가 업데이트되지 않거나 결과를 표시하지 않습니다.
여러 인덱스가 필요한 이유에 대한 자세한 내용은 Splunk Enterprise 인덱서 및 인덱서 클러스터 관리에서 다중 인덱스가 필 요한 이유를 참조하십시오.
Splunk Enterprise Security의 데이터 데이터 모델모델 설정설정
Splunk Enterprise Security는 가속된 가속된 데이터 데이터 모델모델을 활용하여 대시보드와 뷰를 채우고 상관(correlation)검색 결과를 제
21
공합니다. 데이터 모델은 Splunk Enterprise Security 설치본에 포함된 CIM(Common Information Model) 추가 기능 (Splunk_SA_CIM)에서 정의 및 제공됩니다. Enterprise Security는 Splunk Enterprise Security 콘텐츠에만 해당되는 고 유 데이터 모델도 설치합니다.
데이터 데이터 모델모델 가속가속 검색검색 부하부하
데이터 모델은 검색 헤드에서 시작되는 예약된 요약 검색 프로세스를 통해 가속됩니다. 요약 검색은 인덱서에서 실행되어 데 이터 모델을 필터로 사용하면서 새로 인덱싱된 데이터를 검색합니다. 검색 결과의 일치 항목은 인덱스 버킷과 함께 디스크에 저장되어 빨리 액세스할 수 있습니다.
Splunk 플랫폼 6.3 이상에서는 요약 검색을 인덱서별로 데이터 모델당 2개까지 동시에 실행할 수 있습니다. 자세한 내용은 Splunk Enterprise 용량 계획 매뉴얼에서 병렬 요약을 참조하십시오. Splunk Cloud에서 병렬 요약 설정을 조정하려면 지원 을 요청하십시오.
데이터 데이터 모델모델 검색을 검색을 특정특정 인덱스로 인덱스로 제한제한
Splunk Common Information 추가 기능을 사용해 데이터 모델에 의해 검색되는 인덱스를 제한하여 성능을 개선할 수 있습 니다. Splunk CIM(Common Information Model) 추가 기능 사용자 매뉴얼에서 Splunk CIM(Common Information Model) 추가 기능 설정을 참조하십시오.
CIM 데이터 데이터 모델에 모델에 대해대해 데이터 데이터 모델모델 가속가속 설정설정
Splunk Common Information 추가 기능으로 다시 채우기 시간, 최대 동시 검색 수, 수동 재작성 및 예약 우선 순위를 포함한 데이터 모델 가속 설정을 각 데이터 모델별로 조정할 수 있습니다. Splunk 플랫폼 6.6.0 버전을 사용하는 경우, CIM 데이터 모델과 함께 사용하는 사용자 지정 태그가 포함되도록 태그 허용 리스트 설정을 구성하십시오. Splunk CIM(Common Information Model) 추가 기능 사용자 매뉴얼에서 CIM 데이터 모델 가속를 참조하십시오.
데이터 데이터 모델모델 가속가속 저장저장 및 보존보존
데이터 모델 가속에서는 인덱서를 처리 및 저장용으로 사용하여 가속된 데이터를 각 인덱스와 나란히 저장합니다. 총 데이터 용량을 기준으로 인덱서에 필요한 추가 저장 공간을 계산하려면 다음 공식을 사용하십시오.
가속된 데이터 모델 저장 공간/년 = 일별 데이터 용량 * 3.4
이 공식에서는 가속된 데이터 모델에 권장되는 보존율을 사용한다고 가정합니다.
예를 들어 Enterprise Security와 함께 사용할 데이터 용량을 하루에 100GB씩 처리하는 경우, 최대 1년 동안 데이터 모델을 가속하고 원본 데이터를 보존하려면 모든 인덱서에 걸쳐 약 340GB의 추가 공간이 필요합니다.
저장저장 용량용량 설정설정
데이터 모델 가속 저장 용량은
indexes.conf
에서
tstatsHomePath
매개변수를 사용하여 관리됩니다. 기본 데이터 모델 가속 저 장 경로는 명시적으로 달리 설정하지 않는 한 Splunk 플랫폼 기본 인덱스 경로인
$SPLUNK_HOME/var/lib/splunk
입니다. 데이 터 모델 가속에 사용되는 저장 공간은 버킷 롤링과 사용 가능 공간 확인 같은 유지관리 작업에 필요한 인덱스 크기 계산에 포 함되지 않습니다.
데이터 모델 가속 저장 공간을 인덱스 설정과 별도로 관리하려면
[volume:]
스탠자를 사용하여 새 저장 경로를 정의해야 합니 다. 용량을 정의하고 데이터 모델 가속를 저장하는 예는 Splunk 플랫폼 매뉴얼을 참조하십시오.
Splunk Enterprise의 경우, Splunk Enterprise 지식 관리자 매뉴얼에서 데이터 모델 요약에 대한 크기 기반 보존 설정 을 참조하십시오. Splunk Cloud의 경우, Splunk Cloud 지식 관리자 매뉴얼에서 데이터 모델 요약에 대한 크기 기반 보존 설정을 참조 하십시오.
데이터 데이터 모델모델 기본기본 보존보존 설정설정
데이터 모델 보존 설정은 이용 사례와 데이터 원본에 따라 다릅니다. 보존 기간이 더 짧으면 디스크 공간을 더 적게 사용하고 처리 시간을 더 짧게 유지해도 되지만, 그 대신 가속된 데이터의 시간 범위가 제한됩니다.
데이터 데이터 모델모델 요약요약 범위범위
경고 전체 시간
애플리케이션 상태 1개월
자산 및 ID(ES) 없음
인증 1년
22
인증서 1년
변경 사항 분석 1년
데이터베이스 없음
데이터 손실 방지 1년
도메인 분석(ES) 1년
이메일 1년
인시던트 관리(ES) 전체 시간
프로세스 간 메시징 1년
침입 탐지 1년
인벤토리 없음
Java VM(가상 머신) 전체 시간
멀웨어 1년
네트워크 분석(DNS) 3개월
네트워크 세션 3개월
네트워크 트래픽 3개월
성능 1개월
위험 분석(ES) 전체 시간
Splunk 감사(audit) 로그 1년
위협 인텔리전스(ES) 전체 시간
티켓 관리 1년
업데이트 1년
사용자 및 엔티티 행동 분석(ES) 전체 시간
취약점 1년
웹 3개월
Splunk CIM(Common Information Model) 앱의 CIM 설정설정 페이지를 사용하여 CIM 데이터 모델 보존 설정을 수정할 수 있 습니다. 자세한 내용은 Splunk CIM(Common Information Model) 추가 기능 사용자 매뉴얼에서 데이터 모델 가속 요약 범 위 변경을 참조하십시오. 사용자 지정 데이터 모델의 요약 범위나 기타 설정을 변경하려면 앱 또는 추가 기능과 함께 제공된
datamodels.conf
를 수동으로 편집하십시오.
Splunk Enterprise에서 이런 설정을 편집하는 방법에 대한 설명은 Splunk Enterprise 관리자 매뉴얼에서 datamodels.conf spec 파일을 참조하십시오. Splunk Cloud를 사용하는 경우 해당 설정을 조정하려면 지원 요청을 접수하십시오.
데이터 데이터 모델모델 가속가속 재작성 재작성 동작동작
Splunk 플랫폼에서 데이터 모델 구조 설정이 변경되거나 데이터 모델을 만드는 기본 검색이 변경되면 데이터 모델 가속 전 체 재작성이 시작됩니다. Enterprise Security는 데이터 모델 설정 변경 사항을 최신 가속에만 적용하여 기본 동작을 수정하 고, 이전 가속가 제거되지 않게 합니다. 정의된 보존 기간에 도달하거나 인덱스 버킷과 함께 롤링될 때까지 기존의 가속된 데 이터 모델이 모두 이전 설정과 함께 인덱서에 보존됩니다. 최고 성능을 유지하려면 Splunk Enterprise Security에서 사용하 는 데이터 모델에 대한 수동 재작성 설정을 변경하지 마십시오.
재작성 설정 옵션은
datamodels.conf
파일에서 관리됩니다.
가속와 재작성 동작에 대한 자세한 내용은 Splunk 플랫폼 매뉴얼을 참조하십시오.
Splunk Enterprise의 경우, Splunk Enterprise 지식 관리자 매뉴얼에서 영구적으로 가속된 데이터 모델의 고급 설정 을 참조하십시오. Splunk Cloud의 경우, Splunk Cloud 지식 관리자 매뉴얼에서 영구적으로 가속된 데이터 모델의 고급 설정을 참조하 십시오.
23
데이터 데이터 모델모델 관리관리 페이지 페이지를 사용하여 전체 재작성을 적용합니다. 설정설정 > 데이터 데이터 모델모델로 이동하고, 데이터 모델을 선 택하고, 왼쪽 화살표를 사용하여 행을 확장하고, 재작성 재작성 링크를 선택합니다.
모든 데이터 모델의 가속 상태를 검토하려면 데이터 모델 감사 대시보드를 사용합니다.
데이터 데이터 모델모델 가속가속 적용적용
Enterprise Security는 모듈식 입력을 통해 데이터 모델 가속를 적용합니다. ES에서 데이터 모델 가속를 비활성화하는 방법:
1. Splunk Enterprise 도구 모음에서 설정설정 > 데이터 데이터 입력입력을 열고 데이터 데이터 모델모델 가속가속 적용적용 설정설정을 선택합니다. 2. 데이터 모델을 선택합니다. 3. 가속가속 적용됨 적용됨 옵션을 선택 해제합니다. 4. 저장합니다.
Splunk Enterprise Security에서에서 사용하는 사용하는 데이터 데이터 모델모델
Splunk Enterprise Security에서 사용하는 데이터 모델에 대한 참고 정보는 Splunk 개발자 포털에서 ES에서 사용하는 데 이터 모델을 참조하십시오.
