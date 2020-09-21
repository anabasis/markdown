# 상관(correlation)검색

## Splunk Enterprise Security 상관(correlation)검색 개요

상관(correlation)검색은 여러 데이터 원본에서 정의된 패턴을 검색함. 검색에서 패턴을 찾으면 adaptive response작업을 수행함.

상관(correlation)검색에서는 보안 영역의 이벤트(액세스, ID, endpoint, 네트워크), 자산 리스트, ID 리스트, 위협 인텔리전스 및 Splunk 플랫폼의 기타 데이터 등 여러 가지 유형의 데이터 원본을 검색함. 그런 다음 검색에서는 SPL의 함수를 사용하여 초기 검색 결과를 집계하고, adaptive response 작업을 통해 검색 조건과 일치하는 이벤트에 대응하는 조치를 취함.

- 상관(correlation)검색을 만드는 방법은 Splunk Enterprise Security 튜토리얼에서 상관(correlation)검색 만들기를 참조.
- 환경에서 상관(correlation)검색을 설정하거나 수정하는 방법은 상관(correlation)검색 설정을 참조.

상관(correlation)검색 예

- ID 리스트와 호스트 또는 장치 인증 시도의 상관 관계를 분석하여 만료된 계정의 액세스 시도를 식별.
- 자산 리스트와 endpoint 보호 시스템의 이벤트 간 상관 관계를 분석하여 특정 멀웨어 감염 수가 많은 다수의 호스트나 멀웨어 감염 수가 많은 단일 호스트를 식별.
- ID 리스트와 호스트 또는 장치 인증 시도의 상관 관계를 분석하여 단일 호스트의 높은 인증 실패 횟수에 이어 인증에 성공하는 패턴을 식별. 그런 다음 검색에 임계값을 적용하여 인증 시도 횟수를 집계함.

## Splunk Enterprise Security에서 상관(correlation)검색 작성

상관(correlation)검색을 직접 만들어서 주요 이벤트를 만들고, 위험 점수를 수정하고, 기타 adaptive response 작업을 이벤트의 상관에 따라 자동으로 수행할 수 있음. Splunk Enterprise Security에서는 다음 두 가지 방법으로 상관(correlation)검색을 만들 수 있음.

- SPL 전문가인 경우 상관(correlation)검색을 수동으로 만듬. 기존 제공 상관(correlation)검색을 검토하여 검색 방법과 사용 가능한 옵션의 예를 찾을 수 있음. 상관(correlation)검색 아이디어를 구현하기 전에 검색 페이지에서 테스트함.
- 상관(correlation)검색 구문에 관한 도움이 더 필요한 경우 가이드식 검색 작성 마법사를 사용하여 상관(correlation)검색을 만듬. 가이드식 검색 작성 마법사를 사용하면 데이터 모델이나 Lookup을 데이터 원본으로 사용하는 상관(correlation)검색을 만들 수 있음. 마법사는 데이터 원본, 시간 범위, 필터링, 집계 함수, 분할 기준 필드 및 기타 조건에 대한 사용자 선택 사항을 사용하여 검색 구문을 자동으로 작성함. 상관(correlation)검색 작성 단계별 튜토리얼은 Splunk Enterprise Security 튜토리얼에서 상관(correlation)검색 작성을 참조.

추가 필드가 사용자 지정 상관(correlation)검색의 주요 이벤트 세부 정보에 표시되도록 하는 방법에 대한 자세한 내용은 주요 이벤트 필드 변경을 참조.

참고 항목은 아래와 같음.

- Splunk Enterprise Security에서 상관(correlation)검색 설정
- Splunk Enterprise Security의 상관(correlation)검색 나열

## Splunk Enterprise Security에서 상관(correlation)검색 설정

상관(correlation)검색을 설정하여 활성화하거나 비활성화하고, 검색 실행 방법과 관련된 설정을 업데이트하고, 검색 논리를 변경하고, 검색에 따른 adaptive response 작업을 조절함. 상관(correlation)검색에 대해 자세히 알아보려면 Splunk Enterprise Security 상관(correlation)검색 개요를 참조.

### 상관(correlation)검색 활성화

상관(correlation)검색을 활성화하여 adaptive response 작업을 실행하고 주요 이벤트를 수신하기 시작.
Splunk Enterprise Security는 모든 상관(correlation)검색이 비활성화된 상태로 설치되므로, 보안 이용 사례와 가장 관련성이 높은 검색을 선택할 수 있음.

1. Splunk ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택.
2. 콘텐츠 관리 페이지를 상관(correlation)검색의 유형을 기준으로 필터링하여 상관(correlation)검색만 표시.
3. 상관(correlation)검색의 이름과 설명을 검토하여 보안 이용 사례를 지원하기 위해 활성화할 검색을 결정함.
    예를 들어 계정 보안 침해가 문제라면 동시 로그인 시도 탐지됨 및 무차별 공격(Brute Force) 액세스 동작이 탐지됨 상관(correlation)검색을 활성화하는 방안을 고려.
4. 작업 컬럼에서 활성화를 클릭하여 활성화하고 싶은 검색을 활성화함.

상관(correlation)검색을 활성화한 후에 주요 이벤트, 위험 점수 및 기타 데이터가 대시보드에 표시되기 시작.

