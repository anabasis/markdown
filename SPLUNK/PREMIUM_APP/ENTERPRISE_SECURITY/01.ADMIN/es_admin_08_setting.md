# 설정 및 문제 해결

## Splunk Enterprise Security의 일반 설정 구성

Splunk Enterprise 관리자는 Splunk Enterprise Security 설치본의 설정을 변경할 수 있음. 임계값, 매크로 정의, 검색필터 및 기타 흔히 변경되는 값을 일반 설정 페이지에서 변경.

Enterprise Security 메뉴 모음에서 설정 > 일반 > 일반 설정을 선택함.

|설정|설명|
|:--:|:--|
|Asset Sources|A search macro that enumerates the lookup tables that contain asset information used for asset correlation.|
|Auto Pause|Type the time in seconds before a drilldown search will pause.|
|Default Watchlist Search|Define the watchlisted events for the 'Watchlisted Events' correlation search|
|Domain Analysis|Enable or disable WHOIS tracking for Web domains.|
|Domain From URL Extraction Regex|A regular expression used to extract domain (url_domain) from a URL.|
|Enable Identity Generation Autoupdate|If true, permit the Identity Manager to auto-update asset_sources, identity_sources, and generate_identities macros. True by default.|
|Generic Error Search|A search filter for defining events that indicate an error has occurred.|
|HTTP Category Analysis Sparkline Earliest|Set the start time for sparklines displayed on the HTTP User Category Analysis dashboard.|
|HTTP Category Analysis Sparkline Span|Set the time span for sparklines displayed on the HTTP User Category Analysis dashboard.|
|HTTP User Agent Analysis Sparkline Earliest|Set the start time for sparklines displayed on the HTTP User Agent Analysis dashboard.|
|HTTP User Agent Analysis Sparkline Span|Set the time span for sparklines displayed on the HTTP User Agent Analysis dashboard.|
|IRT Disk Sync Delay|Set the number of seconds for Enterprise Security to wait for a disk flush to finish. Relevant to indexed real time searches.|
|Identity Generation|Defines the transformations used to normalize identity information. See How Splunk Enterprise Security processes and merges asset and identity data|
|Identity Generation Timeout|Number of seconds the Identity Manager waits before warning of slow search completion in identity_manager.log.|
|Identity Sources|Enumerates the source lookup tables that contain identity information.|
|Incident Review Analyst Capacity|Estimated maximum capacity of notable events assigned to an analyst. Relative measure of analyst workload.|
|Indexed Realtime|Enable or disable indexed real-time mode for searches.|
|Large Email Threshold|An email that exceeds this size in bytes is considered large.|
|Licensing Event Count Filter|Define the list of indexes to exclude from the "Events Per Day" summarization.|
|Maximum Documents Per Batch Save (kvstore)|The maximum number of documents that can be saved in a single batch to a KV Store collection.|
|New Domain Analysis Sparkline Span|Set the time span for sparklines displayed in the New Domain Analysis dashboard.|
|Notable Modalert Pipeline|SPL for the notable event adaptive response action.|
|Override Email Alert Action|Override the email alert action settings to allow users to send notable events via email through adaptive response actions on the Incident Review dashboard.|
|Risk Modalert Pipeline|SPL for the risk modifier adaptive response action.|
|Search Disk Quota (admin)|Set the maximum amount of disk space in MB that an admin user can use to store search job results.|
|Search Jobs Quota (admin)|Set the maximum number of concurrent searches allowed for admin users.|
|Search Jobs Quota (power)|Set the maximum number of concurrent searches for power users.|
|Short Lived Account Length|An account creation and deletion record that exceeds this threshold is anomalous.|
|TSTATS Allow Old Summaries|Enable or disable searching of data model accelerations containing fields that do not match the current data model configuration.|
|TSTATS Local|Determine whether or not the TSTATS macro will be distributed.|
|TSTATS Summaries Only|Determine whether or not the TSTATS or summariesonly macro will only search accelerated events.|
|Use Other|Enable or disable the term OTHER on charts that exceed default series limits.|
|Website Watchlist Search|A list of watchlisted websites used by the "Watchlisted Events" correlation search.|

|설정|설명|
|:--:|:--|
|자산 원본|자산 상관에 사용되는 자산 정보가 포함된 Lookup 테이블을 열거하는 검색 매크로임.|
|자동 일시 중지|드릴다운 검색을 일시 중지하기 전의 시간(초)을 입력.|
|기본 관심 대상 검색|'관심 대상 이벤트' 상관(correlation)검색용 관심 대상 이벤트를 정의함.|
|도메인 분석|웹 도메인에 대한 WHOIS 추적을 활성화하거나 비활성화함.|
|URL 추출 정규식의 도메인|URL에서 도메인(url_domain)을 추출하는데 사용하는 정규식임.|
|ID 생성 자동 업데이트 활성화|true인 경우 ID 관리자가 asset_sources, identity_sources, generate_identities 매크로를 자동으로 업데이트할 수 있음. 기본값은 true임.|
|일반 오류 검색|오류 발생을 나타내는 이벤트를 정의하는 검색 필터임.|
|가장 빠른 HTTP 범주 분석 sparkline|HTTP 사용자 범주 분석 대시보드에 표시되는 sparkline의 시작 시간을 설정함.|
|HTTP 범주 분석 sparkline 기간|HTTP 사용자 범주 분석 대시보드에 표시되는 sparkline의 시간 범위를 설정함.|
|가장 빠른 HTTP 사용자 에이전트 분석 sparkline|HTTP 사용자 에이전트 분석 대시보드에 표시되는 sparkline의 시작 시간을 설정함.|
|HTTP 사용자 에이전트 분석 sparkline 기간|HTTP 사용자 에이전트 분석 대시보드에 표시되는 sparkline의 시간 범위를 설정함.|
|IRT 디스크 동기화 지연|Enterprise Security가 디스크 플러시가 완료될 때까지 기다리는 초 수를 설정함. 인덱싱된 실시간 검색과 관련이 있음.|
|ID 생성|ID 정보를 정규화하는데 사용되는 변환을 정의함. Splunk Enterprise Security가 자산 및 ID 데이터를 처리 및 병합하는 방법을 참조.|
|ID 생성 제한 시간|ID 관리자가 identity_manager.log에 느린 검색 완료 경고를 표시하기 전에 기다리는 초수임.|
|ID 소스|ID 정보가 포함된 소스 Lookup 테이블을 열거함.|
|인시던트 검토 분석 용량|애널리스트에게 배정되는 주요 이벤트의 추정 최대 용량임. 분석 워크로드의 상대적인 측정치임.|
|인덱싱된 실시간|인덱싱된 실시간 모드를 검색에 대해 활성화하거나 비활성화함.|
|대용량 이메일 임계값|'대용량'으로 간주되는 크기(바이트)를 초과하는 이메일임.|
|라이선싱 이벤트 수 필터|"일별 이벤트" 요약에서 제외된 인덱스 리스트를 정의함.|
|일괄 저장(kvstore) 최대 문서 수|KV 스토어에 일괄로 한 번에 저장할 수 있는 최대 문서 수임.|
|새로운 도메인 분석 sparkline 기간|새로운 도메인 분석 대시보드에 표시되는 sparkline의 시간 범위를 설정함.|
|주요 모듈화 경고 파이프라인|주요 이벤트 adaptive response 작업의 SPL임.|
|이메일 경고 작업 재정의|인시던트 검토 대시보드의 adaptive response 작업에서 사용자가 이메일을 통해 주요 이벤트를 보낼 수 있도록 이메일 경고 작업 설정을 재정의함.|
|위험 모듈화 경고 파이프라인|위험 한정자 adaptive response 작업의 SPL임.|
|검색 디스크 배정량(관리자)|관리자가 검색 작업 결과를 저장하는데 사용할 수 있는 최대 디스크 공간을 MB 단위로 설정함.|
|검색 작업 배정량(관리자)|관리자 사용자에게 허용되는 최대 동시 검색 수를 설정함.|
|검색 작업 배정량(고급)|파워 사용자의 최대 동시 검색 수를 설정함.|
|단기 계정 길이|이 임계값을 초과하는 계정 만들기 및 삭제 레코드는 예외임.|
|TSTATS가 오래된 요약 허용|현재 데이터 모델 설정과 일치하지 않는 필드가 포함된 데이터 모델 가속 검색을 활성화하거나 비활성화함.|
|TSTATS 로컬|TSTATS 매크로의 분산 여부를 결정.|
|TSTATS 요약 전용|TSTATS 또는 요약 전용 매크로가 가속된 이벤트만 검색할 것인지 결정.|
|기타 사용|기본 계열 제한을 초과하는 차트에서 OTHER 조건을 활성화하거나 비활성화함.|
|웹사이트 관심 대상 검색|"관심 대상 이벤트" 상관(correlation)검색에서 사용하는 관심 대상 웹사이트의 리스트임.|

참고 항목은 아래와 같음.

Splunk Enterprise Security에서 입력 자격 증명 관리
Splunk Enterprise Security에서 권한 관리
Splunk Enterprise Security의 메뉴 모음 사용자 지정
Splunk Enterprise Security에서 패널별 필터링 설정

## Splunk Enterprise Security에서 자격 증명 관리

스크립트 기반 또는 모듈식 입력의 자격 증명을 저장하려면 자격 증명 관리 페이지를 사용. 자격 증명을 참조하는 입력 설정에서는 자격 증명 관리에 저장된 자격 증명을 사용. 사용자 이름 및 암호와 같은 자격 증명 또는 타사 시스템을
사용한 인증에 사용되는 인증서를 저장할 수 있음. 서버간 통신을 암호화하는데 사용되는 인증서를 관리하기 위해 이페이지를 사용하지 않음.

역할에 자격 증명 및 인증서를 추가하고 수정하고 볼 수 있는 적절한 기능이 있어야 함. 설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조.

### 새 입력 자격 증명 추가

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 자격 증명 관리를 선택함.
2. 새 자격 증명을 클릭하여 새 사용자 자격 증명을 추가함.
3. 사용자 이름을 입력.
4. (선택 사항) 사용자 이름이 같은 여러 자격 증명을 구별하려면 영역 필드를 입력.
5. 자격 증명에 대한 암호를 입력하고 암호 확인에 다시 입력.
6. 자격 증명의 앱을 선택함.
7. 저장을 클릭함.