### 상관(correlation)검색 예약 변경

상관(correlation)검색의 기본 검색 유형을 실시간에서 예약으로 변경. Splunk Enterprise Security는 기본적으로 인덱싱된 실시간 검색을 사용.

1. 콘텐츠 관리 페이지에서 변경할 상관(correlation)검색을 찾음.
2. 작업 컬럼에서 예약으로 변경을 클릭.

예약할 검색을 변경한 후 검색의 예약 설정을 수정할 수 있음.

1. 콘텐츠 관리 페이지에서 변경할 상관(correlation)검색의 이름을 클릭.
2. (선택 사항) 검색 일정을 수정.
    상관(correlation)검색을 실시간 또는 연속 예약으로 실행할 수 있음. 실시간 예약을 사용하여 현재 데이터와 성능을 우선시함. 실시간 예약 검색에서는 검색을 예약한 시간에 실행할 수 없는 경우 건너뜀. 실시간 예약 검색에서는 검색을 건너뛸 경우에 발생하는 데이터 공백을 다시 채우지 않음. 연속 예약 검색은 건너뛰지 않으므로 데이터 완전성을 우선시하려면 연속 예약을 사용.
3. (선택 사항) 크론 스케줄을 수정하여 검색 실행 빈도를 관리함.
4. (선택 사항) 검색 예약 창을 지정함. 예약 창을 사용하지 않으려면 0을 입력하거나, 스케줄러에서 설정한 자동 예약 창을 사용하려면 auto를 입력하거나, 예약 창이 지속되길 원하는 분 수에 해당하는 숫자를 입력.
    동시에 실행하도록 설정한 예약된 보고서가 많으면 예약 창을 지정하여 검색 스케줄러가 이 검색을 연기하고 우선 순위가 더 높은 검색을 실행할 수 있게 함.
5. (선택 사항) 검색 예약 우선 순위를 지정함. 이 검색을 실행하는 것과 특정 시간에 실행하는 것이 얼마나 중요한지 에 따라 기본값을 더 높게 또는 가장 높게로 변경.
    예약 우선 순위 설정은 예약 창 설정보다 우선하므로 두 가지를 모두 설정할 필요는 없음.

검색 예약 우선 순위에 대한 내용은 Splunk 플랫폼 매뉴얼을 참조.

- Splunk Enterprise의 경우, Splunk Enterprise 보고 매뉴얼의 Splunk Web에서 동시에 예약된 보고서 우선 순위 지정을 참조.
- Splunk Cloud의 경우, Splunk Cloud 보고 매뉴얼의 Splunk Web에서 동시에 예약된 보고서 우선 순위 지정을 참조.

### 상관(correlation)검색 편집

상관(correlation)검색을 환경에 맞게 변경할 수 있음. 예를 들어 검색에 사용되는 임계값을 수정하거나, 성공적인 상관에 따른 대응 작업을 변경하거나, 검색 실행 빈도를 변경할 수 있음. 상관(correlation)검색을 수정해도 기존 주요 이벤트에는 영향이 없음.

1. 콘텐츠 관리 페이지에서 편집할 상관(correlation)검색을 찾음.
2. 콘텐츠 관리 페이지에서 상관(correlation)검색의 이름을 클릭하여 검색을 편집함.
3. 검색 매개변수를 수정한 다음 저장을 클릭.

상관(correlation)검색 시작 시간 및 종료 시간을 수정하는 경우 상대 시간 한정자를 사용. Splunk Enterprise 검색 매뉴얼의 검색에서 시간 한정자 지정을 참조.

#### 가이드 모드에서 상관 (correlation) 검색 편집

가이드 모드에서 일부 상관(correlation)검색을 편집할 수 있음. 모든 상관(correlation)검색에서 가이드식 검색 편집을
지원하지는 않음. 검색이 회색으로 비활성화되어 표시되고 가이드 모드에서 검색 편집 옵션이 있는 경우, 검색이 가이드 모드에서 작성되었고 가이드 모드에서 편집할 수 있음을 의미함. 검색을 검색란에서 편집할 수 있는 경우 가이드 모드에서 편집할 수 없음. 가이드 모드로 전환하려고 하면 새 검색이 기존 검색을 덮어씀.

1. 가이드 모드에서 검색 편집을 클릭하여 가이드식 검색 작성 마법사를 염.
2. 상관(correlation)검색의 검색 요소를 검토하고, 원할 경우 변경.
3. 검색을 저장함.

### 상관(correlation)검색에서 생성되는 대응 작업 수 조절

조절을 설정하여 상관(correlation)검색에서 생성되는 대응 작업의 수를 제한함. 상관(correlation)검색에서 이벤트와 일치하는 항목을 찾으면 대응 작업이 트리거.
기본적으로 상관(correlation)검색에서 반환하는 모든 결과가 대응 작업을 생성. 일반적으로 특정 유형의 경고가 하나만 필요함. 조절을 사용하여 상관(correlation)검색에서 경고를 일정 기간 내에 2개 이상 생성하지 않도록 할 수 있음. 대응 작업을 생성하는 결과의 유형을 변경하려면 트리거 조건을 정의함. 일부 대응 작업에 대해서는 조절 외에 최대 결과 수도 지정할 수 있음. Splunk Enterprise Security에서 adaptive response 작업 설정을 참조.