### 기존 입력 자격 증명 편집

기존 입력 자격 증명의 암호를 편집할 수 있음.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 자격 증명 관리를 선택함.
2. 자격 증명의 작업 컬럼에서 편집을 클릭함.
3. 자격 증명에 대한 새 암호를 입력하고 암호 확인에 다시 입력.
4. 저장을 클릭함.

### 새 인증서 추가

검색 헤드 클러스터(SHC)에 자격 증명 관리를 사용하여 새 인증서를 추가할 수 없음. SHC에서 Splunk Enterprise
Security에 새 인증서를 추가하려면 주 배포 노드의 $SPLUNK_HOME/etc/apps/<app_name>/auth에 인증서를 추가하고 인증서를
SHC 구성원에게 배포함.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 자격 증명 관리를 선택함.
2. 새 인증서를 클릭하여 새 인증서를 추가함.
3. 인증서의 파일 이름을 입력. 이 이름은 인증서가 $SPLUNK_HOME/etc/apps/<app_name>/auth 디렉터리에 저장되는 파일 이름임.
4. 인증서의 인증서 텍스트를 추가함. 인증서를 Splunk Enterprise Security에 추가하려면 기존 인증서 파일의 콘텐츠를 여기에 붙여넣음.
5. 인증서를 저장할 앱을 선택함.
6. 저장을 클릭함.

### 기존 인증서 편집

자격 증명 관리에서 기존 인증서의 인증서 텍스트를 편집할 수 있음. 검색 헤드 클러스터에서 인증서를 편집할 수 없음.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 자격 증명 관리를 선택함.
2. 인증서의 작업 컬럼에서 편집을 클릭함.
3. 인증서의 새 인증서 텍스트를 입력.
4. 저장을 클릭함.

### 기존 입력 자격 증명 또는 인증서 삭제

검색 헤드 클러스터에서 인증서를 삭제할 수 없음.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 자격 증명 관리를 선택함.
2. 자격 증명 또는 인증서의 작업 컬럼에서 삭제를 클릭함.
3. 확인을 클릭하여 확인.

## Splunk Enterprise Security에서 권한 관리

권한 페이지를 사용하여 Enterprise Security 기능을 관리자가 아닌 역할에 배정함.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 권한을 선택함.
2. 역할과 해당 역할의 권한에 해당하는 체크박스를 선택함.
3. 저장을 클릭함.

ES 기능에 대한 자세한 내용은 설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조.

## Splunk Enterprise Security의 메뉴 모음 사용자 지정

탐색 편집 뷰를 사용하여 Splunk Enterprise Security의 메뉴 모음을 사용자 지정함. 새 대시보드, 보고서, 뷰, 필터링된 대시보드 링크 또는 웹 링크를 메뉴 모음에 추가할 수 있음. 메뉴 모음 탐색을 변경하려면 Enterprise Security 관리자 권한이 있어야 함.

뷰를 여러 뷰를 그룹으로 묶는 컬렉션의 일부로 메뉴 모음에 추가하거나, 메뉴 모음의 개별 항목으로 추가할 수 있음. 예를 들어 인시던트 검토는 메뉴 모음의 개별 대시보드고, 감사(audit)는 감사(audit) 대시보드의 컬렉션임.

Splunk Enterprise Security는 이전 버전에서 탐색 메뉴에 적용된 사용자 지정을 계속 유지함.

### 업데이트된 뷰 확인

사용자가 설치한 앱 버전에서 새로 만들거나 업데이트하거나 지원이 중단된 뷰와 컬렉션은 관련 변경 사항을 나타내는 작은 아이콘으로 강조 표시됨.
새 Splunk Enterprise Security 버전이나 Enterprise Security에서 사용하는 뷰와 컬렉션을 제공하는 앱의 새로운 버전을 설치한 후에는 탐색 편집 뷰를 방문하여 해당 뷰 또는 컬렉션이 업데이트되었는지 확인.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 업데이트된 콘텐츠가 있는 경우 탐색 편집기 상단에 "콘텐츠 업데이트 사용 가능" 메시지가 표시됨.
3. 편집기 창의 뷰에 아이콘이 있는지 찾아보고 추가되거나 업데이트되거나 지원이 중단된 콘텐츠를 찾음. 새 뷰 추가 및 새 컬렉션 추가 메뉴에도 동일한 아이콘이 표시됨.

### Splunk Enterprise Security의 기본 뷰 설정

사용자가 Splunk Enterprise Security를 열 때 특정 뷰 또는 링크를 표시하려면 기본 뷰를 설정.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 기본 뷰로 사용할 뷰나 링크를 찾음.
3. 뷰 위로 마우스를 움직이면 나타나는 체크 표시 아이콘을 클릭하여 이 항목을 기본 뷰로 설정함.
4. 저장을 클릭하여 변경 사항을 저장함.
5. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

### 기본 메뉴 모음 탐색 편집

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 뷰나 뷰 컬렉션이 메뉴에 표시되는 위치를 변경하려면 뷰나 뷰 컬렉션을 클릭하고 끌어서 옮김.
3. 뷰 또는 컬렉션을 메뉴에서 제거하려면 뷰 또는 컬렉션 옆의 X를 클릭함.
4. 컬렉션 이름을 편집하려면 아이콘을 클릭함.
5. 구분선을 추가하고 컬렉션의 항목을 시각적으로 분리하려면 아이콘을 클릭함.
6. 저장을 클릭하여 변경 사항을 저장함.
7. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

### 메뉴 모음에 단일 뷰 추가

새 뷰를 컬렉션에 추가하지 않고 메뉴 모음에 추가할 수 있음.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 새 뷰 추가를 클릭함.
3. 뷰 옵션 설정을 기본값인 뷰로 놔둠.
4. 사용하지 않는 뷰에서 뷰 선택을 클릭함.
5. 리스트에서 대시보드 또는 뷰를 선택함.
6. 저장을 클릭함. 대시보드가 탐색 편집기에 표시됨.
7. 메뉴에 항목을 추가하는 작업을 마치면 저장을 클릭하여 변경 사항을 저장함.
8. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

### 메뉴 모음에 컬렉션 추가

컬렉션을 사용하여 메뉴 모음의 여러 뷰 또는 링크를 함께 정리함.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 새 컬렉션 추가를 클릭함.
3. 이름을 입력. 예: 감사(audit)
4. 저장을 클릭함. 컬렉션이 탐색 편집기에 표시됨.

뷰 또는 링크를 메뉴 탐색에 표시하려면 컬렉션에 먼저 추가해야 함.

### 기존 컬렉션에 뷰 추가

뷰를 기존 컬렉션에 추가함.

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 뷰를 추가할 컬렉션을 찾음.
3. 아이콘을 클릭함.
4. 뷰 옵션 설정을 기본값인 뷰로 놔둠.
5. 사용하지 않는 뷰에서 뷰 선택을 클릭함.
6. 리스트에서 뷰를 선택함.
7. 저장을 클릭함. 뷰가 탐색 편집기에 표시됨.
8. 메뉴에 항목을 추가하는 작업을 마치면 저장을 클릭하여 변경 사항을 저장함.
9. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

### 메뉴 모음에 링크 추가

Splunk Enterprise Security의 메뉴 모음에 링크를 추가할 수 있음. 예를 들어 링크를 인스던트 검토의 구체적으로 필터
링된 뷰나 외부 티케팅 시스템에 추가할 수 있음.

#### 메뉴에 외부 시스템 또는 웹페이지 링크 만들기

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 새 뷰 추가를 클릭하여 뷰를 메뉴에 추가하거나, 기존 컬렉션을 찾고 아이콘을 클릭하여 링크를 기존 뷰 컬렉션에 추가함.
3. 뷰 옵션에서 링크를 선택함.
4. Splunk Enterprise Security 메뉴에 표시할 이름을 입력합니다 . 예: Splunk 응답 페이지
5. 링크를 입력. 예: <https://answers.splunk.com/>
6. 저장을 클릭함.
7. 메뉴에 항목을 추가하는 작업을 마치면 저장을 클릭하여 변경 사항을 저장함.
8. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

#### 인시던트 검토의 필터링된 뷰에 링크 추가

메뉴 모음에 많이 추가되는 링크 중 하나는 인시던트 검토의 필터링된 뷰임.

1. 인시던트 검토를 원하는 필터로 필터링함. 대시보드를 필터링하면 필터와 일치하는 쿼리 문자열 매개변수로 URL이 업데이트됨.
2. 웹 브라우저 주소 표시줄에서 URL의 /app/SplunkEnterpriseSecuritySuite/로 시작되는 부분을 복사하여 일반 텍스트 파일에 참고용으로 붙여넣음.
    예를 들어 중요한 주요 이벤트만 표시되도록 대시보드를 필터링한 경우 URL에서 복사하는 부분은
    /app/SplunkEnterpriseSecuritySuite/incident_review?form.selected_urgency=critical과 유사함.
3. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
4. 새 뷰 추가를 클릭하여 뷰를 메뉴에 추가하거나, 기존 컬렉션을 찾고 뷰 추가 아이콘을 클릭하여 링크를 기존 뷰 컬렉션에 추가함.
5. 뷰 옵션에서 링크를 선택함.
6. Splunk Enterprise Security 메뉴에 표시할 이름을 입력합니다 . 예: IR - 중요
7. 링크 필드에 URL의 일부분을 붙여넣음.
    예: /app/SplunkEnterpriseSecuritySuite/incident_review?form.selected_urgency=critical
8. 저장을 클릭함.
9. 메뉴에 항목을 추가하는 작업을 마치면 저장을 클릭하여 변경 사항을 저장함.
10. 확인을 클릭하여 페이지를 새로 고치고 변경 사항을 확인.

매개변수가 여러 개인 링크를 추가하는 경우 매개변수를 &amp;로 분리하는 &를 인코딩하여 쿼리 문자열 매개변수를 수정해야 함. 예를 들어 배정되지 않은 새 주요 이벤트를 /app/SplunkEnterpriseSecuritySuite/incident_review?form.status_form=1&amp;form.owner_form=unassigned로 표시하는 인시던트 검토의 필터링된 뷰에 대한 링크를 입력.

다음 테이블에 있는 매개변수를 사용하여 URL을 수동으로 구성할 수도 있음. 특정 매개변수에 대해 모든 결과를 표시하려면 별표를 사용. 모든 매개변수가 필요하지는 않음.

<table>
<tr><td>매개변수</td><td>설명</td><td>가능한 값</td><td>예</td></tr>
<tr><td>form.selected_urgency</td><td>Display notable events with the urgency specified by this parameter.</td><td>critical, high, medium, low, informational</td><td>form.selected_urgency=critical</td></tr>
<tr><td>form.status_form</td><td>Display notable events with the status specified by this parameter. An integer corresponds to each status value.</td><td>0 for unassigned, 1 for new, 2 for in progress, 3 for pending, 4 for resolved, 5 for closed</td><td>form.status_form=0</td></tr>
<tr><td>form.owner_form</td><td>Display notable events owned by the user specified by this parameter.</td><td>usernames</td><td>form.owner_form=admin</td></tr>
<tr><td>form.source</td><td>Display notable events created by the correlation search specified by this parameter. HTML-encode spaces in the correlation search name and use the name that appears in the notable event rather than the name that appears on Content Management.</td><td>Endpoint - Host With Multiple Infections - Rule</td><td>form.source=Endpoint%20-%20Host%20With%20Multiple%20Infections%20-%20Rule</td></tr>
<tr><td>form.rule_name</td><td>Display notable events created by the correlation search specified by this parameter. HTML-encode spaces in the correlation search name. Use the name that appears on Content Management.</td><td>Host With Multiple Infections</td><td>form.rule_name=Host%20With%20Multiple%20Infections</td></tr>
<tr><td>form.tag</td><td>Displays notable events with the tag specified by this paramter.</td><td>malware, any custom tag value</td><td>form.tag=malware</td></tr>
<tr><td>form.srch</td><td>Displays notable events that match the SPL specified in this parameter. HTML-encode special characters such as = for key-value pairs.</td><td>dest=127.0.0.1</td><td>form.srch=dest%3D127.0.0.1</td></tr>
<tr><td>form.security_domain_form</td><td>Displays notable events in the security domain specified by this parameter.</td><td>access, endpoint, network, threat, identity, audit</td><td>form.security_domain_form=endpoint</td></tr>
<tr><td>earliest= and latest=</td><td>Displays notable events in the time range specified by these parameters. Specify a relative time range. HTML-encode special characters such as @.</td><td>-24h@h, now</td><td>earliest=-24h%40h&latest=now</td></tr>
<tr><td>form.new_urgency_count_form</td><td>Displays notable events that do not have the urgency specified by this parameter.</td><td>critical, high, medium, low, informational</td><td>form.new_urgency_count_form=informational</td></tr>
<tr><td>form.selected_urgency</td><td>Displays notable events that have the urgency specified by this parameter. Use multiple instances of this parameter to select multiple urgency settings.</td><td>critical, high, medium, low, informational</td><td>form.selected_urgency=critical&form.selected_urgency=high</td></tr>
<tr><td>event_id</td><td>Displays the notable event that matches the specified event_id.</td><td>3C84A9D8-87F6-4066-8659-C7DD680F98E6@@notable@@80e0f89da83cad6665dd1de7447cedb4</td><td>event_id=3C84A9D8-87F6-4066-8659-C7DD680F98E6@@notable@@80e0f89da83cad6665dd1de7447cedb4</td></tr>
<tr><td>form.association_type
form.association_id</td><td>Used together, displays the notable events associated with a short ID or an investigation.</td><td>short_id, investigation EYIYNW, 5a4be2b8cdc9736b2352c7c3</td><td>form.association_type=short_id&form.association_id=EYIYNW</td></tr>
</table>

<table>
<tr><td>매개변수</td><td>설명</td><td>가능한 값</td></tr>
<tr><td>form.selected_urgency</td><td>주요 이벤트를 이 매개변수를 통해 지정되는 긴급도와 함께 표시.</td><td>critical, high, medium, low, informational</td></tr>
<tr><td>form.status_form</td><td>주요 이벤트를 이 매개변수를 통해 지정되는 상태와 함께 표시. 정수는 각 상태값에 해당함.</td><td>0은 배정되지 않음, 1은 새 이벤트, 2는 진행 중, 3은 보류 중, 4는 해결됨, 5는 종료됨에 해당함.</td></tr>
<tr><td>form.owner_form</td><td>이 매개변수를 통해 지정되는 사용자가 소유한 주요 이벤트를 표시.</td><td>usernames</td></tr>
<tr><td>form.source</td><td>이 매개변수를 통해 지정된 상관(correlation)검색에서 만드는 주요 이벤트를 표시. 상관(correlation)검색 이름의
공백을 HTML로 인코딩하고 주요 이벤트에 표시되는 이름을 콘텐츠 관리에 표시되는 이름 대신 사용.</td><td>Endpoint - Host With Multiple Infections - Rule</td></tr>
<tr><td>form.rule_name</td><td>이 매개변수를 통해 지정된 상관(correlation)검색에서 만드는 주요 이벤트를 표시. 상관(correlation)검색 이름의 공백을 HTML로 인코딩함. 콘텐츠 관리에 표시되는 이름을 사용.</td><td>Host With Multiple Infections</td></tr>
<tr><td>form.tag</td><td>이 매개변수를 통해 지정된 태그가 있는 주요 이벤트를 표시.</td><td>malware, 사용지 지정 태그 값</td></tr>
<tr><td>form.srch</td><td>이 매개변수에 지정된 SPL과 일치하는 주요 이벤트를 표시. 키값 쌍의 = 같은 특수 문자를 HTML로 인코딩함.</td><td>dest=127.0.0.1</td></tr>
<tr><td>form.security_domain_form</td><td>이 매개변수를 통해 지정된 보안 도메인의 주요 이벤트를 표시.</td><td>access, endpoint, network, threat, identity, audit</td></tr>
<tr><td>earliest= 및 latest=</td><td>이 매개변수를 통해 지정된 시간 범위의 주요 이벤트를 표시. 상대 시간 범위를 지정. @ 같은 특수 문자를 HTML로 인코딩함.</td><td>-24h@h, now</td></tr>
<tr><td>form.new_urgency_count_form</td><td>이 매개변수를 통해 지정된 긴급도가 없는 주요 이벤트를 표시.</td><td>critical, high, medium, low, informational</td></tr>
<tr><td>form.selected_urgency</td><td>이 매개변수를 통해 긴급도가 지정된 주요 이벤트를 표시. 여러 긴급도 설정을 선택하려면 이 매개변수의 여러 인스
턴스를 사용.</td><td>critical, high, medium, low, informational</td></tr>
<tr><td>event_id</td><td>지정된 event_id와 일치하는 주요 이벤트를 표시.</td><td>3C84A9D8-87F6-4066-8659-
C7DD680F98E6@@notable@@80e0f89da83cad6665dd1de7447cedb4</td></tr>
<tr><td>form.association_type
form.association_id</td><td>함께 사용되면 짧은 ID또는 조사와 연결된 주요 이벤트를 표시.</td><td>short_id, investigation
EYIYNW, 5a4be2b8cdc9736b2352c7c3</td></tr>
</table>

### 기본 탐색 복원

Splunk Enterprise Security 메뉴 모음의 기본 탐색을 복원하는 방법:

1. Enterprise Security 메뉴 모음에서 설정 > 일반 > 탐색을 선택함.
2. 오른쪽 상단 모서리에서 기본 설정 복원을 클릭함.
3. 확인을 클릭하여 확인.
4. 페이지 하단으로 스크롤하고 저장을 클릭함.

## Splunk Enterprise Security에서 패널별 필터링 설정

Splunk Enterprise Security의 일부 대시보드에는 필터링을 통해 항목을 대시보드 뷰에서 제거할 수 있는 패널별 필터 옵션이 있어 조사할 필요가 있는 이벤트를 더 쉽게 찾을 수 있음.

- 이벤트가 위협이라고 확인될 경우 패널별 필터 편집기를 사용하여 항목을 알려진 위협 차단 리스트에 추가.
- 이벤트가 위협이 아니라고 확인될 경우 허용 리스트에 추가하여 대시보드 뷰에서 제거할 수 있음.

참고: 패널별 필터 단추는 사용자에게 권한이 있는 경우에만 표시됨. 이 권한을 설정하는 방법은 설치 및 설정 매뉴얼에
서 사용자 및 역할 설정을 참조.

### 허용 리스트 이벤트

이벤트가 위협이 아니라고 확인된 후에는 이벤트를 허용 리스트에 추가하여 대시보드 뷰에서 숨길 수 있음. 요약 통계는
허용 리스트에 추가된 항목을 계속 계산하지만, 대시보드에 항목이 표시되지 않음.

#### 허용 리스트에 이벤트 추가

패널별 필터를 사용하여 대시보드에 있는 이벤트를 허용 리스트에 추가하거나 필터링함.
예를 들어 트래픽 크기 분석 대시보드에서 트래픽 이벤트를 허용 리스트에 추가하는 방법은 다음과 같음.

1. 체크박스를 사용하여 필터링할 항목을 선택함.
2. 오른쪽 상단 모서리에 있는 패널별 필터를 클릭하여 이 대시보드에서 필터링할 수 있는 이벤트에 대한 옵션을 표시.
3. 라디오 단추를 선택하여 이 대시보드에서 이벤트를 필터링함. 예를 들어 트래픽 크기 분석 대시보드에서는 이벤트를 필터링하여 더 이상 표시되지 않게 하거나 강조 표시하여 중요한 이벤트로 분류할 수 있음.
4. 완료되면 저장을 클릭함.

참고: 필터링된 이벤트는 이 대시보드 계산에서 제거되지 않고 뷰에서만 제거됨.
이 예에서, 항목을 허용 리스트에 추가한 후에는 항목이 (위협이 아닌) 좋은 것으로 간주되고 트래픽 크기 분석 대시보드에 더 이상 표시되지 않음.

#### 허용 리스트에서 항목 제거

1. 패널별 필터를 클릭한 다음 Lookup 파일 보기/편집을 클릭하여 현재 필터링되고 있는 항목의 리스트를 확인.
2. 테이블에서 셀을 마우스 오른쪽 단추로 클릭하여 컨텍스트 메뉴를 표시.
3. 행 제거를 선택하여 허용 리스트에 있는 항목이 포함된 행을 제거.
4. 저장을 클릭함.