1. 설정 > 콘텐츠 관리를 선택.
2. 편집할 상관(correlation)검색의 제목을 클릭.
3. 창 기간을 입력. 이 기간 중에는 그룹화 기준 필드와 일치하는 추가 이벤트가 새 경고를 만들지 않음. 기간이 끝난 후에는 다음으로 일치하는 이벤트가 새 경고를 만들고 조절 조건을 다시 적용.
4. 그룹화 기준 필드를 입력하여 유사 이벤트 일치 시에 사용할 필드를 지정함. 여기에 나열된 필드가 생성된 경고와 일치하는 경우 상관(correlation)검색에서 새 경고를 만들지 않음. 필드를 2개 이상 정의할 수 있음. 사용 가능한 필드는 상관(correlation)검색에서 반환하는 검색 필드에 따라 다름.
5. 상관(correlation)검색을 저장함.

조절은 상관(correlation)검색 대응 작업의 유형에 관계없이 적용되고, 주요 이벤트 제거 전에 수행. 주요 이벤트 제거에 대한 자세한 내용은 주요 이벤트 제거 작성 및 관리를 참조.

### 상관(correlation)검색에 의해 adaptive response 작업이 생성되는 트리거 조건 정의

상관(correlation)검색에 의해 adaptive response 작업이 생성되는 경우를 제어하는 조건을 수정할 수 있음. 조절은 트리거 조건을 정의하는 것과 다르며, 검색 결과가 트리거 조건을 충족한 후에 수행. 트리거 조건을 정의하면 상관(correlation)검색 결과가 조건과 일치하는지 확인하기 하기 위해 평가. 검색 결과가 조건과 일치하는 경우, 조절 규칙에 따라 adaptive response 작업 생성 여부가 제어.

상관(correlation)검색에서 반환되는 결과의 수, 호스트 수나 원본 수 또는 사용자 지정 기준에 따라 대응 작업을 결과별로 생성하는 트리거 조건을 설정할 수 있음. 사용자 지정 기준을 사용하는 경우, 사용자 지정 검색 문자열을 입력하여 조건을 만듬. 트리거 조건은 상관(correlation)검색 결과를 대상으로 한 보조 검색으로 사용.

트리거 조건과 검색에 대해 트리거 조건을 설정하는 방법에 대한 내용은 Splunk 플랫폼 매뉴얼을 참조.

- Splunk Enterprise의 경우, Splunk Enterprise 경고 매뉴얼에서 경고 트리거 조건 설정을 참조.
- Splunk Cloud의 경우, Splunk Cloud 경고 매뉴얼에서 경고 트리거 조건 설정을 참조.

참고 항목은 아래와 같음.

- Splunk Enterprise Security의 상관(correlation)검색 나열
- Splunk Enterprise Security에서 adaptive response 작업 설정

## Splunk Enterprise Security의 상관(correlation)검색 나열

Splunk Enterprise Security에서 사용 가능한 상관(correlation)검색의 리스트를 얻으려면 REST 검색을 사용하여 테이블에 포함시킬 정보를 추출함.

예를 들어 환경의 앱, 보안 도메인, 이름과 모든 상관(correlation)검색에 대한 설명이 포함된 테이블을 만들 수 있음.

```sql
| rest splunk_server=local count=0 /services/saved/searches | where match('action.correlationsearch.enabled',
"1|[Tt]|[Tt][Rr][Uu][Ee]") | rename eai:acl.app as app, title as csearch_name, action.correlationsearch.label as
csearch_label, action.notable.param.security_domain as security_domain | table csearch_name, csearch_label, app,
security_domain, description
```

또 다른 예로, 환경에서 사용 가능한 상관(correlation)검색과 해당 검색과 관련된 adaptive response 작업만 포함된 테이블도 만들 수 있음. 모든 상관(correlation)검색에 대한 adaptive response 작업을 보려면 | where disabled=0을 제거함.

```sql
| rest splunk_server=local count=0 /servicesNS/-/SplunkEnterpriseSecuritySuite/saved/searches | where
match('action.correlationsearch.enabled', "1|[Tt]|[Tt][Rr][Uu][Ee]") | where disabled=0 | eval
actions=split(actions, ",") | table title,actions
```

## Splunk Enterprise Security의 상관(correlation)검색 업그레이드

Splunk Enterprise Security 4.6.0 버전부터 correlationsearches.conf는 더 이상 상관 분석을 정의하는데 사용되지 않음. 그 대신 savedsearches.conf에서 action.correlationsearch.enabled=1 매개변수를 사용하여 상관(correlation)검색을 고
유하게 식별. correlationsearches.conf 파일은 지원이 중단.

### 업그레이드 시에 Splunk Enterprise Security가 적용하는 변경 사항

Splunk Enterprise Security 4.6.0으로 업그레이드할 때 Splunk Enterprise Security는 환경에 있는 모든 상관(correlation)
검색을 confcheck_es_correlationmigration.py 스크립트를 사용하여 correlationsearches.conf에서 savedsearches.conf로 마이그레이션함. 업그레이드 후에 마이그레이션을 완료하는 데는 최대 5분이 소요될 수 있음. 검색 헤드 클러스터에
서 캡틴이 마이그레이션을 수행함.