### 차단 리스트에 이벤트 추가

이벤트를 차단 리스트에 추가할 수도 있음. 항목을 차단 리스트에 추가하면 악성이라고 알려진 이벤트나 악성이라고 알려진 명령어 및 관리 서버와 통신한다고 의심되는 이벤트를 식별했음을 의미함. 이 이벤트나 문자열이 데이터에 나타날
때는 항상 시스템, 시스템과 연결된 사용자, 웹 활동을 조사하여 위협의 성격과 확산 가능성을 이해해야 함.

이벤트 또는 문자열을 차단 리스트에 추가하는 방법은 허용 리스트와 비슷함. 대시보드에서 필터링된 이벤트만 차단 리스트에 추가할 수 있음.

예를 들어 트래픽 크기 분석 대시보드에 있는 트래픽 이벤트를 차단 리스트에 추가하려면 다음 작업을 수행함.

1. 고급 필터 페이지에서 Lookup 파일 보기/편집을 클릭하여 현재 필터링되고 있는 항목의 리스트를 확인.
2. 차단 리스트에 추가할 항목을 찾음. 필터 컬럼에서 whitelist라는 단어를 두 번 클릭하여 셀을 편집함. "whitelist"를 삭제하고 "blacklist"를 입력.
3. 저장을 클릭함.

### 패널별 필터 리스트 편집

현재 패널별 필터 리스트를 대시보드별로 보려면 설정 > 데이터 보강 > 리스트 및 Lookup으로 이동함. 대시보드 필터임을 나타내는 설명이 있는 리스트에 해당 대시보드의 현재 패널별 필터가 표시됨. 대시보드의 허용 리스트에 추가된 이벤
트가 여기에 나열됨.

예를 들어 위협 활동 필터 리스트에는 위협 활동 대시보드의 필터가 표시됨.
패널별 필터 리스트를 편집함.

1. 관련 대시보드의 필터 리스트를 염. 필터 이름(예: ppf_threat_activity)이 왼쪽 상단 모서리에 표시됨.
2. 필드를 편집하려면 셀을 선택하고 입력을 시작함.
3. 필터에(서) 행 또는 컬럼을 삽입하거나 제거하려면 옵션 편집 필드를 마우스 오른쪽 단추로 클릭함. 행을 제거하면 해당 항목이 대시보드 패널 뷰에 다시 추가되고 허용 리스트에서 제거됨.
4. 항목을 차단 리스트에 추가하려면 편집기를 사용하여 새 행을 테이블에 추가하고 "filter" 컬럼에 "blacklist"를 사용.
5. 마치면 저장을 클릭함.

#### 패널별 필터 감사 (audit)

패널별 필터에 적용한 변경 사항은 패널별 필터링 감사(audit) 로그에 기록됨. Lookup 편집기와 패널별 필터 모듈이 패널별
필터를 수정함. 패널별 필터 감사(audit) 대시보드를 사용하여 패널별 필터를 감사(audit)함.

## Splunk Enterprise Security에서 Splunk Web 메시지 만들기

Splunk Web에서 Splunk 메시지 만들기 경고 작업을 사용하여 검색의 결과를 기반으로 메시지를 만듬. 관리자만 이경고 작업을 사용하여 메시지를 만들 수 있음.

이 경고 작업을 사용하여 만드는 메시지는 이미 messages.conf에 존재해야 함. 메시지 만들기에 대한 자세한 내용은 Splunk Enterprise 관리자 매뉴얼의 Splunk Web 메시지 사용자 지정을 참조.

1. 검색 또는 상관(correlation)검색에서 Splunk Web 메시지를 만들 수 있음.
    |옵션|절차|
    |:--:|:--|
    |새 경고 만들기|검색 및 보고 앱의 검색 페이지에서 다른 이름으로 저장 > 경고를 선택함. 경고 세부정보를 입력 및 선택하고 필요에 따라 트리거 및 조절을 설정함.|
    |상관(correlation)검색 만들기 또는 편집|ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함. 새 콘텐츠 만들기 > 상관(correlation)검색을 선택함. 필요에 따라 상관(correlation)검색 설정을 입력 및 선택함.|
    |상관(correlation)검색 편집|ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함. 상관(correlation)검색을 선택함.|
2. 작업 추가를 클릭하고 Splunk 메시지 작성을 선택함.
3. 이름을 선택함. 이 이름은 기존 메시지의 messages.conf에 있는 스탠자에 해당함.
    예: DISK_MON:INSUFFICIENT_DISK_SPACE_ERROR.
4. (선택 사항) 메시지를 나타내는 메시지 ID를 입력.
    예: insufficient_diskspace.
5. (선택 사항) 메시지에서 필드 대체를 사용하는 경우 사용할 필드를 입력. 인수 대체에 사용되는 필드는 검색 결과에 반환되어야 메시지에 포함됨. 필드를 메시지에서 대체되어야 하는 순서대로 입력.
    예를 들어, Host %s has free disk space %d, below the minimum 5GB. 메시지의 경우 src,FreeMBytes 필드를 입력.
6. (선택 사항) 최근 메시지만 유지에 대해 예를 선택하고 검색에 의해 생성된 최근 메시지만을 유지함.
    예를 들어, 3일 동안 디스크 공간이 적으면 3일 동안 매일 메시지를 받는 대신 이 설정에 대해 예를 선택하여 하나의 메시지만 표시.
7. 저장을 클릭함.

## Splunk Enterprise Security에서 스크립트 오류 문제 해결

Splunk Enterprise Security에서 모듈식 입력의 스크립트 오류 문제를 해결함. 스크립트가 비정상적으로 종료되었거나 스크립트의 상태를 알 수 없다는 메시지가 표시되면 오류가 발생한 스크립트와 스탠자를 조사.

Audit - Script Errors 검색은 설정 확인 스크립트를 대체하고 Splunk 배포의 스크립트 결과 발생하는 0이 아닌 종료 코드에 대해 경고하기 위해 Splunk 메시지를 만듬.

|가능한 근본 원인|확인|해결 방법|
|:--:|:--|:--|
|스크립트가 성공적으로 실행되지 않았음.|스크립트의 로그 파일을 검토함. 스크립트를 수동으로 실행하여 성공적으로 실행되는지 확인하고 결과로 발생하는 종료 코드를 검토함.|스크립트가 0이 아닌 종료 코드로 종료된 이유를 설명.|
|스크립트가 성공적으로 실행되고 0이 아닌 종료 코드가 발생함.|스크립트를 수동으로 실행하여 성공적으로 실행되는지 확인하고 결과로 발생하는 종료 코드를 검토함.|검색에 대한 제거에 스크립트를 포함하여 이 스크립트에 대한 메시지를 표시하지 않도록 함.|
|스크립트의 상태를 알 수 없음. 스크립트의 중지 시간이 있지만 종료 상태나 시작 시간이 없음.|모듈식 입력 설정이 올바른지 확인.|모듈식 입력 설정을 수정함.|

### 특정 스크립트에 대한 메시지가 표시되지 않도록 하기