Splunk Enterprise Security는 업그레이드 중에 주요 이벤트를 중단 없이 계속 만듬. 이렇게 변경해도 Threat -
Correlation Searches - Lookup Gen 저장된 검색에서 인시던트 검토에 의해 사용되는 correlationsearches KV 스토어 컬렉션을 채우기 위해 correlationsearches.conf 및 savedsearches.conf를 계속 사용하므로 주요 이벤트가 인시던트 검토에 표시되지 않거나 표시가 지연되지 않음.

### 업그레이드 후에 변경해야 하는 사항

Splunk Enterprise Security 4.6.0 이상으로 업그레이드한 후에 추가로 변경해야 하는 사항이 있음.

- 검색이 성공적으로 마이그레이션되지 않았음을 나타내는 검색 정의에 대해서는 correlationsearches.conf를 확인. 마이그레이션된 검색은 savedsearches.conf에만 존재함. 검색이 성공적으로 마이그레이션되지 않은 경우 아래의 매개변수 정의를 사용하여 correlationsearches.conf 항목을 savedsearches.conf로 수동으로 마이그레이션함.
- correlationsearches REST endpoint를 호출하는 검색을 업데이트함.
  - 예를 들어 환경에 있는 상관(correlation)검색의 리스트를 표시하는 검색은 다음과 같이 변경.

    ```sql
    | rest splunk_server=local /services/alerts/correlationsearches | rename eai:acl.app as app, title as
    csearch_name | table app security_domain csearch_name description
    ~
    | rest splunk_server=local count=0 /services/saved/searches | where
    match('action.correlationsearch.enabled', "1|[Tt]|[Tt][Rr][Uu][Ee]") | rename eai:acl.app as app, title
    as csearch_name, action.correlationsearch.label as csearch_label, action.notable.param.security_domain as
    security_domain | table csearch_name, csearch_label, app, security_domain, description
    ```

  - 업데이트되는 검색의 더 많은 예는 Splunk Enterprise Security의 상관(correlation)검색 나열을 참조.

correlationsearches KV 스토어 컬렉션을 참조하는 사용자 지정 검색 매크로는 계속 전과 같이 작동하지만, 그래도 업그레이드하는 방안을 고려.

### correlationsearches.conf savedsearches.conf로 매개변수 변환

모든 correlationsearches.conf 매개변수는 이제 savedsearches.conf에 있고, correlationsearches.conf 파일은 지원이 중단. 상관(correlation)검색 정의를 수동으로 마이그레이션하려는 경우를 제외하고 직접 업데이트하지 않음.

#### 상관(correlation)검색의 식별 매개변수

새로운 매개변수가 저장된 검색이 상관(correlation)검색인지 여부와 상관(correlation)검색의 이름을 식별.

<table>
    <tr><td>correlationsearches.conf 매개변수(4.6.0 전 버전)</td><td>savedsearches.conf 매개변수(4.6.0 이후)</td><td>참고</td></tr>
    <tr><td>해당 없음</td><td>action.correlationsearch=0</td><td>내부 매개변수이며, 무시할 수 있음.
    <tr><td>검색에 대한 스탠자가 있음</td><td>action.correlationsearch.enabled=1</td><td>
    이 매개변수는 저장된 검색을 상관(correlation)검색으로 식별.</td></tr>
    <tr><td>rule_name</td><td>action.correlationsearch.label</td><td>
    이 매개변수는 상관(correlation)검색의 이름을 제공.</td></tr>
    <tr><td>description</td><td>description</td><td>
    이 매개변수는 상관(correlation)검색에 대한 설명을 제공.</td></tr>
</table>

#### 상관(correlation)검색의 주요 이벤트 매개변수

action.notable 매개변수는 상관(correlation)검색과 연결된 주요 이벤트를 식별. 주요 이벤트와 관련된 추가적인 세부 정보를 설명하는 매개변수는 이제 savedsearches.conf 파일에 있음.

<table>
    <tr><td>correlationsearches.conf 매개변수(4.6.0 전 버전)</td><td>
savedsearches.conf 매개변수(4.6.0 이후)</td></tr>
<tr><td>security_domain</td><td>action.notable.param.security_domain</td></tr>
<tr><td>severity</td><td>action.notable.param.severity</td></tr>
<tr><td>rule_title</td><td>action.notable.param.rule_title</td></tr>
<tr><td>rule_description</td><td>action.notable.param.rule_description</td></tr>
<tr><td>nes_fields</td><td>action.notable.param.nes_fields</td></tr>
<tr><td>drilldown_name</td><td>action.notable.param.drilldown_name</td></tr>
<tr><td>drilldown_search</td><td>action.notable.param.drilldown_search</td></tr>
<tr><td>default_status</td><td>action.notable.param.default_status</td></tr>
<tr><td>default_owner</td><td>action.notable.param.default_owner</td></tr>
</table>

#### 상관(correlation)검색 관련 검색 매개변수

상관(correlation)검색과 관련된 검색(극한 검색을 사용하는 상관(correlation)검색과 연결된 콘텐츠 생성 검색 등)은 이제JSON blob action.correlationsearch.related_searches 매개변수의 일부분임.

<table>
    <tr><td>correlationsearches.conf 매개변수(4.6.0 전 버전)</td><td>
savedsearches.conf 매개변수(4.6.0 이후)</td></tr>
<tr><td>related_search_name = Endpoint - Emails By Source - Context
Gen
related_search_name.0 = Endpoint - Emails By Destination
Count - Context Gen</td><td>
action.correlationsearch.related_searches = [\
"Endpoint - Emails By Source - Context
Gen",\
"Endpoint - Emails By Destination Count -
Context Gen"\
]</td></tr>
</table>

#### 이 버전과 이전 버전의 상관 (correlation) 검색 스탠자 예

4.6.0부터 상관(correlation)검색에 대한 savedsearches.conf 스탠자는 다음과 같음.

```properties
[Access - Concurrent App Accesses - Rule]
action.correlationsearch = 0
action.correlationsearch.enabled = 1
action.correlationsearch.label = Concurrent Login Attempts Detected
action.email.sendresults = 0
action.notable = 0
action.notable.param.security_domain = access
action.notable.param.severity = medium
action.notable.param.rule_title = Concurrent Access Event Detected For $user$
action.notable.param.rule_description = Concurrent access attempts to $app1$ by $user$ from two different sources(
$src1$, $src2$ ) have been detected.
action.notable.param.nes_fields = user
action.notable.param.drilldown_name = View access attemps by $user$
action.notable.param.drilldown_search = | datamodel Authentication Authentication search | search
Authentication.user="$user$"
action.risk = 1
action.risk.param._risk_object = user
action.risk.param._risk_object_type = user
action.risk.param._risk_score = 20
alert.suppress = 1
alert.suppress.fields = user
alert.suppress.period = 86300s
alert.track = false
cron_schedule = 10 * * * *
description = Alerts on concurrent access attempts to an app from different hosts. These are good indicators of
shared passwords and potential misuse.
disabled = True
dispatch.earliest_time = -70m@m
dispatch.latest_time = -5m@m
enableSched = 1
is_visible = false
request.ui_dispatch_app = SplunkEnterpriseSecuritySuite
search = | tstats `summariesonly` count from datamodel=Authentication.Authentication by
_time,Authentication.app,Authentication.src,Authentication.user span=1s | `drop_dm_object_name("Authentication")` |
eventstats dc(src) as src_count by app,user | search src_count>1 | sort 0 + _time | streamstats current=t window=2
earliest(_time) as previous_time,earliest(src) as previous_src by app,user | where (src!=previous_src) | eval
time_diff=abs(_time-previous_time) | where time_diff<300
```

이전 Splunk Enterprise Security 버전에서 동일한 상관(correlation)검색의 savedsearches.conf 및 correlationsearches.conf 정의는 다음과 같음. savedsearches.conf

```properties
[Access - Concurrent App Accesses - Rule]
action.email.sendresults = 0
action.risk = 1
action.risk.param._risk_object = user
action.risk.param._risk_object_type = user
action.risk.param._risk_score = 20
alert.suppress = 1
alert.suppress.fields = user
alert.suppress.period = 86300s
alert.track = false
cron_schedule = 10 * * * *
disabled = True
dispatch.earliest_time = -70m@m
dispatch.latest_time = -5m@m
enableSched = 1
is_visible = false
request.ui_dispatch_app = SplunkEnterpriseSecuritySuite
search = | tstats `summariesonly` count from datamodel=Authentication.Authentication by
_time,Authentication.app,Authentication.src,Authentication.user span=1s | `drop_dm_object_name("Authentication")` |
eventstats dc(src) as src_count by app,user | search src_count>1 | sort 0 + _time | streamstats current=t window=2
earliest(_time) as previous_time,earliest(src) as previous_src by app,user | where (src!=previous_src) | eval
time_diff=abs(_time-previous_time) | where time_diff<300
```

correlationsearches.conf

```properties
[Access - Concurrent App Accesses - Rule]
security_domain = access
severity = medium
rule_name = Concurrent Login Attempts Detected
description = Alerts on concurrent access attempts to an app from different hosts. These are good indicators
of shared passwords and potential misuse.
rule_title = Concurrent Access Event Detected For $user$
rule_description = Concurrent access attempts to $app1$ by $user$ from two different sources( $src1$, $src2$ )
have been detected.
nes_fields = user
drilldown_name = View access attemps by $user$
drilldown_search = | datamodel Authentication Authentication search | search Authentication.user="$user$"
default_owner =
default_status =
```

## Splunk Enterprise Security에서 adaptive response 작업 설정

Adaptive response 작업을 통해 상관(correlation)검색의 결과나 주요 이벤트의 세부 정보에 대응하여 정보를 수집하거나 기타 작업을 수행할 수 있음. Splunk Enterprise Security에는 몇 가지 adaptive response 작업이 포함되어 있음. 포함된 adaptive response 작업을 참조.

Adaptive response 작업과 경고 작업을 상관(correlation)검색에 추가하거나, 인시던트 검토 대시보드의 주요 이벤트에서 adaptive response 작업을 실행할 수 있음. 조사를 시작하기 전에 정보를 수집하여 분류 시에 adaptive response 작업을 상관(correlation)검색에 추가해서 시간을 절약할 수 있음. 분류 시에 인시던트 검토 대시보드에서 adaptive response 작업을 실행하여 조치를 취함.

### 새 adaptive response 작업 추가

새 adaptive response 작업을 추가하려면 adaptive response 작업이 있는 추가 기능을 설치하거나 adaptive response 작업을 직접 만들 수 있음. Adaptive response 작업 만들기에 대한 내용은 Splunk 개발자 포털에서 adaptive response 작업 만들기를 참조. 설치 및 업그레이드 매뉴얼에서 Splunk Enterprise Security에 포함된 배포 추가 기능을 참조.

### Adaptive Response 작업 감사(audit)

Adaptive Response 작업 센터에서 모든 adaptive response 작업에 대한 감사(audit)를 실시할 수 있음.

### adaptive response 작업 권한 설정

경고 작업 관리자에서 adaptive response 작업에 대한 권한을 조정하여 특정 adaptive response 작업을 특정 역할로 제한할 수 있음. 경고 작업 관리자에 대한 내용은 Splunk 플랫폼 매뉴얼에서 확인할 수 있음.

- Splunk Enterprise의 경우, Splunk Enterprise 경고 매뉴얼에서 경고 작업 관리자 사용을 참조.
- Splunk Cloud의 경우, Splunk Cloud 경고 매뉴얼에서 경고 작업 관리자 사용을 참조.

자격 증명 관리자에 자격 증명이 저장되어 있는 인시던트 검토 대시보드에서 adaptive response 작업을 실행하기 위해서는 해당 기능을 보유하고 있어야 함.

- Splunk 플랫폼 6.5.0 버전 이상의 경우 list_storage_passwords
- 이전 Splunk 플랫폼 버전의 경우 admin_all_objects

### 상관(correlation)검색에 adaptive response 작업 추가

1. Splunk Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 클릭.
2. 기존 상관(correlation)검색을 클릭하거나 새로 만들기 > 상관(correlation)검색을 클릭.
3. 새 대응 작업 추가를 클릭하고 추가할 대응 작업을 선택.
4. 작업에 대한 필드를 작성함. 원할 경우 대응 작업을 하나 더 추가.
5. 저장을 클릭하여 상관(correlation)검색에 적용한 변경 사항을 모두 저장함.

Splunk Enterprise Security에 포함된 adaptive response 작업을 각각 설정하는 방법에 대한 설명은 Splunk Enterprise Security에서 상관(correlation)검색에 대한 adaptive response 작업 설정을 참조. 사용자 지정 adaptive response 작업을 설정하는 방법에 대한 설명은 adaptive response 작업을 제공한 앱 또는 추가 기능에 대한 매뉴얼을 참조.

### Adaptive response 작업을 선택할 수 없는 문제 해결

상관(correlation)검색 편집기나 인시던트 검토에서 adaptive response 작업을 선택할 수 없는 경우, 몇 가지 이유 때문일 수 있음.

- 역할에 adaptive response 작업을 보고 사용할 권한이 없을 수 있음. 경고 매뉴얼에서 경고 작업 관리자 사용을 참조.
- 경고 작업 관리자에서 Splunk 플랫폼에 adaptive response 작업이 있는지 확인. 경고 매뉴얼에서 경고 작업 관리자 사용을 참조.
- 추가 기능의 adaptive response 작업이 Splunk Enterprise Security에 표시되지 않지만 경고 작업 관리자에는 표시되는 경우, Splunk Enterprise Security에서 추가 기능을 가져오고 있는지 확인. 설치 및 업그레이드 매뉴얼에서 사용자 지정 앱 및 추가 기능을 Splunk Enterprise Security로 가져오기를 참조.
- Adaptive response 작업을 상관(correlation)검색 편집기에서 선택할 수 있지만 인시던트 검토에서는 선택할 수 없는 경우, adaptive response 작업이 일반적인 경고 작업이거나 대응 작업이 임시 호출을 지원하지 않기 때문일 수 있음. Splunk 개발자 포털에서 작업이 임시 호출을 지원하는지 여부 확인을 참조.

## Splunk Enterprise Security에서 상관(correlation)검색에 대한 adaptive response 작업 설정

Splunk Enterprise Security 관리자는 상관(correlation)검색이 트리거하는 adaptive response 작업을 설정할 수 있음.
애널리스트는 일부 adaptive response 작업을 인시던트 검토에서 임시로 실행할 수 있음. Splunk Enterprise Security 사용에서 Splunk Enterprise Security에 포함된 adaptive response 작업을 참조.

Splunk Enterprise Security에는 몇 가지 adaptive response 작업이 포함되어 있으며, Splunkbase에서 제공되는 추가 기능에서 추가 작업을 얻을 수 있음.

### 포함된 Adaptive Response 작업

Splunk Enterprise Security에는 몇 가지 adaptive response 작업이 포함되어 있음.

- 주요 이벤트를 만듬.
- 위험 수정자를 사용하여 위험 점수를 수정.
- 이메일을 보냄.
- 스크립트를 실행.
- Splunk Stream을 사용하여 스트림 캡처를 시작.
- 호스트를 핑(Ping)함.
- Nbtstat를 실행.
- Nslookup을 실행.
- 위협 인텔리전스를 추가.
- Splunk Web 메시지를 만듬.

### 주요 이벤트 만들기

상관(correlation)검색 조건이 충족되면 주요 이벤트를 만듬.

1. Splunk Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 클릭.
2. 기존 상관(correlation)검색을 클릭하거나 새로 만들기 > 상관(correlation)검색을 클릭.
3. 새 Response Action 추가를 클릭하고 주요를 선택하여 주요 이벤트를 추가.
4. 인시던트 검토 대시보드에서 주요 이벤트의 제목을 입력. 일치하는 이벤트의 필드에서 변수 대체를 지원.
5. 주요 이벤트에 대한 설명을 입력. 일치하는 이벤트의 필드에서 변수 대체를 지원.
6. 주요 이벤트의 보안 도메인을 드롭다운 리스트에서 선택.
7. 주요 이벤트의 심각도를 드롭다운 리스트에서 선택. 심각도는 주요 이벤트의 긴급도를 계산하는데 사용.
8. (선택 사항) 주요 이벤트 기본 소유자를 시스템 기본값인 배정되지 않음에서 변경.
9. (선택 사항) 주요 이벤트 기본 상태를 시스템 기본값인 새 이벤트에서 변경.
10. 주요 이벤트의 기여 이벤트 링크에 대한 드릴다운 이름을 입력.
11. 주요 이벤트의 기여 이벤트 링크에 대한 드릴다운 검색을 입력.
12. 드릴다운 시작 오프셋 필드에 트리거링 이벤트 시간 전에 주요 이벤트의 기여 이벤트 링크 관련 이벤트를 찾아볼 시간을 입력.
    예를 들어 트리거링 이벤트 2시간 전의 기여 이벤트를 찾아보려면 2h를 입력.
13. 드릴다운 종료 오프셋 필드에 트리거링 이벤트 시간 후에 주요 이벤트의 기여 이벤트 링크 관련 이벤트를 찾아볼 시간을 입력.
    예를 들어 트리거링 이벤트 1시간 후의 기여 이벤트를 찾아보려면 1h를 입력.
14. (선택 사항) 주요 이벤트에 적용되는 조사 프로파일을 추가.
    예를 들어, "멀웨어"의 이용 사례를 멀웨어 관련 주요 이벤트에 일치시키는 조사 프로파일을 추가.
15. (선택 사항) 자산이 포함된 필드를 자산 추출에 추가하여 필드 값을 추출하고 주요 이벤트가 조사에 추가될 때 필드 값을 조사 워크벤치에 아티팩트로 추가.
16. (선택 사항) ID가 포함된 필드를 ID 추출에 추가하여 필드 값을 추출하고 주요 이벤트가 조사에 추가될 때 필드 값을 조사 워크벤치에 아티팩트로 추가.
17. 주요 이벤트를 분류한 후에 애널리스트가 수행할 다음 단계를 입력. 텍스트를 입력하거나, 다음 단계 텍스트의 대응 작업을 참조하려면 Adaptive Response 작업 삽입을 클릭. 다음 단계 필드에는 일반 텍스트와 대응 작업 링크만 입력할 수 있음. 특정한 순서로 수행해야 하는 대응 작업을 권장하려면 다음 단계를 사용.
    예를 들어 호스트를 핑(ping)하여 호스트가 네트워크에서 활성인지 확인. 호스트가 활성인 경우 위험 점수를 100점 올리고, 그렇지 않으면 위험 점수를 50점 올림.
18. 권장 작업을 선택하여 다음 단계를 보완함. 모든 adaptive response 작업 리스트에서 이 주요 이벤트의 분류 또는 조사 단계로 권장할 작업의 이름을 클릭하여 애널리스트가 이 주요 이벤트에 대해 수행할 수 있는 권장 작업 리스트에 추가. 권장 작업을 원하는 수만큼 추가할 수 있음. 권장 작업은 특정한 순서로 수행하지 않아도 되는 대응 작업을 권장하는데 사용.
    예를 들어 호스트의 위험 점수를 높이고 도메인 이름에 대한 nslookup을 수행하는데 사용.

### 위험 수정자를 사용하여 위험 점수 수정

위험 분석 adaptive response 작업을 통해 상관(correlation)검색의 결과에 따라, 또는 주요 이벤트 세부 사항에 대응하여 위험 점수를 수정할 수 있음. 위험 adaptive response 작업은 위험 수정자 이벤트를 만듬. Enterprise Security의위험 분석 대시보드에서 위험 수정자 이벤트를 확인할 수 있음.

1. 새 대응 작업 추가를 클릭하고 위험 분석을 선택.
2. 위험 개체에 배정할 점수를 입력.
3. 위험 점수를 적용할 검색의 필드를 위험 개체 필드에 입력.
    예를 들어 소스 필드를 지정하려면 "src"를 입력.
4. 위험 점수를 적용할 위험 개체 유형을 선택.

위험 점수를 수정하는 다른 방법은 Splunk Enterprise Security 사용의 개체에 위험 배정을 참조.

### 이메일 보내기

상관(correlation)검색 일치의 결과로 이메일을 보냄.

전제 조건

이 대응 작업을 설정하기 전에 Splunk 플랫폼에 메일 서버가 설정되어 있는지 확인.

- Splunk Enterprise의 경우, Splunk Enterprise 경고 매뉴얼에서 이메일 알림 설정 구성을 참조.
- Splunk Cloud의 경우, Splunk Cloud 경고 매뉴얼에서 이메일 알림 설정 구성을 참조.

절차

1. 새 대응 작업 추가를 클릭하고 이메일 보내기를 선택.
2. 수신 필드에 이메일을 보낼 이메일 주소의 쉼표로 구분된 리스트를 입력.
3. (선택 사항) 이메일 우선 순위를 변경. 기본값은 최저임.
4. 이메일 제목을 입력. 이메일의 기본 제목은 "Splunk 경고: $name$"임. 여기서 $name$은 상관(correlation)검색의 검색 이름임.
5. 이메일 본문으로 포함시킬 메시지를 입력. 기본값은 "예약된 보고서 '$name$'이(가) 실행되었습니다.".
6. 이메일 메시지에 포함시킬 정보의 체크박스를 선택.
7. 보낼 이메일 메시지의 형식을 일반 텍스트 또는 HTML과 일반 텍스트 중에서 선택.

### 스크립트 실행

$SPLUNK_HOME/bin/scripts에 저장된 스크립트를 실행.

1. 새 대응 작업 추가를 클릭하고 스크립트 실행을 선택.
2. 스크립트 파일 이름을 입력.

스크립트 기반 경고에 대한 자세한 내용은 Splunk 플랫폼 매뉴얼에서 확인할 수 있음.
Splunk Enterprise의 경우, Splunk Enterprise 경고 매뉴얼에서 스크립트 기반 경고 설정을 참조.
Splunk Cloud의 경우, Splunk Cloud 경고 매뉴얼에서 스크립트 기반 경고 설정을 참조.

### Splunk Stream을 사용하여 스트림 캡처 시작

스트림 캡처를 시작하여 선택된 프로토콜의 IP 주소에서 선택된 기간 동안 패킷을 캡처할 수 있음. 캡처 세션 결과는 프로토콜 인텔리전스 대시보드에서 확인할 수 있음.
Splunk 스트림을 Splunk Enterprise Security과 통합하지 않는 한, 스트림 캡처가 작동하지 않음. Splunk Enterprise Security와 Splunk 스트림 통합을 참조.

1. 상관(correlation)검색 일치에 대응하여 패킷 캡처를 시작하려면 새 대응 작업 추가를 클릭하고 스트림 캡처를 선택.
2. 상관(correlation)검색 일치에 대응하여 만든 스트림에 대해 설명하는 설명을 입력.
3. 스트림 캡처 유형을 정의하는 범주를 입력. Splunk 스트림에서 스트림을 범주별로 볼 수 있음.
4. 스트림 캡처 IP 주소를 검색할 쉼표로 구분된 이벤트 필드를 입력. null이 아닌 첫 번째 필드가 캡처에 사용.
5. 캡처할 프로토콜의 쉼표로 구분된 리스트를 입력.
6. 패킷 캡처 길이를 정의하는 캡처 기간을 선택.
7. 스트림 캡처 제한을 입력하여 상관(correlation)검색에 의해 시작되는 스트림 캡처 수를 제한함.

### 호스트 핑(Ping)

호스트를 핑(ping)하여 호스트가 네트워크에서 아직 활성인지 확인.

1. 새 대응 작업 추가를 클릭하고 핑을 선택.
2. 핑(ping)할 호스트가 있는 이벤트 필드를 호스트 필드에 입력.
3. 핑(ping)에서 반환되는 최대 결과 수를 입력. 기본값은 1임.

### Nbtstat 실행

Nbtstat을 실행하여 호스트와 호스트가 실행하는 서비스에 대해 자세히 알아봄.

1. 새 대응 작업 추가를 클릭하고 Nbtstat을 선택.
2. Nbtstat을 실행할 호스트가 있는 이벤트 필드를 호스트 필드에 입력.
3. Nbtstat에서 반환되는 최대 결과 수를 입력. 기본값은 1임.

### Nslookup 실행

Nslookup을 실행하여 IP 주소의 도메인 이름이나 도메인 이름의 IP 주소를 조회.

1. 새 대응 작업 추가를 클릭하고 Nslookup을 선택.
2. Nslookup을 실행할 호스트가 있는 이벤트 필드를 호스트 필드에 입력.
3. Nslookup에서 반환되는 최대 결과 수를 입력. 기본값은 1임.

### 위협 인텔리전스 추가

위협 아티팩트를 위협 컬렉션에 만듬.

1. 새 대응 작업 추가를 클릭하고 위협 인텔리전스 추가를 선택.
2. 이 아티팩트를 귀속시킬 위협 그룹을 선택.
3. 위협 아티팩트를 삽입할 위협 컬렉션을 선택.
4. 위협 아티팩트에 삽입할 값이 있는 검색 필드를 입력.
5. 위협 아티팩트에 대한 설명을 입력.
6. 위협 리스트와 연결된 가중치를 입력. 기본값은 1임.
7. 최대 결과 수를 입력하여 위협 아티팩트로 처리할 결과의 수를 지정함. 각 고유 검색 필드 값이 결과 수에 포함. 기본값은 100임.