필요한 경우 `script_error_message_ignore` 매크로에서 일치 구문을 수정하여 특정 스크립트에 대한 메시지를 표시하지 않을 수 있음.
[configuration_check://confcheck_script_errors] 스탠자에 로컬로 정의된 스크립트 제거 정규식이 있으면 매크로에 복제할 수 있음. 예를 들어, 제거 스탠자에는 다음과 같은 정규식이 포함됨.

suppress = ((streamfwd|splunk-
(wmi\.path|MonitorNoHandle\.exe|winevtlog\.exe|netmon\.exe|perfmon\.exe|regmon\.exe|winprintmon\.exe|admon\.exe)).*exited
with code 1)

매크로는 다음과 같은 정의를 사용하여 이 제거를 복제함.

match(script, "(streamfwd|splunk-
(wmi\.path|MonitorNoHandle\.exe|winevtlog\.exe|netmon\.exe|perfmon\.exe|regmon\.exe|winprintmon\.exe|admon\.exe|powershell\.exe))")
AND exit_status=1

특정 스크립트를 표시하지 않는 대신 특정 스크립트에 대한 메시지의 빈도를 줄이려면 경고를 조절함. script 필드 등 필요한 값을 기반으로 Audit - Script Errors 검색에 대한 경고 조절을 설정함.

- Splunk Enterprise의 경우 경고 매뉴얼에서 경고 조절을 참조.
- Splunk Cloud의 경우 경고 매뉴얼에서 경고 조절을 참조.

## 관리자 역할에 의해 검색된 기본 인덱스에 대한 메시지 문제 해결

Splunk 플랫폼에서 관리자 역할에 의해 검색된 기본 인덱스에 대한 Splunk 메시지의 문제를 해결함.

### 기본 관리자 검색에 요약 인덱스 포함

관리자 역할에서 기본적으로 요약 인덱스를 검색하는 경우 성능이 저하될 수 있음. 관리자 역할에 의해 검색되는 인덱스를 제한하거나 검색을 비활성화하여 이 설정에 대한 메시지를 표시하지 않을 수 있음.

#### 관리자 역할에 의해 검색되는 인덱스 제한

관리자 역할에서 요약 인덱스를 검색하지 못하도록 함. 인덱스 이름이 _summary(예: endpoint_summary)로 끝나므로 요약 인덱스 이름을 식별할 수 있음.

1. 설정 > 액세스 제어를 선택함.
2. 역할을 클릭함.
3. 관리자를 클릭함.
4. 인덱스에서 선택된 인덱스에서 제거할 요약 인덱스를 클릭함.
5. 저장을 클릭함.

#### 메시지를 표시하지 않도록 검색 비활성화

관리자 역할에 의해 검색된 인덱스를 제한하지 않지만 메시지를 표시하지 않으려면 검색을 비활성화함.

1. 설정 > 검색, 보고서 및 경고를 선택함.
2. 감사(Audit) - 기본 관리자 검색 인덱스 검색을 찾음.
3. 편집 > 비활성화를 선택함.
4. 비활성화를 클릭함.

### 기본 관리자 검색에는 내부 인덱스가 아닌 모든 인덱스가 포함됩니다

관리자 역할에서 기본적으로 내부 인덱스가 아닌 모든 인덱스를 검색하는 경우 성능이 저하될 수 있음. 관리자 역할에 의해 검색되는 인덱스를 제한하거나 검색을 비활성화하여 이 설정에 대한 메시지를 표시하지 않을 수 있음.

#### 관리자 역할에 의해 검색되는 인덱스 제한2

관리자 역할에서 내부 인덱스가 아닌 모든 인덱스를 검색하지 못하도록 함.

1. 설정 > 액세스 제어를 선택함.
2. 역할을 클릭함.
3. 관리자를 클릭함.
4. 인덱스에서 선택된 인덱스에서 제거할 내부 인덱스가 아닌 모든 인덱스를 클릭함.
5. 저장을 클릭함.

#### 메시지를 표시하지 않도록 검색 비활성화2

관리자 역할에 의해 검색된 인덱스를 제한하지 않지만 메시지를 표시하지 않으려면 검색을 비활성화함.

1. 설정 > 검색, 보고서 및 경고를 선택함.
2. 감사(Audit) - 기본 관리자 검색 내부 인덱스가 아닌 모든 검색을 찾음.
3. 편집 > 비활성화를 선택함.
4. 비활성화를 클릭함.

## Splunk Enterprise Security에서 인텔리전스 다운로드 실패 문제 해결

위협 리스트가 다운로드되지 않았다는 메시지가 표시되는 경우 여러 가지 가능한 근본 원인이 있음.

|가능한 근본 원인|확인|해결 방법|
|:--|:--|:--|
|IP 주소 또는 URL에서 더 이상 위협 또는 인텔리전스 소스를 사용할 수 없음.|URL을 방문하거나 위협 소스를 수동으로 curl하려고 시도함.|인텔리전스 소스를 더 이상 다운로
드할 수 없는 경우 비활성화함.|
|방화벽 또는 프록시 설정으로 인해 인텔리전스 소스에 액세스할 수 없음.|다른 컴퓨터에서 URL를 방문하거나 수동으로 인텔리전스 소스를 curl할 수 있는지 테스트.|인텔리전스 소스에 액세스할 수 있도록 방화벽 또는 프록시 설정을 수 정함.|

### Splunk Enterprise Security에서 대시보드 문제 해결

Enterprise Security의 각 대시보드는 여러 데이터 모델의 데이터를 참조함. 관련 데이터가 없으면 대시보드가 계속 비어 있음. 표시되어야 하는 데이터가 있거나 기대한 것보다 더 오래된 데이터가 표시되는 경우, 다음 문제 해결 절차를 따르십시오.

1. 데이터 모델을 검색함. 대시보드 뷰의 왼쪽 하단 모서리에 있는 검색에서 열기를 클릭하여 데이터 모델을 직접 검색함. 새 검색 대시보드에는 특정 뷰를 채우는 데 사용된 검색 명령어와 개체도 표시됨.
2. 검색 결과가 없는 경우, 대시보드에 필요한 데이터 중에 데이터 모델에서 사용 가능한 데이터가 있는지 확인.
    1. 대시보드에서 사용되는 데이터 모델 데이터 집합을 확인하려면 이 매뉴얼에서 대시보드 요구사항 표를 참조.
    2. 데이터 모델 및 데이터 모델 데이터 집합을 사용하여 데이터 모델에서 이벤트를 검색함.
    <table>
    <tr><td>작업</td><td>검색</td><td>예상되는 결과</td></tr>
    <tr><td>데이터가 Common Information Model로정규화되는지 확인</td><td>
    | datamodel data_model_name root_object_name search | table _time, sourcetype, root_object_name.*
    예:
    | datamodel Network_Traffic All_Traffic search | dedup sourcetype | table _time, sourcetype, All_Traffic.*
    </td><td>sourcetype과 해당 sourcetype으로 채워지는 데이터 모델 개체 및 필드의 리스트 반환</td></tr>
    </table>
3. 사용 가능한 데이터가 없는 경우 데이터 모델이 가속되고 있는지 확인.
    1. Enterprise Security에서 감사(audit) > 데이터 모델 감사(audit)로 이동함.
    2. 가속 세부정보 패널을 검토하여 최근에 데이터 모델 가속가 수행된 시간이나 가속 100% 완료 여부 등 데이터 모델 가속 상태에 대한 정보를 확인. 설치 및 업그레이드 매뉴얼에서 Splunk Enterprise Security의 데이터 모델 설정을 참조.
4. 데이터 모델 가속 상태가 예상한 것과 같으면 추가로 필요한 데이터 원본이 사용 가능한지 확인. 예를 들어 사용자 작업 대시보드에서는 추가 데이터 원본을 사용.
    <table>
    <tr><td>대시보드 이름</td><td>데이터 유형</td><td>데이터 원본</td></tr>
    <tr><td rowspan=3>사용자 작업</td><td>Lookup</td><td>클라우드 도메인, 기업 이메일 도메인, 기업 웹 도메인 Lookup 파일</td></tr>
    <tr><td>ID</td><td>ID 필드; bunit, email, watchlist, work_city, work_country, work_lat 및 work_long. 자세한 내용은 이 매뉴얼에서 ID Lookup 필드를 참조.</td></tr>
    <tr><td>상관(correlation)검색</td><td>* 비기업 도메인을 사용한 대량 이메일 작업</br>* 허용 리스트에 있는 이벤트 관찰됨</br>* 사용자가 웹에서 비기업 사이트에 업로드</td></tr>
    <tr><td>액세스 이상</td><td>상관(correlation)검색</td><td>* 사용자에게 불가능한 여행 이벤트 탐지됨</td></tr>
    </table>

## Splunk Enterprise Security 대시보드 요구사항 표

Enterprise Security 대시보드는 CIM(Common Information Model)을 준수하는 이벤트에 의존하며, 달리 명시되어 있지 않은 한 데이터 모델 가속를 사용하여 채워짐.

대시보드 패널과 데이터 모델

### A - E

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=2>Access Anomalies</td><td>Geographically Improbable Accesses</td><td>Relies on the gia_summary summary index, which is populated by the Access - Geographically Improbable Access - Summary Gen search. That search references the Authentication data model.</td><td>Authentication.app, .src, .user</td></tr>
<tr><td>Concurrent Application Accesses</td><td>Authentication</td><td>Authentication.app, .src, .user</td></tr>
<tr><td rowspan=4>Access Center</td><td>Access Over Time By Action</td><td rowspan=4>Authentication</td><td>Authentication.action</td></tr>
<tr><td>Access Over Time By App</td><td>Authentication.app</td></tr>
<tr><td>Top Access By Source</td><td>Authentication.src</td></tr>
<tr><td>Top Access By Unique User</td><td>Authentication.user,.src</td></tr>
<tr><td>Access Search</td><td></td><td></td><td>Authentication.action, .app, src, .dest, .user, src_user</td></tr>
<tr><td rowspan=4>Access Tracker</td><td>First Time Access - Last 7 days</td><td rowspan=3 colspan=2>None. Calls access_tracker lookup</td></tr>
<tr><td>Inactive Account Usage - Last 90 days</td></tr>
<tr><td>Completely Inactive Accounts - Last 90 days</td></tr>
<tr><td>Account Usage For Expired Identities - Last 7 days</td><td>Authentication</td><td>Authentication.dest</td></tr>
<tr><td rowspan=4>Account Management</td><td>Account Management Over Time</td><td rowspan=4>Change Analysis</td><td>All_Changes.Account_Management, .action</td></tr>
<tr><td>Account Lockouts</td><td>All_Changes.Account_Management, .result</td></tr>
<tr><td>Account Management By Source User</td><td>All_Changes.Account_Management, .src_user</td></tr>
<tr><td>Top Account Management Events</td><td>All_Changes.Account_Management, .action</td></tr>
<tr><td rowspan=4>Asset Center</td><td>Assets By Priority</td><td rowspan=4>Assets And Identities</td><td rowspan=4>All_Assets.priority, .bunit, .category, .owner</td></tr>
<tr><td>Assets By Business Unit</td></tr>
<tr><td>Assets By Category</td></tr>
<tr><td>Asset Information</td></tr>
<tr><td>Asset Investigator</td><td>Asset Investigator</td><td colspan=2>Based on swim lane selection</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=2>액세스 이상</td><td>지리적으로 가능성이 매우 낮은 액세스</td><td rowspan=2>인증</td><td>Authentication.app,.src,.user</td></tr>
<tr><td>동시 애플리케이션 액세스</td><td>Authentication.app, .src, .user</td></tr>
<tr><td rowspan=4>액세스 센터</td><td>작업별 시간에 따른 액세스</td><td rowspan=5>인증</td><td>Authentication.action</td></tr>
<tr><td>앱별 시간에 따른 액세스</td><td>Authentication.app</td></tr>
<tr><td>소스별 상위 액세스</td><td>Authentication.src</td></tr>
<tr><td>고유 사용자별 상위 액세스</td><td>Authentication.user,.src</td></tr>
<tr><td colspan=2>액세스 검색</td><td>Authentication.action,.app,src,.dest,.user,src_user</td></tr>
<tr><td rowspan=4>액세스 추적기</td><td>첫 번째 액세스 - 최근 7일</td><td rowspan=3 colspan=2>없음. access_tracker Lookup 호출</td></tr>
<tr><td>비활성 계정 사용량 - 최근 90일</td></tr>
<tr><td>완전히 비활성화된 계정 - 최근 90일</td></tr>
<tr><td>만료된 ID에 대한 계정 사용량 - 최근 7일</td><td>인증</td><td>Authentication.dest</td></tr>
<tr><td rowspan=4>계정 관리</td><td>시간에 따른 계정 관리</td><td rowspan=4>변경 사항 분석</td><td>All_Changes.Account_Management,.action</td></tr>
<tr><td>계정 잠금</td><td>All_Changes.Account_Management, .result</td></tr>
<tr><td>소스 사용자별 계정 관리</td><td>All_Changes.Account_Management,.src_user</td></tr>
<tr><td>상위 계정 관리 이벤트</td><td>All_Changes.Account_Management,.action</td></tr>
<tr><td rowspan=4>자산 센터</td><td>우선순위별 자산</td><td rowspan=4>자산 및 ID</td><td rowspan=4>All_Assets.priority, .bunit, .category,.owner</td></tr>
<tr><td>사업 부문별 자산</td></tr>
<tr><td>범주별 자산</td></tr>
<tr><td>자산 정보</td></tr>
<tr><td>Asset Investigator</td><td>Asset Investigator</td><td colspan=2>스윔 레인(Swim lane) 선택에 따름</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=2>데이터 보호</td><td>인덱스별 데이터 무결성 제어</td><td colspan=2>인시던트 관리</td></tr>
<tr><td>민감한 데이터</td><td colspan=2>없음. 데이터 무결성 컨트롤이 있는지 확인하는 인덱스에서 REST 검색 호출</td></tr>
<tr><td rowspan=3>기본 계정 작업</td><td>앱별 시간에 따른 기본 계정 사용량</td><td rowspan=2>인증</td><td>Authentication.Default_Authentication,.action, .app</td></tr>
<tr><td>사용 중인 기본 계정</td><td>Authentication.user_category, .dest,.user</td></tr>
<tr><td>기본 로컬 계정</td><td colspan=3>없음. useraccounts_tracker Lookup 호출</td></tr>
<tr><td rowspan=5>DNS 작업</td><td>고유 소스별 상위 응답 코드</td><td rowspan=6>네트워크 분석 DNS</td><td>DNS.message_type, DNS.reply_code</td></tr>
<tr><td>상위 DNS 쿼리 소스</td><td>DNS.message_type, DNS.src</td></tr>
<tr><td>상위 DNS 쿼리</td><td>DNS.message_type, DNS.query</td></tr>
<tr><td>도메인별 쿼리</td><td>DNS.message_type, DNS.query</td></tr>
<tr><td>최근 DNS 쿼리</td><td>DNS.message_type</td></tr>
<tr><td colspan=2>DNS 검색</td><td>DNS.message_type,DNS.reply_code,DNS.dest, DNS.src,DNS.query_type, DNS.query,DNS.answer</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>이메일 작업</td><td>상위 이메일 소스</td><td rowspan=5>이메일</td><td>All_Email.src</td></tr>
<tr><td>대용량 이메일</td><td>All_Email.size, src, .src_user, .dest</td></tr>
<tr><td>거의 보이지 않는 발신자</td><td>All_Email.protocol, .src, .src_user,.recipient</td></tr>
<tr><td>거의 보이지 않는 수신자</td><td>All_Email.protocol, .src,.recipient</td></tr>
<tr><td colspan=2>이메일 검색</td><td>All_Email.protocol, .recipient, .src, .src_user, .dest</td></tr>
<tr><td rowspan=3>endpoint 변경 사항</td><td>작업별 endpoint 변경 사항</td><td rowspan=3>변경 사항 분석</td><td>All_Changes.Endpoint_Changes,.action</td></tr>
<tr><td>유형별 endpoint 변경 사항</td><td>All_Changes.Endpoint_Changes,.object_category</td></tr>
<tr><td>시스템별 endpoint 변경 사항</td><td>All_Changes.Endpoint_Changes,.object_category, .dest</td></tr>
</table>

### F - M

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>포워더 감사(audit)</td><td>호스트별 시간에 따른 이벤트 수</td><td colspan=2 rowspan=2>없음. host_eventcount 매크로 및 검색 호출</td></tr>
<tr><td>마지막 보고 시간별 호스트</td></tr>
<tr><td>Splunkd 프로세스 이용률</td><td rowspan=2>애플리케이션 상태</td><td>All_Application_State.Processes.cpu_load_percent,.mem_used, .process, All_Application_State.dest</td></tr>
<tr><td>Splunk 서비스 시작 모드</td><td>All_Application_State.Services.start_mode, .status,.service</td></tr>
<tr><td rowspan=2>HTTP 범주 분석</td><td>범주 분포</td><td rowspan=2>웹</td><td>Web.src, .category</td></tr>
<tr><td>범주 세부정보</td><td>Web.src, .dest, .category,</td></tr>
<tr><td rowspan=2>HTTP 사용자 에이전트 분석</td><td>사용자 에이전트 분포</td><td rowspan=2>웹</td><td>Web.http_user_agent_length, .http_user_agent</td></tr>
<tr><td>사용자 에이전트 세부정보</td><td>Web.http_user_agent_length, .src, .dest,
.http_user_agent</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>ID 센터</td><td>우선순위별 ID</td><td rowspan=4>자산 및 ID</td><td rowspan=4>All_Identities.priority, .bunit, .category</td></tr>
<tr><td>사업 부문별 ID</td></tr>
<tr><td>범주별 ID</td></tr>
<tr><td>ID 정보</td></tr>
<tr><td>Identity Investigator</td><td>Identity Investigator</td><td colspan=2>스윔 레인(Swim lane) 선택에 따름</td></tr>
<tr><td rowspan=5>인시던트 검토 감사(audit)</td><td>검토자별 검토 작업</td><td rowspan=5 colspan=2>
없음. es_notable_events KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td>상위 검토자</td></tr>
<tr><td>상태별 주요 이벤트 - 최근 48시간</td></tr>
<tr><td>소유자별 주요 이벤트 -최근 24시간</td></tr>
<tr><td>최근 검토 작업</td></tr>
<tr><td rowspan=3>인덱싱 감사(audit)</td><td>시간에 따른 일별 이벤트</td><td rowspan=3 colspan=2>없음. licensing_epd KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td>일별 이벤트</td></tr>
<tr><td>인덱스별 이벤트(마지막 날)</td></tr>
<tr><td rowspan=4>침입 센터</td><td>심각도별 시간에 따른 공격</td><td rowspan=5>침입 탐지</td><td>IDS_Attacks.severity</td></tr>
<tr><td>상위 공격</td><td>IDS_Attacks.dest, .src, .signature</td></tr>
<tr><td>스캔 작업(여러 공격)</td><td>IDS_Attacks.signature</td></tr>
<tr><td>새로운 공격</td><td>IDS_Attacks.ids_type</td></tr>
<tr><td colspan=2>침입 검색</td><td>IDS_Attacks.severity, .category, .signature, .src, .dest</td></tr>
<tr><td rowspan=5>조사</td><td>조사</td><td colspan=2>없음. 조사 KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td>조사 시간 표시줄</td><td colspan=2>없음. investigation_event KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td>조사 메모 첨부 파일</td><td colspan=2>없음. investigation_attachment KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td>작업 이력</td><td colspan=2>없음. 다섯 개 중 하나의 검색 호출 Splunk Enterprise Security에서 조사관리를 참조.</td></tr>
<tr><td>조사 워크벤치 아티팩트</td><td colspan=2>없음. investigation_leads KV 스토어 컬렉션을 통해 검색 호출</td></tr>
<tr><td rowspan=19>조사 워크벤치</td><td>인증 데이터</td><td>인증</td><td>Authentication.app, .action, .src, .src_user, .dest, .user</td></tr>
<tr><td>인증서 활동</td><td>인증서</td><td>Certificates.SSL, .src, .src_port, .dest, .dest_port,.ssl_is_valid, .ssl_validity_window, .ssl_hash,.ssl_serial, .ssl_subject, .ssl_start_time, .ssl_end_time</td></tr>
<tr><td>컴퓨터 인벤토리</td><td>인벤토리</td><td>Compute_Inventory.All_Inventory, .os, .vendor_product,.user, .dest</td></tr>
<tr><td>DNS 데이터</td><td>네트워크 분석 DNS</td><td>Network_Resolution.DNS, DNS.dest, .query,.query_count, .message_type, .answer, .reply_code</td></tr>
<tr><td>이메일 데이터</td><td>이메일</td><td>Email.All_Email, .src, .dest, .src_user, .action, .recipient,.recipient_count, .subject</td></tr>
<tr><td>파일 시스템 변경 사항</td><td>변경 사항 분석</td><td>Change_Analysis.All_Changes, .user, .dest, .action, .status,
All_Changes.Endpoint_Changes.Filesystem_Changes, .file_name, .file_hash, .file_path, .file_size, .file_create_time, .file_modify_time, .file_access_time</td></tr>
<tr><td>IDS 경고</td><td>침입 탐지</td><td>Intrusion_Detection.IDS_Attacks, .user, .src, .dest,.severity, .category, .signature, .ids_type,.vendor_product, .dvc</td></tr>
<tr><td>최신 OS 업데이트</td><td>업데이트</td><td>Updates.status, .user, .dest, .signature_id, .signature,.vendor_product</td></tr>
<tr><td>네트워크 세션 데이터</td><td>네트워크 세션</td><td>Network_Sessions.All_Sessions, .src_ip, .dest_ip, .dest_nt_host, .tag, .action, .vendor_product</td></tr>
<tr><td>네트워크 트래픽 데이터</td><td>네트워크 트래픽</td><td>Network_Traffic.All_Traffic, .packets, .src_ip, .dest_ip, .user, .transport, .action, .src, .src_port, .dest, .dest_port</td></tr>
<tr><td>주요 이벤트</td><td>인시던트 관리</td><td>Incident_Management.Notable_Events, .user, .src, .dest, .rule_name, .severity, .urgency, .security_domain, .status_label, .owner, .savedsearch_description</td></tr>
<tr><td>포트 활동</td><td>애플리케이션 상태</td><td>Application_State.Ports, .dest, .user, dest_port,.transport, .process_name, .process</td></tr>
<tr><td>프로세스 활동</td><td>애플리케이션 상태</td><td>Application_State.All_Application_State, .dest, .user, .process_name, .process</td></tr>
<tr><td>레지스트리 활동</td><td>변경 사항 분석</td><td>Change_Analysis_All_Changes, .user, .dest, .action,.status, .object, object_path, .object_attrs, .object_id,
.Endpoint_Changes.Registry_Changes</td></tr>
<tr><td>위험 점수</td><td>위험 분석</td><td>Risk.All_Risk, .risk_score, .risk_object_type,.risk_object</td></tr>
<tr><td>서비스 작업</td><td>애플리케이션 상태</td><td>Application_State.Services, .dest, .user, .service,.service_id, .status, .start_mode, .process_name, .process</td></tr>
<tr><td>시스템 취약점</td><td>취약점</td><td>Vulnerabilities.Vulnerabilities, .user, .dest, .severity,.signature, .category, .vendor_product</td></tr>
<tr><td>사용자 계정 변경 사항</td><td>변경 사항 분석</td><td>Change_Analysis.All_Changes, .user, .dest, .action,
.status, .object, .object_path, .object_attrs, .object_id,.Account_Management</td></tr>
<tr><td>웹 활동</td><td>웹</td><td>Web.Web, .src, .dest, .user, .action, .http_method, .url,.http_referrer, .http_user_agent, .http_content_type,
.status</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>멀웨어 센터</td><td>작업별 시간에 따른 멀웨어 작업</td><td rowspan=3>멀웨어</td><td>Malware_Attacks.action</td></tr>
<tr><td>서명별 시간에 따른 멀웨어 작업</td><td>Malware_Attacks.signature</td></tr>
<tr><td>상위 감염</td><td>Malware_Attacks.signature, .dest</td></tr>
<tr><td>새로운 멀웨어 - 최근 30일</td><td colspan=2>없음. malware_tracker Lookup 호출</td></tr>
<tr><td rowspan=4>멀웨어 작업</td><td>제품 버전별 클라이언트</td><td rowspan=3  colspan=2>없음. malware_operations_tracker Lookup 호출</td></tr>
<tr><td>서명 버전별 클라이언트</td></tr>
<tr><td>가장 오래된 감염</td></tr>
<tr><td>반복적인 감염</td><td rowspan=2>멀웨어</td><td>Malware_Attacks.action, .signature, .dest</td></tr>
<tr><td colspan=2>멀웨어 검색</td><td>Malware_Attacks.action, .file_name, .user,
.signature, .dest</td></tr>
<tr><td rowspan=3>모듈식 작업 센터</td><td>이름별 시간에 따른 작업 호출</td><td rowspan=3>Splunk 감사 (audit) 로그</td><td>Modular_Actions.Modular_Action_Invocations,.action_name</td></tr>
<tr><td>이름별 상위 작업</td><td>Modular_Actions.Modular_Action_Invocations,
.action_mode, .user, .duration, .search_name,.rid, .sid</td></tr>
<tr><td>검색별 상위 작업</td><td>Modular_Actions.Modular_Action_Invocations,
.action_name, .action_mode, .user,.search_name, .rid, .sid</td></tr>
</table>

### N - S

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=2>네트워크 변경 사항</td><td>작업별 네트워크 변경 사항</td><td rowspan=2>변경 사항 분석</td><td>All_Changes.Network_Changes,.action</td></tr>
<tr><td>장치별 네트워크 변경 사항</td><td>All_Changes.Network_Changes, .dvc</td></tr>
<tr><td rowspan=4>새로운 도메인 분석</td><td>새로운 도메인 작업</td><td rowspan=3>웹</td><td rowspan=3>Web.dest</td></tr>
<tr><td>기간별 새로운 도메인 작업</td></tr>
<tr><td>TLD별 새로운 도메인 작업</td></tr>
<tr><td>등록 세부정보</td><td colspan=2>없음</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>포트 및 프로토콜 추적기</td><td>포트/프로토콜 프로파일러</td><td rowspan=4>네트워크 트래픽</td><td>All_Traffic.transport, .dest_port</td></tr>
<tr><td>시간에 따른 금지된 트래픽 또는 안전하지 않은 트래픽 - 최근 24시간</td><td>All_Traffic.src_category,
.dest_category, .src, .dest, .transport,
.dest_port</td></tr>
<tr><td>금지된 트래픽 세부정보 - 최근 24시간</td><td>All_Traffic.src_category,
.dest_category, .src, .dest, .transport,.dest_port</td></tr>
<tr><td>새로운 포트 작업 - 최근 7일</td><td>없음. 애플리케이션 프로토콜 Lookup 호출</td></tr>
<tr><td rowspan=5>프로토콜 센터</td><td>프로토콜별 연결</td><td rowspan=5>네트워크 트래픽</td><td>All_Traffic.app</td></tr>
<tr><td>프로토콜별 사용량</td><td>All_Traffic.app, .bytes</td></tr>
<tr><td>상위 연결 소스</td><td>All_Traffic.src</td></tr>
<tr><td>잘 알려진 포트에 대한 사용량</td><td>All_Traffic.bytes, .dest_port</td></tr>
<tr><td>장시간 운용 가능한 연결</td><td>All_Traffic.src, .src_port, .duration, .dest, .dest_port, .transport</td></tr>
<tr><td rowspan=4>위험 분석</td><td>시간에 따른 위험 수정자</td><td rowspan=4>위험 분석</td><td>All_Risk.risk_score</td></tr>
<tr><td>개체별 위험 점수</td><td>All_Risk.risk_score</td></tr>
<tr><td>가장 활성화된 소스</td><td>All_Risk.risk_score, .risk_object</td></tr>
<tr><td>최근 위험 수정자</td><td>All_Risk.*</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>보안 포스처</td><td>긴급도별 주요 이벤트</td><td rowspan=4 colspan=2>없음. es_notable_events KVStore 컬렉션을 통해 검색 호출</td></tr>
<tr><td>시간에 따른 주요 이벤트</td></tr>
<tr><td>상위 주요 이벤트</td></tr>
<tr><td>상위 주요 이벤트 소스</td></tr>
<tr><td rowspan=2>세션 센터</td><td>시간에 따른 세션</td><td rowspan=2>네트워크 세션</td><td>All_Sessions.Session_*</td></tr>
<tr><td>세션 세부정보</td><td>All_Sessions.*</td></tr>
<tr><td rowspan=3>SSL 작업</td><td>공통 이름별 SSL 작업</td><td rowspan=4>인증서</td><td>All_Certificates.SSL.ssl_subject_common_name</td></tr>
<tr><td>SSL 클라우드 세션</td><td>All_Certificates.SSL.ssl_subject_common_name, .src,</td></tr>
<tr><td>최근 SSL 세션</td></tr>
<tr><td colspan=2>SSL 검색</td><td>All_Certificates.src, .dest,.ssl_subject_common_name,.ssl_subject_email,.ssl_issuer_common_name,.ssl_issuer_organization, .ssl_start_time,.ssl_end_time, .ssl_validity_window, .ssl_is_valid</td></tr>
<tr><td rowspan=4>제거 감사(audit)</td><td>시간에 따른 제거된 이벤트 -최근 24시간</td><td rowspan=4>없음</td><td>주요 이벤트를 검색하기 위한 매크로 호출</td></tr>
<tr><td>시간에 따른 제거 내역 - 최근 30일</td><td>요약 생성 정보에 대한 매크로 및 검색 호출</td></tr>
<tr><td>제거 관리 작업</td><td>eventtype별 검색 호출</td></tr>
<tr><td>만료된 제거</td><td>eventtype별 검색 호출</td></tr>
<tr><td rowspan=4>시스템 센터</td><td>운영 체제</td><td colspan=2>없음. system_version_tracker Lookup 호출</td></tr>
<tr><td>시스템별 상위 평균 CPU 로드</td><td>성능</td><td>All_Performance.CPU.cpu_load_percent,All_Performance.dest</td></tr>
<tr><td>시스템 수별 서비스</td><td rowspan=2>애플리케이션 상태</td><td>All_Application_State.Services</td></tr>
<tr><td>시스템 수별 포트</td><td>All_Application_State.Ports</td></tr>
</table>

### T - Z

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>위협 활동</td><td>시간에 따른 위협 활동</td><td rowspan=4 colspan=2>침입 탐지, 네트워크 트래픽, 웹. 자세한 내용은 위협 활동 데이터 소스를 참조.</td></tr>
<tr><td>가장 활동적인 위협 컬렉션</td></tr>
<tr><td>가장 활동적인 위협 소스</td></tr>
<tr><td>위협 활동 세부정보</td></tr>
<tr><td rowspan=5>위협 아티팩트</td><td>위협 개요</td><td rowspan=5 colspan=2>없음. 위협 인텔리전스 KV 스토어 컬렉션을 호출함. 위협 인텔리전스 컬렉션 리스트는 Splunk Enterprise Security에서 지원되는 위협 인텔리전스 유형을 참조.</td></tr>
<tr><td>endpoint 아티팩트</td></tr>
<tr><td>네트워크 아티팩트</td></tr>
<tr><td>이메일 아티팩트</td></tr>
<tr><td>인증서 아티팩트</td></tr>
<tr><td rowspan=2>위협 인텔리전스 감사(audit)</td><td>위협 인텔리전스 다운로드</td><td colspan=5>없음. REST endpoint별로 검색 호출</td></tr>
<tr><td>위협 인텔리전스 감사(audit) 이벤트</td><td colspan=2>없음. eventtype별 검색 호출</td></tr>
<tr><td rowspan=4>시간 센터</td><td>시간 동기화 실패</td><td rowspan=2>성능</td><td rowspan=2>All_Performance.OS.Timesync,
All_Performance.dest,.dest_should_timesync,OS.Timesync.action</td></tr>
<tr><td>시간 동기화하지 않은 시스템</td></tr>
<tr><td>인덱싱 시간 지연</td><td colspan=2>없음. Summary Gen 검색 결과 호출</td></tr>
<tr><td>시간 서비스 시작 모드 이상</td><td>애플리케이션 상태</td><td>All_Application_State.Services.start_mode,.Services.status, .dest_should_timesync,.tag, .dest</td></tr>
<tr><td rowspan=4>트래픽 센터</td><td>작업별 시간에 따른 트래픽</td><td rowspan=5>네트워크 트래픽</td><td>All_Traffic.action</td></tr>
<tr><td>프로토콜별 시간에 따른 트래픽</td><td>All_Traffic.transport</td></tr>
<tr><td>스캔 작업(여러 시스템)</td><td>All_Traffic.dest, .src</td></tr>
<tr><td>상위 소스</td><td>All_Traffic.src</td></tr>
<tr><td colspan=2>트래픽 검색</td><td>All_Traffic.action, .src_port, .src, .dest,
.transport, .dest_port</td></tr>
<tr><td rowspan=2>트래픽 크기 분석</td><td>시간에 따른 트래픽 크기 이상</td><td rowspan=2>네트워크 트래픽</td><td>All_Traffic.transport, .src</td></tr>
<tr><td>트래픽 크기 세부정보</td><td>All_Traffic.bytes, .dest, .src</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=4>업데이트 센터</td><td>업데이트가 필요한 상위 시스템</td><td rowspan=3>업데이트</td><td>Updates.status, .dest, .signature_id,.vendor_product</td></tr>
<tr><td>필요한 상위 업데이트</td><td>Updates.status, .dest, .signature_id,.vendor_product</td></tr>
<tr><td>업데이트하지 않은 시스템 - 30일 이상</td><td>Updates.dest_should_update, .dest,
.signature_id, .vendor_product, .status</td></tr>
<tr><td>업데이트 서비스 시작 모드 이상</td><td>애플리케이션 상태</td><td>All_Application_State.Services.start_mode,.Services.status, .Services.service, .tag</td></tr>
<tr><td colspan=2>업데이트 검색</td><td>업데이트</td><td>Updates.dest_should_update, .status,.dest, .signature_id, .vendor_product</td></tr>
<tr><td rowspan=2>URL 길이 분석</td><td>시간에 따른 URL 길이 이상</td><td rowspan=2>웹</td><td>Web.http_method, .url</td></tr>
<tr><td>URL 길이 세부정보</td><td>Web.url_length, .src, .dest, .url</td></tr>
<tr><td rowspan=6>사용자 작업</td><td>위험 점수별 사용자</td><td>위험 분석</td><td>All_Risk.risk_object</td></tr>
<tr><td>비기업 웹 업로드</td><td>웹</td><td>Web.bytes, .user, .http_method, .url</td></tr>
<tr><td>비기업 이메일 작업</td><td>이메일</td><td>All_Email.size, .recipient, .src_user,</td></tr>
<tr><td>관심 대상 사이트 작업</td><td>웹</td><td>Web.src, .url</td></tr>
<tr><td>원격 액세스</td><td>인증</td><td>Authentication.src, .user</td></tr>
<tr><td>티켓 작업</td><td>티켓 관리</td><td>All_Ticket_Management.description,.priority, . severity, .src_user</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>패널 제목</td><td>데이터 모델</td><td>데이터 모델 데이터 집합</td></tr>
<tr><td rowspan=2>뷰 감사(audit)</td><td>시간에 따른 뷰 작업</td><td rowspan=2>Splunk 감사(audit) 로그</td><td>View_Activity.app, .view</td></tr>
<tr><td>예상된 뷰 작업</td><td>View_Activity.app, .view, .user</td></tr>
<tr><td rowspan=4>취약성 센터</td><td>상위 취약성</td><td rowspan=3>취약점</td><td>Vulnerabilities.signature, .dest</td></tr>
<tr><td>가장 취약한 호스트</td><td>Vulnerabilities.signature, .severity,.dest</td></tr>
<tr><td>심각도별 취약성</td><td>Vulnerabilities.signature, .severity,.dest</td></tr>
<tr><td>새로운 취약성</td><td colspan=2>vuln_signature_reference Lookup 호출</td></tr>
<tr><td rowspan=3>취약성 작업</td><td>시간에 따른 스캔 작업</td><td>취약점</td><td>Vulnerabilities.dest</td></tr>
<tr><td>기간별 취약성</td><td colspan=2>vulnerability_tracker Lookup 호출</td></tr>
<tr><td>지체된 스캐닝</td><td rowspan=2>취약점</td><td>Vulnerabilities.dest</td></tr>
<tr><td colspan=2>취약성 검색</td><td>Vulnerabilities.category, .signature,.dest, .severity, .cve,</td></tr>
<tr><td rowspan=4>웹 센터</td><td>메서드별 시간에 따른 이벤트</td><td rowspan=5>웹</td><td>Web.http_method</td></tr>
<tr><td>상태별 시간에 따른 이벤트</td><td>Web.status</td></tr>
<tr><td>상위 소스</td><td>Web.dest, .src</td></tr>
<tr><td>상위 대상</td><td>Web.dest, .src</td></tr>
<tr><td colspan=2>웹 검색</td><td>Web.http_method, .status, .src, .dest,.url</td></tr>
</table>

### 추가할 대시보드

다음 대시보드는 Splunk Enterprise Security에 포함되어 있음. 탐색 편집기를 사용하여 대시보드를 메뉴 모음에(서) 추가하거나 재배열함. 탐색 편집기 사용에 대한 자세한 내용은 Splunk Enterprise Security의 메뉴 모음 사용자 지정을 참조.

Enterprise Security의 전체 대시보드 리스트를 보려면 검색 > 대시보드를 선택.
Enterprise Security의 대시보드 리스트를 추가 기능별로 보려면 콘텐츠 프로파일 대시보드를 사용. 콘텐츠 프로파일을 참조.

<table>
<tr><td>대시보드 이름</td><td>보안 도메인</td><td>추가 기능의 일부분</td></tr>
<tr><td>Access Anomalies</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>Access Center</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>Access Search</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>Access Tracker</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>Account Management</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>Asset Center</td><td>Asset</td><td>SA-IdentityManagement</td></tr>
<tr><td>Asset Investigator</td><td>Asset</td><td>SA-IdentityManagement</td></tr>
<tr><td>Content Profile</td><td>Audit</td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>Data Model Audit</td><td>Audit</td><td>Splunk_SA_CIM</td></tr>
<tr><td>Default Account Activity</td><td>Access</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>DNS Activity</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>DNS Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Email Activity</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Email Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Endpoint Changes</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Forwarder Audit</td><td>Audit</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>HTTP Category Analysis</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>HTTP User Agent Analysis</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Identity Center</td><td>Identity</td><td>SA-IdentityManagement</td></tr>
<tr><td>Identity_investigator</td><td>Identity</td><td>SA-IdentityManagement</td></tr>
<tr><td>Incident Review</td><td>Threat</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>Incident Review Audit</td><td>Threat</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>Indexing Audit</td><td>Audit</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>Intrusion Center</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Intrusion Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Malware Center</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Malware Operations</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Malware Search</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Network Changes</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>New Domain Analysis</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Per-Panel Filter Audit</td><td>Audit</td><td>SA-Utils</td></tr>
<tr><td>Port & Protocol Tracker</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Predictive Analytics</td><td></td><td>Splunk_SA_CIM</td></tr>
<tr><td>Protocol Center</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>REST Audit</td><td>Audit</td><td>SA-Utils</td></tr>
<tr><td>Risk Analysis</td><td>Threat</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>Search Audit</td><td>Audit</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>Security Posture</td><td></td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>Session Center</td><td>Identity</td><td>SA-IdentityManagement</td></tr>
<tr><td>SSL Activity</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>SSL Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Suppression Audit</td><td>Threat</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>System Center</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Threat Activity</td><td>Threat</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>Threat Artifacts</td><td>Threat</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>Threat Intelligence Audit</td><td>Audit</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>Time Center</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Traffic Center</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Traffic Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Traffic Size Analysis</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Update Center</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>Update Search</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>URL Length Analysis</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>User Activity</td><td>Identity</td><td>DA-ESS-IdentityManagement</td></tr>
<tr><td>View Audit</td><td>Audit</td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>Vulnerability Center</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Vulnerability Operations</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Vulnerability Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Web Center</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>Web Search</td><td>Network</td><td>DA-ESS-NetworkProtection</td></tr>
</table>

<table>
<tr><td>대시보드 이름</td><td>보안 도메인</td><td>추가 기능의 일부분</td></tr>
<tr><td>액세스 이상</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>액세스 센터</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>액세스 검색</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>액세스 추적기</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>계정 관리</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>자산 센터</td><td>자산</td><td>SA-IdentityManagement</td></tr>
<tr><td>Asset Investigator</td><td>자산</td><td>SA-IdentityManagement</td></tr>
<tr><td>콘텐츠 프로파일</td><td>감사(audit)</td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>데이터 모델</td><td>감사(audit) 감사(audit)</td><td>Splunk_SA_CIM</td></tr>
<tr><td>기본 계정 작업</td><td>액세스</td><td>DA-ESS-AccessProtection</td></tr>
<tr><td>DNS 작업</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>DNS 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>이메일 작업</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>이메일 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>endpoint 변경 사항</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>포워더 감사(audit)</td><td>감사(audit)</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>HTTP 범주 분석</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>HTTP 사용자 에이전트 분석</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>ID 센터</td><td>ID</td><td>SA-IdentityManagement</td></tr>
<tr><td>Identity_investigator</td><td>ID</td><td>SA-IdentityManagement</td></tr>
<tr><td>인시던트 검토</td><td>위협</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>인시던트 검토</td><td>감사(audit) 위협</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>인덱싱 감사(audit)</td><td>감사(audit)</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>침입 센터</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>침입 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>멀웨어 센터</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>멀웨어 작업</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>멀웨어 검색</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>네트워크 변경 사항</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>새로운 도메인 분석</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>패널당 필터 감사(audit)</td><td>감사(audit)</td><td>SA-Utils</td></tr>
<tr><td>포트 및 프로토콜 추적기</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>예측 분석</td><td></td><td>Splunk_SA_CIM</td></tr>
<tr><td>프로토콜 센터</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>REST 감사(audit)</td><td>감사(audit)</td><td>SA-Utils</td></tr>
<tr><td>위험 분석</td><td>위협</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>검색 감사(audit)</td><td>감사(audit)</td><td>SA-AuditAndDataProtection</td></tr>
<tr><td>보안 포스처</td><td></td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>세션 센터</td><td>ID</td><td>SA-IdentityManagement</td></tr>
<tr><td>SSL 작업</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>SSL 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>제거 감사(audit)</td><td>위협</td><td>SA-ThreatIntelligence</td></tr>
<tr><td>시스템 센터</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>위협 활동</td><td>위협</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>위협 아티팩트</td><td>위협</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>위협 인텔리전스 감사(audit)</td><td>감사(audit)</td><td>DA-ESS-ThreatIntelligence</td></tr>
<tr><td>시간 센터</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>트래픽 센터</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>트래픽 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>트래픽 크기 분석</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>업데이트 센터</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>업데이트 검색</td><td>Endpoint</td><td>DA-ESS-EndpointProtection</td></tr>
<tr><td>URL 길이 분석</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>사용자 작업</td><td>ID</td><td>DA-ESS-IdentityManagement</td></tr>
<tr><td>뷰 감사(audit)</td><td>감사(audit)</td><td>SplunkEnterpriseSecuritySuite</td></tr>
<tr><td>취약성 센터</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>취약성 작업</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>취약성 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>웹 센터</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
<tr><td>웹 검색</td><td>네트워크</td><td>DA-ESS-NetworkProtection</td></tr>
</table>