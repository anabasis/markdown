# 인시던트 검토 및 조사

## Splunk Enterprise Security에서 인시던트 검토 관리

Splunk Enterprise Security는 데이터에서 패턴을 감지하고 상관(correlation)검색을 사용하여 자동으로 이벤트에 보안관련 인시던트가 있는지 검토
상관(correlation)검색에서 수상한 패턴이 감지되면 notable event라는 경고가 생성
인시던트 검토 대시보드는 모든 notable event를 표시하고 잠재적인 심각도에 따라 범주로 나누므로 애널리스트는 문제를 빨리 분류 및 배정하고 추적

- 애널리스트가 인시던트 검토 대시보드를 사용하는 방법에 대한 내용은 Splunk Enterprise Security 사용에서 [인시던트 검토 개요](http://docs.splunk.com/Documentation/ES/5.0.0/User/IncidentReviewdashboard)를 참조
- 인시던트 검토 대시보드에서 애널리스트 작업을 감사(audit)하고 검토하려면 Splunk Enterprise Security 사용에서 [인시던트 검토 감사(audit)](http://docs.splunk.com/Documentation/ES/5.0.0/User/Audit#Incident_Review_Audit)를 참조
- 인시던트 검토 대시보드 디스플레이를 커스터마이징하고 애널리스트 기능 및 권한도 수정하려면 Splunk Enterprise Security에서 [인시던트 검토 커스터마이징](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/CustomizeIR)을 참조
- notable event를 수동으로 만들려면 Splunk Enterprise Security에서 [수동으로 notable event 만들기](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Createnotablesmanually)를 참조
- notable event 설정을 커스터마이징하려면 Splunk Enterprise Security에서 [notable event 설정 커스터마이징](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizenotables)을 참조
- notable event 프레임워크에 의해 notable event가 채워지고 관리되는 방법에 대한 자세한 내용은 Splunk 개발자 포털에서 Splunk Enterprise Security의 [notable event 프레임워크](http://dev.splunk.com/view/enterprise-security/SP-CAAAFA9)를 참조

### 인시던트 검토에 risk score가 표시되는 방법

모든 Asset 및 Identity에 대한 risk score가 인시던트 검토에 표시되지는 않음
risk score가 있고 위험 개체 유형이 "system" 또는 "user"인 Asset 및 Identity(위험 개체)만 인시던트 검토에 표시
risk score는 **orig_host, dvc, src, dest, src_user 및 user** 필드에만 표시
Asset 및 Identity의 risk score는 위험 분석 대시보드에 표시되는 점수와 일치하지 않을 수 있음
risk score는 정확한 사용자 이름으로 한정되는 점수가 아니라 Asset 및 Identity에 대한 누적 점수

- 예를 들어 어떤 사람에게 risk score가 40인 "buttercup"이라는 사용자 이름이 있고 risk score가 60인 "buttercup@splunk.com"이라는 이메일 주소가 있는데 Identity Lookup에서 "buttercup"과 "buttercup@splunk.com"이 동일 한 사람의 사용자 이름과 이메일 주소로 확인될 경우, 인시던트 검토에 "buttercup" 및 "buttercup@splunk.com" 계정의 risk score가 100으로 표시
- 또 다른 예로, IP 10.11.36.1의 risk score가 80이고 IP 10.11.36.19의 risk score가 30인데 자산 Lookup에서 "10.11.36.1 - 10.11.36.19"가 동일한 자산에 속한 IP 범위로 확인될 경우, 인시던트 검토에 IP 주소 "10.11.36.1" 및 "10.11.36.19"의 risk score가 모두 110으로 표시

위험도 점수는 **"Threat - Risk Correlation - Lookup Gen"** Lookup 생성 검색을 사용하여 Incident Review에 대해 계산
검색은 30 분마다 실행되고 **risk_correlation_lookup** Lookup 파일을 업데이트
인시던트 검토에서 risk score가 더 자주 업데이트되도록 하려면 saved search의 **cron_schedule**을 업데이트

### 분류되지 않은 notable event에 대해 애널리스트에게 알림

notable event가 분류되지 않은 경우 상관(correlation)검색을 사용하여 애널리스트에게 알릴 수 있음

1. **설정 > 콘텐츠 관리**를 선택
2. 필터를 사용하여 분류되지 않은 notable event 상관(correlation)검색
3. notable event 소유자 또는 상태 필드를 원하는 대로 변경하여 검색을 수정
4. 원하는 경고 작업을 설정
5. 변경 사항을 저장
6. 분류되지 않은 notable event 상관(correlation)검색을 활성화

## Splunk Enterprise Security에서 인시던트 검토 커스터마이징

Splunk Enterprise Security 관리자는 애널리스트가 인시던트 검토 대시보드에서 notable event를 보고 notable event와 상호 작용하는 방법을 커스터마이징할 수 있음

### 애널리스트 기능 및 권한 수정

**인시던트 검토 설정** 페이지에서 애널리스트가 notable event의 계산된 긴급도를 재정의할 수 있는지 설정
애널리스트가 notable event를 업데이트할 때 주석을 추가하도록 요구할 것인지 선택할 수 있음

1. **설정 > 인시던트 관리 > 인시던트 검토 설정**을 선택하여 인시던트 검토 설정
2. 긴급도 재정의 허용 체크박스를 사용하여 애널리스트가 notable event의 계산된 긴급도를 재정의하는 것을 허용하거나 차단(애널리스트는 기본적으로 긴급도를 재정의할 수 있음)
3. 주석 아래의 필수 체크박스를 선택하여 애널리스트가 notable event를 업데이트할 때 주석을 추가할 것을 요구
4. 애널리스트가 주석을 추가하도록 요구할 경우, 필수 주석의 최소 문자 수를 입력(기본 문자 수는 20자)

### 애널리스트 권장 용량 설정

일반 설정 페이지에서 보안 애널리스트 1인당 배정할 notable event의 권장 최대 수를 설정

1. **설정 > 일반 > 일반 설정**을 선택하여 일반 설정을 표시
2. **인시던트 검토 애널리스트 용량** 설정을 사용하여 애널리스트에게 배정할 notable event 수를 입력.(Incident Review Analyst Capacity 기본값은 12)

> Tip
> 이 값은 감사(audit) 용도로 사용되며, 애널리스트에게 기본 수보다 많은 notable event가 배정되는 것을 방지하지 않음

### 인시던트 검토 컬럼 변경

인시턴트 검토 대시보드에 표시되는 컬럼을 변경할 수 있음

1. **설정 > 인시던트 관리 > 인시던트 검토 설정**을 선택하여 인시던트 검토 설정
2. **인시던트 검토 - 테이블 속성**에서 기존 컬럼을 검토
3. 작업 컬럼을 사용하여 사용 가능한 컬럼을 편집 또는 제거하거나 컬럼 순서를 변경
4. 아래에 삽입을 선택하거나 더 보기...를 선택한 다음 위에 삽입을 선택하여 사용자 지정 컬럼을 추가

### 애널리스트가 인시던트 검토에서 notable event를 성공적으로 편집할 수 없는 경우 문제 해결

애널리스트가 인시던트 검토에서 notable event를 성공적으로 편집할 수 없는 경우 몇 가지 이유 때문일 수 있음

- 애널리스트에게 상태를 전환할 수 있는 권한이 없음. [notable event 상태 관리](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizenotables#Manage_notable_event_statuses)를 참조
- 애널리스트가 표시되지만 버킷에서 제한된 수의 이벤트만 가져올 수 있기 때문에 성공적으로 편집할 수 없는 notable event를 편집하려고 시도할 수도 있음

상관(correlation)검색에서 짧은 기간에 많은 수(예: 5분 미만에 1000개)의 notable event를 만드는 경우 notable 인덱스에서 표시할 notable event를 가져오려고 시도할 때 인시던트 검토 대시보드가 `max_events_per_bucket` 한도에 도달

```properties
[limits.conf]
max_events_per_bucket = <integer>
* For searches with “status_buckets>0”, this setting limits the number of
  events retrieved for each timeline bucket.
* Default: 1000 in code.
```

이러한 이유로 notable event를 편집할 수 없는 경우 애널리스트는 더 짧은 기간에 인시던트 검토의 notable event를 검토
예를 들어, 인시던트 검토 대시보드에서 1000개 미만의 이벤트를 검토
1000은 `max_events_per_bucket`의 기본값이므로 이벤트를 1000개 미만 생성하는 검색의 경우 이 오류가 발생하지 않음.

어떤 경우에도 이 문제가 발생하지 않게 하려면 버킷에서 반환될 수 있는 최대 이벤트 수를 수정
하지만, 이 설정을 수정할 경우 Splunk 소프트웨어 베포의 성능에 부정적인 영향을 미칠 수 있음

> Tip
> Splunk Enterprise Security를 Splunk Cloud에서 실행하는 경우, 지원 요청을 접수하여 이 설정에 관한 도움

1. `limits.conf`를 열어서 편집. Splunk Enterprise 관리자 매뉴얼에서 설정 파일 편집 방법을 참조
2. `max_events_per_bucket`을 1,000보다 큰 숫자로 설정
3. 저장

`max_events_per_bucket` 설정에 대한 자세한 내용은 `limits.conf`를 참조

### 인시던트 검토의 필터링된 뷰에 탐색링크 추가

ES 애널리스트의 워크로드 처리를 돕기 위해, 필터가 적용된 인시던트 검토 버전을 로드하는 링크를 앱 탐색에 추가
[인시던트 검토의 필터링된 뷰에 링크 추가](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizemenubar#Add_a_link_to_a_filtered_view_of_Incident_Review)를 참조

## Splunk Enterprise Security에서 notable event 수동으로 만들기

인덱싱된 이벤트를 사용하여 notable event를 수동으로 만들거나 처음부터 새로 만듬

> 참고: 기본적으로 관리자만 notable event를 수동으로 만들 수 있음.
> 다른 사용자에게 이 기능을 부여하는 방법은 설치 및 업그레이드 매뉴얼에서 [사용자 및 역할 설정](http://docs.splunk.com/Documentation/ES/5.0.0/Install/ConfigureUsersRoles)을 참조

### 기존 이벤트를 토대로 notable event 만들기

**이벤트 작업(Actions)** 메뉴를 사용해 인덱싱된 이벤트를 토대로 notable event를 만들 수 있음
인시던트 검토 대시보드에 있는 notable event를 토대로 notable event를 만들지 않음.

1. 이벤트에서 이벤트 세부 정보를 보고 **이벤트 작업(Actions)**을 클릭
2. notable event 만들기를 선택
3. 이벤트의 제목을 입력
4. (선택 사항) 보안 도메인을 선택
5. (선택 사항) 긴급도 레벨을 선택
6. (선택 사항) 소유자를 선택
7. (선택 사항) 상태를 선택
8. notable event를 만든 이유와 무엇을 조사해야 하는지 설명하는 이벤트에 대한 설명을 입력
9. 새 notable event를 저장. 새 notable event가 있는 **인시던트 검토** 대시보드에 표시

> 참고: 이 방법으로 만든 notable event에는 소유자와 상태 같은 추척 필드가 포함되지만, notable event가 상관(correlation) 검색 경고 작업을 통해 생성되는 경우에 만들어지는 고유 필드 또는 링크는 포함되지 않음

### notable event 처음부터 새로 만들기

관찰 결과나 Splunk 외부 보안 시스템에서 발견한 사항 등을 기반으로 notable event를 만듬.

1. **설정 > 인시던트 관리 > 새로운 notable event**를 선택
2. 이벤트의 제목을 입력
3. (선택 사항) 보안 도메인을 선택
4. (선택 사항) 긴급도 레벨을 선택
5. (선택 사항) 소유자를 선택
6. (선택 사항) 상태를 선택
7. notable event를 만든 이유와 무엇을 조사해야 하는지 설명하는 이벤트에 대한 설명을 입력
8. 새 notable event를 저장. 새 notable event가 있는 인시던트 검토 대시보드가 표시됨

## Splunk Enterprise Security에서 notable event 설정 사용자 지정

Splunk Enterprise Security 관리자는 notable event 설정을 변경

[notable event 필드 변경](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizenotables#Change_notable_event_fields)
[notable event 상태 관리](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizenotables#Manage_notable_event_statuses)
[notable event 제거 작성 및 관리](http://docs.splunk.com/Documentation/ES/5.0.0/Admin/Customizenotables#Create_and_manage_notable_event_suppressions)

### notable event 필드 변경

인시던트 검토 설정 대시보드에서 인시던트 검토 대시보드에 notable event에 대해 표시되는 필드를 변경
예를 들어 notable event 세부 정보의 필드 레이블을 변경하거나, 필드를 제거하거나, 필드를 notable event 세부 정보의 추가 필드 섹션에 추가
notable event 필드에 적용하는 변경 사항은 모든 notable event에 영향

1. Splunk Enterprise Security 메뉴 모음에서 **설정 > 인시던트 관리 > 인시던트 검토 설정**을 선택(**모든 설정 > 인시던트 설정**)
2. **인시던트 검토 - 이벤트 속성**을 검토
3. 편집을 클릭하여 인시던트 검토에 표시되는 필드나 특정 필드의 레이블을 변경
4. 제거를 클릭하여 인시던트 검토 대시보드의 notable event 세부 정보에서 필드를 제거
5. 저장을 클릭하여 변경 사항을 저장

### notable event 세부 정보에 필드 추가

필드가 상관(correlation)검색 결과에 있고 인시던트 검토에 필드를 표시할 수 있는 경우, notable event 세부 정보의 추가 필드에 필드가 표시
notable event 세부 정보에 필드를 추가하려면 상관(correlation)검색 결과에 필드가 포함되어 있는지 먼저 확인한 다음 인시던트 검토에 필드가 표시될 수 있는지 확인

1. 보고 싶은 필드가 상관(correlation)검색 결과에 포함되어 있는지 확인. 검색 페이지에서 상관(correlation)검색을 실행하여 출력 또는 검색 구문을 검토
    - 필드가 검색 결과에 있으면 4 단계로 진행
    - 필드가 검색 결과에 없으면 2 단계로 진행
2. 필드를 포함하도록 상관(correlation)검색을 수정
    - 가이드식 검색 편집기로 검색을 편집할 수 있는 경우, 별칭을 사용하여 필드를 집계 함수로 추가. `values`함수를 사용하여 특정 필드의 모든 가능한 값을 반환하거나, `latest` 함수를 사용하여 필드의 최근 값을 반환.
    - 검색을 수동으로 만든 경우, 필드를 추출하도록 검색을 수정. 검색을 수정할 때 상관 조건을 수정하지 않음
      - 검색에 통계 변환이 포함되지 않은 경우 `| fields + newfieldname`을 검색 끝에 추가. 여기서 `newfieldname`은 추가 세부 정보에 표시할 새 필드의 이름.
      - 검색에 통계 변환이 포함되어 있는 경우 통계 변환을 수행할 때 필드를 추출. 예를 들어 검색에 `| stats count by src | where count>5` 통계 검색이 포함된 경우 `src` 및 `count` 필드가 notable event 세부 정보에 표시. notable event 세부 정보에 `dest` 필드를 추가하려면, 검색을 다음과 같이 변경. `| stats values(dest) as dest,count by src`.
3. 상관(correlation)검색을 변경한 후 저장하기 전에 검색 페이지에서 변경 사항을 확인.
4. 필드를 추가 필드 리스트에 추가.
    1. Splunk Enterprise Security 메뉴 모음에서 **설정 > 인시던트 관리 > 인시던트 검토 설정**을 선택
    2. 새 항목 추가를 클릭하여 새 필드를 notable event 세부 정보의 추가 필드 섹션에 추가
    3. notable event 세부 정보에 있는 필드의 표시 이름으로 사용할 레이블을 입력
    4. notable event 세부 정보에 표시할 필드와 일치하는 필드를 입력
    5. 완료를 클릭
    6. 저장을 클릭

### notable event 상태 관리

애널리스트는 조사케이스 워크플로의 notable event에 상태를 배정. 상태는 조사케이스의 단계와 일치하고, 인시던트 검토 감사(audit) 대시보드에서 notable event 조사의 진행률을 검토하고 보고하는데 사용

사용 가능한 notable event 상태를 보려면 **설정 > 인시던트 관리 > 상태 설정**을 선택.

|레이블|설명|편집 가능|
|:--:|:--|:--:|
|Unassigned[배정되지 않음]|오류로 인해 notable event에 유효한 상태를 배정할 수 없을 때 Enterprise Security에 의해 사용됨|아니오|
|New(Default) [새 이벤트(기본값)]|notable event가 검토되지 않음|아니오|
|In Progress[진행중]|notable event 조사 또는 대응이 진행중|예|
|Pending[보류중]|notable event 종료가 일부 작업을 보류중|예|
|Resolved[해결됨]|notable event가 해결되었고 확인을 기다리는 중|예|
|Closed[종료됨]|notable event가 해결되고 확인|예|

상관(correlation)검색이 notable event를 만들 때 모든 이벤트에 기본적으로 **New** 이벤트 상태가 배정. notable event 상태를 커스터마이징하여 조직의 기존 워크플로와 일치시킬 수 있음.

### notable event 상태 편집

**notable event 상태 편집** 페이지에서 사용 가능한 notable event 상태 변경

1. Splunk Enterprise Security 도구 모음에서 **설정 > 인시던트 관리 > 상태 설정**을 선택
2. notable event 상태를 선택하여 **notable event 상태 편집** 페이지
3. (선택 사항) **레이블** 또는 **설명**을 변경

> Tip
> **Unassigned** 및 **New** 이벤트 상태는 notable event를 만들 때 사용되는 기본값이므로 편집할 수 없음

### notable event 상태 이력 관리

notable event는 사용자, 상태 및 주석과 연결
변경한 상태 이름은 상태 이름에만 적용되고 주요 인덱스의 notable event에 배정된 상태 ID에는 적용되지 않음
기본 notable event 상태의 이름을 변경하면 과거 및 미래 notable event의 이름이 모두 변경
예를 들어 이름을 "보류중"에서 "고객 대기 중"으로 바꾸면 상태가 "보류중"인 모든 notable event의 상태가 "고객 대기 중"으로 바뀜
주요 이벤트에 배정된 상태 ID는 똑같이 유지됨

### notable event 상태 전환

상태는 notable event를 조사하는 단계를 나타냄
상태 전환은 notable event 조사 경로를 정의
애널리스트가 조사가 진행됨에 따라 notable event의 상태를 변경
notable event의 상태를 변경하는 방법:

- 에널리스트는 상태를 바꿀 권한이 있는 역할의 구성원
- notable event 상태를 변경할 수 있는 기능은 기본적으로 **ess_analyst(보안분석가)** 및 **ess_admin(보안운영자)** 역할이 사용
- 후속 상태는 현재 상태에서 전환할 수 있는 상태. 기본적으로 모든 상태는 기타 상태로 전환할 수 있음. 예를 들어, 애널리스트는 **New** 이벤트 상태인 notable event의 상태를 다른 상태(예: **Closed**)로 변경할 수 있음

### notable event 상태 전환 제한

상태 워크플로를 정의하고 상태 애널리시트가 다른 상태로 전환할 수 있는 상태를 제한하여 notable event 조사 경로를 만들수 있음. 기본적으로 모든 상태의 notable event를 다른 상태로 변경

전제 조건

- **ess_admin** 역할이 있어야하며 역할에 **상태 편집**기능이 지정되어야 함. 사용자 역할 및 기능에 대한 자세한 내용은 설치 및 업그레이드 매뉴얼에서 [사용자 및 역할 설정](http://docs.splunk.com/Documentation/ES/5.1.0/Install/ConfigureUsersRoles)을 참조
- notable event 조사 상태 워크플로를 정의. 어떤 상태를 요구할 것인지 결정하고, 애널리스트가 워크플로를 완료하기 전에 특정한 상태 순서를 따라야 하는지 결정. 전체 워크플로를 우회할 수 있는 역할이 있는지 결정.

절차

1. Splunk Enterprise Security 도구 모음에서 **설정 > 인시던트 관리 > 상태 설정**을 선택.
2. notable event 상태를 선택하여 notable event 상태 편집 페이지
3. 상태 전환에서 **전환 후 상태** 필드를 수정. 이 필드는 notable event가 편집하려는 상태에 있는 경우 상태 애널리스트가 notable event를 전환할 수 있는 상태를 제어
    1. notable event를 선택된 상태로 전환할 수 있는 역할을 정의하려면 **권한** 필드를 선택하고 역할을 추가 또는 제거
    2. 선택된 상태로 이벤트 전환을 제거하려면 **모두 선택 취소**를 선택
4. 저장

### notable event 상태 전환 제한 예

이 예는 애널리스트를 위해 상태 전환 제한을 설정하는 과정. 새 이벤트에서 진행중 또는 보류중, 해결됨, 종료됨의 경로를 따라야 하도록 상태 전환을 제한.

1. Splunk Enterprise Security 도구 모음에서 **설정 > 인시던트 관리 > 상태 설정**을 선택.
2. **New** 이벤트 상태에서 전환을 제한. **New** 이벤트 상태를 선택하여 조사 상태 편집 페이지.
3. **상태 전환**에서 **Resolved[해결됨]** 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제.
4. **Closed[종료됨]** 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제.
5. 저장을 클릭하여 **New** 이벤트 상태의 변경 사항을 저장.
6. ess_analyst 역할이 새 이벤트 또는 종료됨으로 전환하지 못하도록 진행중 및 보류중 상태에서 전환을 제한함.
7. **In Progress[진행중]** 상태를 선택.
8. **상태 전환**에서 **New** 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. **Closed[종료됨]** 상태에 대해 반복.
9. **저장**을 클릭하여 **In Progress[진행중]** 상태의 변경 사항을 저장.
10. **Pending[보류중]** 상태에 대해 8단계와 9단계를 반복.
11. **Resolved[해결됨]** 상태를 제한. **조사** 탭을 클릭하고 **Resolved[해결됨]** 상태를 선택.
12. **상태 전환**에서 **New** 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. **In Process[진행중]** 및 **Pending[보류중]** 상태에 대해 반복.
13. **저장**을 클릭하여 **Resolved[해결됨]** 상태의 변경 사항을 저장.
14. **Closed[종료됨]** 상태에 대한 전환을 제한. **Closed[종료됨]** 상태를 선택.
15. **상태 전환**에서 **New** 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. **In Process[진행중]**, **Pending[보류중]** 및 **Resolved[해결됨]** 상태에 대해 반복.
16. **저장**을 클릭하여 **Closed[종료됨]** 상태의 변경 사항을 저장.

### 상태 만들기

notable event 조사 워크플로에 대한 상태를 만듦.

전제 조건

상태 전환을 제한하는 경우 워크플로에 새 상태가 필요한지, 어느 역할이 워크플로에서 새 상태를 우회할 수 있는지를 결정

절차

1. Splunk Enterprise Security 도구 모음에서 **설정 > 인시던트 관리 > 상태 설정**을 선택
2. **새 상태 만들기 > 이벤트(Create New Status > Notable)**를 선택.
3. 인시던트 검토 대시보드에 상태를 나타내는 **레이블**을 입력.
    예: ITOps 기다리는 중.
4. (선택 사항) 상태 설정 페이지에 표시되는 설명을 입력.
    예: IT 운영 부서 기다리는 중.
5. (선택 사항) 기본 상태 체크박스를 선택. **New** 이벤트 상태를 새로 만든 notable event의 기본 상태로 대체하려면 이 체크박스를 선택.
6. (선택 사항) 종료 상태 체크박스를 선택. False Positive와 같은 notable event의 **Closed[종료됨]** 상태를 추가하려면 이 체크박스를 선택.
7. (선택 사항) **활성화됨** 체크박스를 선택 해제. 이 체크박스를 사용하지 않고 이 상태를 만들려면 체크박스를 선택 해제.
8. **전환 후 상태(To Status)** 필드를 수정하여 상태 전환을 업데이트. 이 상태에서 다른 상태로 전환할 수 있는 역할을 선택하지 않으면 notable event를 이 상태로 전환한 후 아무도 notable event를 다른 상태로 이동할 수 없음. 상태 전환을 제한하지 않은 경우 각 상태에 대한 모든 역할을 선택.
9. 저장을 클릭.

사용자 역할을 기반으로 상태 전환을 제한하는 경우 이 새 상태로 전환할 수 있는 각 상태의 상태 전환을 수정.

### notable event 제거 작성 및 관리

notable event 제거를 작성하여 notable event를 인시던트 검토 대시보드에서 숨길 수 있음.

제거는 추가 notable event를 뷰에서 숨기는 검색 필터로, 과도하거나 원치 않는 수의 notable event가 인시던트 검토 대시보드에 표시되지 않도록 하는데 사용됨. 검색 조건을 충족하는 notable event는 계속 생성되고 주요 인덱스에 추가됨. 제거된 notable event는 보안 포스처 및 감사(audit) 대시보드에서 notable event 수에 계속 포함됨.

특정 조건을 충족하는 notable event가 생성되지 않도록 하려면 상관(correlation)검색에서 생성하는 대응 작업 수 조절을 참조.

제거 필터는 두 가지 방법으로 만들 수 있음.

- 인시던트 검토에서 제거 필터 만들기. notable event 제거를 참조.
- 설정 메뉴에서 제거 만들기. notable event 제거를 사용하여 제거 만들기를 참조.

#### notable event 제거를 사용하여 제거 만들기

1. 설정 > 인시던트 관리 > notable event 제거를 선택.
2. 새 제거 만들기를 클릭.
3. 제거 필터의 이름과 설명을 입력.
4. 제거할 notable event를 찾는 데 사용할 검색을 입력.
5. 만료 시간을 설정함. 이 설정은 제거 필터의 시간 제한을 정의함. 시간 제한이 충족되면 제거 필터가 비활성화.

#### notable event 제거 편집

1. 설정 > 인시던트 관리 > notable event 제거를 선택.
2. notable event 제거를 선택하여 notable event 제거 편집 페이지를 염.
3. 제거 필터에 사용되는 설명 및 검색 필드를 편집함.
4. 저장을 클릭.

#### notable event 제거 비활성화

1. 설정 > 인시던트 관리 > notable event 제거를 선택.
2. notable event 제거 상태 컬럼에서 비활성화를 선택.

#### notable event 제거 삭제

1. Splunk 플랫폼 도구 모음에서 설정 > Event type을 선택.
2. 제거 이벤트 \<code\>\<font size="2"\>notable_suppression-\<\<i\>suppression_name\<\/i\>\>\<\/font\>\<\/code\>을 검색합니다 notable_suppression-\<suppression_name\>.
3. notable event 제거 작업 컬럼에서 삭제를 선택.

#### notable event 제거 감사 (audit)

제거 감사(audit) 대시보드를 통해 notable event 제거를 감사(audit)함. Splunk Enterprise Security 사용의 제거 감사
(audit)를 참조.

## expandtoken 명령어를 사용하여 notable event의 토큰 확장

notable event 제목 및 설명의 토큰은 인시던트 검토 대시보드의 토큰 값을 포함하도록 자동 확장됨. expandtoken 검색 명령어를 사용하여 검색 결과에서 토큰 교체가 발생하도록 토큰을 확장할 수 있음.

설명

notable event의 제목(rule_name) 또는 설명(rule_description)과 같이 값에 토큰이 포함된 notable event의 필드를 확장함.
인시던트 검토 대시보드에서는 토큰이 자동으로 확장되지만 검색 내에서는 자동으로 확장되지 않음.

구문

```sql
... | expandtoken [field],[field1],[field2]...
```

선택 인수

field

설명: 확장할 토큰이 포함된 notable event의 필드 이름. 토큰의 이름을 지정하지 않음. 쉼표로 구분된 추가 필드를 지정함. 필드를 지정하지 않으면 모든 필드가 토큰 확장을 위해 처리됨. notable event의 예제 필드 리스트는 Splunk 개발자 포털에서 검색에 notable event 사용을 참조.

사용법

expandtoken 명령어는 스트리밍 명령어임.

제한 사항

검색 명령어에서 필드 이름의 중간에 토큰 구분자를 사용할 수 없음.

다른 토큰의 확장에 종속되는 토큰은 토큰이 확장되는 순서를 지정할 수 없기 때문에 안전하게 확장할 수 없을 수도 있음. 예를 들어 rule_description: "$src$에서 무차별 공격(Brute Force) 액세스 동작 탐지됨." 및 drilldown_name:
"$rule_description$의 기여 이벤트 참조"가 있는 경우 다음 검색이 $rule_description$ 토큰을 확장하지 않고 $src$ 토큰을
확장할 수 있음.

```sql
`notable` | expandtoken
```

토큰에 대한 자세한 내용은 Splunk Enterprise 대시보드 및 시각화 매뉴얼의 대시보드에서의 토큰 사용을 참조.

### 예

#### 모든 notable event의 토큰 확장

```sql
`notable` | expandtoken rule_title,rule_description,drilldown_name,drilldown_search
```

#### 특정 notable event의 토큰 확장

event_id 필드를 기반으로 특정 notable event의 토큰을 확장함.

```sql
`notable` |where event_id="<event_id>" | expandtoken rule_title,rule_description
```

짧은 ID 필드를 기반으로 특정 notable event의 토큰을 확장함.

```sql
`notable` | where notable_xref_id="<short ID>" | expandtoken rule_title,rule_description
```

#### 참고 항목

notable event의 예제 필드 리스트는 Splunk 개발자 포털에서 검색에 notable event 사용을 참조.
토큰에 대한 자세한 내용은 Splunk Enterprise 대시보드 및 시각화 매뉴얼의 대시보드에서의 토큰 사용을 참조.

## Splunk Enterprise Security에서 조사 관리

Enterprise Security 관리자는 보안 조사에 대한 액세스를 관리하고 애널리스트의 작업 이력을 사용해 문제를 해결하여 애널리스트를 지원할 수 있음.
애널리스트 조사 워크플로에 대한 자세한 내용은 Splunk Enterprise Security 사용에서 Splunk Enterprise Security의 조사를 참조.

### 조사 액세스 관리

ess_admin 역할을 보유한 사용자는 기본적으로 조사를 만들고, 보고, 관리할 수 있음. ess_analyst 역할을 보유한 사용자는 조사를 만들고 편집할 수 있음. 기능을 변경하려면 권한 대시보드를 사용.

- 다른 사용자가 조사를 만들고 편집할 수 있게 허용하려면 헤당 사용자의 역할에 조사 관리 기능을 추가함. 사용자는 협력 인원 권한이 있는 조사만 변경할 수 있음.
- 다른 사용자가 모든 조사를 관리하고, 보고, 삭제할 수 있게 허용하려면 해당 사용자의 역할에 모든 조사 관리 기능을 추가함.

설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조.

특정 조사 협력 인원의 쓰기 권한을 설정하여 누가 조사를 변경할 수 있는지 관리할 수 있음. 기본적으로 모든 협력 인원

은 자신이 추가된 조사에 대해 쓰기 권한을 갖지만, 같은 시간 표시줄의 다른 협력 인원이 해당 권한을 읽기 전용으로 변경할 수 있음. Splunk Enterprise Security 사용에서 조사의 협력 인원 변경을 참조.
사용자가 조사를 만든 후에는 모든 조사 관리 기능을 보유한 모든 사용자가 조사를 볼 수 있지만, 조사의 협력 인원만 조사를 편집할 수 있음. 조사 KV 스토어 컬렉션을 Lookup으로 표시할 수 없음. 관리자 역할이 있는 사용자만 KV 스토어 API endpoint를 사용하여 KV 스토어 컬렉션을 보거나 수정할 수 있음. KV 스토어 API endpoint에 대한 자세한 내용은
Splunk Enterprise REST API 조회 매뉴얼의 KV 스토어 endpoint 설명을 참조.

### 조사 데이터 원본

Splunk Enterprise Security는 조사 정보를 여러 KV 스토어 컬렉션에 저장함. 조사 대시보드에 있는 조사와 조사에 추가된 항목, 조사의 메모에 추가된 첨부 파일, 그리고 조사 워크벤치에 추가된 아티팩트의 컬렉션이 각각 있음. Splunk Enterprise Security 대시보드 요구사항 표에서 조사를 참조.
Splunk Enterprise Security 4.6.0 이전 버전에서 만든 조사의 조사 세부 정보는 다음 두 가지 KV 스토어 컬렉션, investigative_canvas 및 investigative_canvas_entries에 저장됨. 이 컬렉션은 4.6.0 버전에서 보존되지만, 새 조사 KV 스토어 컬렉션에 콘텐츠가 추가됨.

### 조사 작업 이력 항목 문제 해결

애널리스트가 조사에 추가할 작업 이력의 유형을 선택하면 선택한 시간 범위 동안 다섯 개 중 하나의 검색이 실행.

- 대시보드 뷰 - 작업 이력
- 검색 추적 - 작업 이력
- 패널별 필터링 - 작업 이력
- 주요 제거 - 작업 이력
- 주요 상태 - 작업 이력

설정 > 콘텐츠 관리로 이동하고 페이지에서 필터를 사용하여 검색을 봄. 이런 saved search을 변경하면 작업 이력 항목이 작업 이력에 더 이상 표시되지 않을 수 있음. 작업 이력에서 검색을 제외하려면 작업 이력 검색 추적 허용 리스트 Lookup을 사용. Splunk Enterprise Security에서 Lookup 작성 및 관리를 참조.

## 조사 워크벤치 관리 및 사용자 지정

워크벤치는 애널리스트가 한 위치에서 조사 작업을 수행할 수 있도록 허용하여 Splunk Enterprise Security의 기존 기능을 확장함. 애널리스트는 워크벤치에서 패널, 탭 및 프로파일을 사용하여 아티팩트 또는 자산 및 ID를 조사. 애널리
스트를 돕기 위해 패널, 탭 및 프로파일을 만들어 워크벤치를 사용자 지정할 수 있음. notable event에서 시작하는 조사의 속도를 높이기 위해 notable event로부터 아티팩트 추출을 설정할 수도 있음.

워크벤치에는 패널, 탭 및 프로파일용 메타데이터를 관리하기 위해 사용되는 설정 파일, es_investigations.conf이 도입됨. es_investigations.conf 파일에 스탠자를 추가하여 파일 시스템에서 변경할 수 있음. 자세한 내용은
es_investigations.conf.spec 및 es_investigations.conf.example을 참조.

### 조사 워크벤치용 패널 및 탭 만들기

조사 워크벤치에는 워크벤치 패널 참조가 있고 워크벤치 탭에 추가된, 미리 작성된 패널이 모두 표시.

1. 미리 작성된 패널을 만들거나 수정. 이 항목의 조사 워크벤치를 위해 미리 작성된 패널 만들기 또는 수정을 참조.
2. 미리 작성된 패널을 참조하는 워크벤치 패널을 만듬. 이 항목의 조사 워크벤치용 탭 만들기를 참조.
3. 워크벤치 패널이 포함된 워크벤치 탭을 만듬. 이 항목의 조사 워크벤치용 탭 만들기를 참조.

전체 프로세스의 예는 이 항목의 패널 변환 및 워크벤치 패널 만들기의 예를 참조.

#### 조사 워크벤치를 위해 미리 작성된 패널 만들기 또는 수정

조사 워크벤치에서 어느 미리 작성된 패널이나 사용할 수 있음. 워크벤치만을 위한 패널을 만들거나 기존 패널을 수정할 수 있음. Splunk Enterprise Security를 사용하여 여러 가지 방법으로 미리 작성된 패널을 만들거나 수정할 수 있음.

- 콘텐츠 관리에서 패널을 만듬.
  1. ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택.
  2. 새 콘텐츠 만들기 > 패널을 선택.
  3. 미리 작성된 패널 ID를 입력.
  4. 대상 앱을 선택.
  5. 미리 작성된 패널 XML을 입력.
  6. 저장을 클릭.
- 대시보드 패널을 미리 작성된 패널로 변환함. 대시보드 및 시각화의 기존 패널을 미리 작성된 패널로 변환을 참조.
- Splunk 설정에서 패널을 수정.
  1. Splunk 메뉴 모음에서 설정 > 사용자 인터페이스를 선택.
  2. 미리 작성된 패널을 클릭하고 수정할 패널에 대해 편집 > 패널 편집을 클릭.
    기존의 미리 작성된 패널을 수정하려면 수정하기 전에 복제하는 것을 고려함. 패널을 복제하는 경우 워크벤치용 패널을 기억할 수 있도록 패널 ID를 변경.
- Splunk 설정에서 패널을 만듬. 대시보드 및 시각화의 대시보드에 패널 추가를 참조.

워크벤치를 위해 미리 작성된 패널을 만들거나 수정하는 경우 최고의 사용자 경험을 위한 이 지침을 따름.

- 패널 검색에 토큰을 하나 이상 추가하여 워크벤치에서 조사된 아티팩트로 검색 결과를 제한함. 두 개 이상의 아티팩트 유형을 대체하려면 여러 토큰을 사용. $token$ 구문을 사용하여 토큰을 정의함. 워크벤치 패널을 만들 때 토큰의 형식을 설정함.
- 패널 XML에서 패널 이름을 제거함. 이렇게 하지 않으면 워크벤치에 두 개의 패널 제목이 표시. 워크벤치 패널은 워크벤치 패널을 만들 때 레이블 필드에서 제목을 가져옴.
- 애널리스트가 패널에서 아티팩트를 추가할 수 있도록 패널에 드릴다운을 추가함. 패널 XML에 \<option name="drilldown"\>cell\</option\> 구문을 사용하여 드릴다운을 추가함. 워크벤치는 사용자 지정 검색과 같은 기존 패널 드릴다운을 패널의 워크벤치 범위에 아티팩트를 추가할 수 있는 이 기능으로 대체함.
- Splunk Enterprise Security와 공유할 수 있도록 패널의 권한을 업데이트함. 패널이 앱에서 공유됨이거나 표시: 모든 앱으로 설정되어 있는지 확인함.
- (선택 사항) 패널에서 워크벤치에 의해 설정된 것과 다른 시간 범위를 사용하려면 패널 검색 또는 패널 XML에서 시간 범위를 설정함.

그런 다음 절차에 따라 조사 워크벤치용 패널을 만듬. 이 항목의 조사 워크벤치용 패널 만들기를 참조.

#### 조사 워크벤치용 패널 만들기

워크벤치 패널을 만듬.

1. 설정 > 콘텐츠 관리를 선택.
2. 새 콘텐츠 만들기 > 워크벤치 패널을 선택.
3. 드롭다운 리스트에서 워크벤치에 사용할 미리 작성된 패널을 선택.
4. (선택 사항) 워크벤치의 기본 패널 제목을 대체할 레이블을 입력.
5. (선택 사항) 패널에 대한 정보를 제공하는 설명을 입력.
6. 패널 검색의 토큰을 대체할 토큰을 추가함. 이 항목의 패널 변환 및 워크벤치 패널 만들기의 예를 참조하거나 대시보드 및 시각화 매뉴얼의 Splunk Enterprise에서 다중선택 입력 정보를 위한 토큰 정의를 참조.
7. 저장을 클릭.

그런 다음 패널을 탭에 추가하여 워크벤치에 표시되도록 함.

#### 조사 워크벤치용 탭 만들기

특정 데이터 유형, 이용 사례 등에 관련된 정보를 표시하려면 탭을 만듬.

1. 설정 > 콘텐츠 관리를 선택.
2. 새 콘텐츠 만들기 > 워크벤치 탭을 선택.
3. 탭 이름을 입력. 이 이름은 es_investigations.conf의 스탠자 이름의 일부가 되며 레이블을 지정하지 않는 경우 레이블로 사용됨.
4. (선택 사항) 사용자에게 표시되는 워크벤치 탭의 이름을 제공하려면 레이블을 입력.
5. 워크벤치 패널에서 이 탭에 표시할 패널을 선택. 패널은 선택하는 순서에 따라 워크벤치의 이 탭에 표시.
6. (선택 사항) 이 탭과 연결할 워크벤치 프로파일을 선택. 한 탭은 한 프로파일과만 연결할 수 있음. 애널리스트는 프로파일을 사용하여 워크벤치의 이용 사례와 관련된 여러 탭을 로드할 수 있음.
7. (선택 사항) 기본적으로 로드 선택 항목을 변경. 모든 워크벤치 조사를 위해 이 탭을 로드하려면 True를 선택.
8. (선택 사항) 탭의 설명을 입력. 설명을 입력하면 애널리스트가 탭의 패널을 사용하여 수집할 수 있는 정보 및 컨텍스트의 유형을 결정하는데 도움이 됨.
9. 저장을 클릭.

#### 패널 변환 및 워크벤치 패널 만들기의 예

전제 조건

Blue Coat ProxySG용 Splunk 추가 기능을 설치해야 하고 Splunk Enterprise Security 배포 추가 기능의 데이터가 있어야
함. Splunkbase에서 Blue Coat ProxySG용 Splunk 추가 기능을 다운로드할 수 있음.

1. 입력 토큰을 워크벤치와 함께 사용하려면 패널을 복제하고 검색을 수정.
    1. 설정 > 사용자 인터페이스를 선택.
    2. 미리 작성된 패널을 클릭.
    3. Splunk_TA_bluecoat-proxysg의 actions_by_destination_ip에 대한 편집 > 복제를 클릭.
    4. 미리 작성된 패널 ID를 입력. workbench_actions_by_dest_ip.
    5. 워크벤치에 두 개의 제목을 표시하지 않으려면 XML에서 제목을 제거함.
    6. 조사된 자산 아티팩트로 결과를 제한하는 토큰을 포함하도록 XML의 쿼리를 수정.
    sourcetype="bluecoat:proxysg:access*" $dest_token$ | iplocation dest | geostats count by action
    7. 패널의 \<earliest\> 및 \<latest\> 시간 범위를 제거할지 여부를 결정함. 이 시간 범위가 워크벤치에서 설정된 시간 범위보다 우선하므로 애널리스트가 상황에 맞는 검색을 수행할 수 있도록 제거하고 싶을 수도 있음.
    8. 저장을 클릭.
2. 패널의 권한을 수정.
    1. 방금 만든 패널, workbench_actions_by_dest_ip를 찾음.
    2. 편집 > 권한 편집을 선택.
    3. 표시에서 모든 앱을 선택.
    4. 저장을 클릭.
3. Splunk Enterprise Security로 돌아가 워크벤치에 사용될 패널을 설정함.
    1. 설정 > 콘텐츠 관리를 선택.
    2. 새 콘텐츠 만들기 > 워크벤치 패널을 선택.
    3. workbench_actions_by_dest_ip의 패널 이름을 선택.
    4. (선택 사항) 워크벤치에서 사용자에게 표시되는 레이블을 입력합니다: 대상별 프록시 작업.
    5. (선택 사항) 워크벤치에서 사용자에게 표시되는 설명을 입력합니다: 가능한 경우 조사된 자산에 해당하는 대상 IP별 작업을 그래프로 표시하는 지도를 표시.
    6. 검색의 $dest_token$에 대한 토큰을 추가하려면 토큰 추가를 클릭.
    7. 토큰 이름에 해당하는 토큰 이름을 입력. dest_token
    8. \(의 접두사를 입력.
    9. \)의 접미사를 입력.
    10. dest="의 값 접두사를 입력.
    11. "의 값 접미사를 입력.
    12. 구분자에 대한 Is Null 체크박스를 선택 취소하고 텍스트 상자에 OR 을 입력. OR의 양쪽에 공백을 포함함.
    13. 기본 필드에 대한 Is Null 체크박스를 선택된 상태로 놔둠. 이 체크박스가 선택되면 워크벤치에서 관련 유형의 아티팩트가 선택된 경우에만 검색이 실행. 이 경우 워크벤치에서 자산을 조사하고 있는 경우에만 검색이 실행.
    14. 아티팩트의 유형을 선택.
    15. 대상이 ID가 아니라 자산이므로 자산의 필드 유형을 선택.
    16. 저장을 클릭.
    이 패널에는 이제 다음과 같이 워크벤치에서 조사된 두 자산에 대해 구성되는 검색이 포함됨.
    ```sql
    sourcetype="bluecoat:proxysg:access*" (dest="<investigated_asset_1>" OR dest="<investigated_asset_2>") |
    iplocation dest | geostats count by action
    ```
4. 새 패널을 새 탭에 추가함.
    1. 콘텐츠 관리에서 새 콘텐츠 만들기 > 워크벤치 탭을 선택.
    2. proxy_data의 탭 이름을 입력. 이 이름은 es_investigations.conf의 스탠자 이름이 되며 레이블이 지정 되지 않는 경우 레이블로 사용됨.
    3. (선택 사항) 프록시 데이터의 레이블을 입력.
    4. 워크벤치 패널에서 대상 IP별 프록시 작업 패널을 입력하고 선택.
    5. 기본적으로 로드의 경우 False로 놔둠. 모든 워크벤치 조사를 위해 이 탭을 로드하려면 True를 선택.
    6. (선택 사항) 탭의 설명을 입력. 조사된 자산 및 ID와 관련된 프록시 데이터
    7. 저장을 클릭.

그런 다음 애널리스트는 워크벤치를 열고 새 탭을 추가하여 워크벤치에서 프록시 데이터를 조사하기 시작할 수 있음.

#### 워크벤치 프로파일 만들기

워크벤치의 프로파일을 사용하여 특정 이용 사례에 적합한 여러 탭을 서로 연결할 수 있음. 예를 들어, DDoS 조사 프로파일에 방화벽 데이터 탭 및 일반 네트워크 데이터 탭이 포함될 수 있음. 그런 다음 애널리스트는 DDoS 조사 프로파일을 조사에 추가하여 조사에 적합한 탭을 개별적으로 추가하는 대신 두 탭을 모두 워크벤치에 추가할 수 있음.

1. 설정 > 콘텐츠 관리를 선택.
2. 새 콘텐츠 만들기 > 워크벤치 프로파일을 선택.
3. 프로파일 이름을 입력. 이 이름은 es_investigations.conf의 스탠자 이름이 되며 레이블이 지정되지 않는 경우 레이블로 사용됨.
4. (선택 사항) 사용자에게 표시되는 워크벤치 프로파일의 이름을 제공하려면 레이블을 입력.
5. (선택 사항) 프로파일의 설명을 입력. 설명을 입력하면 애널리스트가 프로파일을 조사에 추가하여 수집할 수 있는 정보 및 컨텍스트의 유형을 결정하는데 도움이 됨.
6. 저장을 클릭.

프로파일을 만든 후 만든 프로파일을 사용하여 탭을 업데이트함. DDoS 조사 예의 경우 방화벽 데이터 및 네트워크 데이터 탭을 편집하고 새 DDoS 조사 프로파일을 선택.

#### notable event의 아티팩트 추출 설정

notable event가 연결된 각 상관(correlation)검색에 대해 notable event가 조사에 추가될 때 워크벤치에서 ID 또는 자산으로 자동 추출되는 필드를 정의할 수 있음. 기본적으로 자산 및 ID 상관(correlation)에 사용되는 것과 같은 필드가 포함된 상관(correlation)검색에 의해 생성된 notable event에서 추출되는 필드임. 모든 사용자 지정 상관(correlation)검색에 대해 추출될 필드를 추가해야 함.

|조사 아티팩트의 유형|조사 범위에 대해 추출되는 필드|
|:--:|:--:|
|자산|dest, src, dvc, orig_host|
|ID|user, src_user|

상관(correlation)검색에서 데이터 모델을 사용하지 않거나 검색 결과에 추출하고자 하는 필드와 다른 필드가 포함된 경우 조사 범위 내로 추출할 필드를 지정할 수 있음.

1. 설정 > 콘텐츠 관리를 선택.
2. 사용자 지정할 상관(correlation)검색을 클릭하여 열어서 편집함.
3. notable event adaptive response 작업을 선택.
4. 자산 추출의 경우 상관(correlation)검색 결과에서 자산을 나타내는 필드 이름을 입력. Enter 키를 눌러 필드 이름을 추가함.
5. ID 추출의 경우 상관(correlation)검색 결과에서 ID를 나타내는 필드 이름을 입력. Enter 키를 눌러 필드 이름을추가함.
6. 저장을 클릭.

## Splunk Enterprise Security에서 조사 상태 관리 및 사용자 지정

버전 5.0.0부터 조사에 상태를 추가할 수 있음. 이 버전으로 업그레이드한 후 상태가 없었던 조사에는 새 조사 상태가 배정됨.
조사의 상태를 변경하려면 애널리스트에게 전환할 상태에 대한 transition_reviewstatus-\<x\>_to_\<y\> 기능이 있어야 함.
ess_analyst 역할과 ess_admin 역할에는 기본적으로 모든 상태에 대해 이 기능이 있음. 조사의 상태 전환을 수정하면 이 기능이 수정됨.

애널리스트로서 상태를 변경하려면 edit_reviewstatuses 기능이 있어야 함. ess_admin 역할에는 기본적으로 이 기능이
있음. 설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조.

### 조사 상태 만들기

조사를 수행할 때 애널리스트가 선택할 상태를 만듬.
상태 전환을 제한하는 경우 상태를 만든 후 상태 전환을 업데이트함. 그렇지 않으면 애널리스트가 새 상태를 선택할
수 없음. 이 항목의 조사에 대한 상태 전환 제한을 참조.

1. Enterprise Security 도구 모음에서 설정 > 인시던트 관리 > 상태 설정을 선택.
2. (선택 사항) 기존 조사 상태를 검토하려면 조사 탭을 선택.
3. 새 상태 만들기 > 조사를 선택.
4. 조사에서 상태의 이름으로 표시되는 레이블을 입력.
    예: 데스크톱 IT 기다리는 중.
5. (선택 사항) 상태를 설명하려면 상태 설정 페이지에 표시되는 설명을 입력.
    예: 조사에서 데스크톱 IT가 추가 해결 조치 또는 포렌식 단계를 수행하기를 기다리고 있음.
6. (선택 사항) 이 상태를 새로 만든 조사의 기본값으로 설정하려면 기본 상태 체크박스를 선택.
7. (선택 사항) 이 상태를 새로 만든 조사의 가능한 마지막 상태로 설정하려면 종료 상태 체크박스를 선택.
8. (선택 사항) 아무도 아직 사용할 수 없는 상태를 만들려면 활성화됨 체크박스를 선택 해제.
9. 조사를 이 새 상태(예: 데스크톱 IT 기다리는 중)에서 다른 상태(예: 종료됨)로 전환할 수 있는 사용자 역할을 업데이트 함. 이 상태에서 다른 상태로 전환할 수 있는 역할을 선택하지 않으면 조사를 이 상태로 전환한 후 아무도 조사를 다른 상태로 이동할 수 없음.
10. 저장을 클릭.

### 조사의 상태 전환 제한

조사에서 수행할 수 있는 상태 전환에 따라 조사의 경로가 정의됨. 기본적으로 모든 상태의 조사를 다른 상태로 변경할 수 있음. 예를 들어, 누군가 새 조사 상태인 조사의 상태를 다른 상태(예: 종료됨)로 변경할 수 있음.

애널리스트가 조사 시 선택할 수 있는 상태를 제한할 수 있음. 어떤 상태를 요구할 것인지 결정하고, 애널리스트가 조사를 완료하기 전에 특정한 상태 순서를 따라야 하는지 결정함. 상태의 전체 순서를 우회할 수 있는 역할이 있는지 결정함.

이 예는 애널리스트를 위해 상태 전환 제한을 설정하는 과정을 안내함. 새 이벤트에서 진행중 또는 보류중, 해결됨, 종료됨의 경로를 따라야 하도록 상태 전환을 제한함.

|1|2|3|4|
|:--:|:--:|:--:|:--:|
|새로 만들기|진행중|보류중|해결됨 종료됨|

전제 조건

ess_admin 역할을 보유하고 있거나 역할에 상태 편집 기능이 배정되어야 함. 사용자 역할 및 기능에 대한 자세한 내용은 설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조.

1. Splunk Enterprise Security 도구 모음에서 설정 > 인시던트 관리 > 상태 설정을 선택.
2. 조사 탭을 클릭.
3. 새 이벤트 상태에서 전환을 제한함. 새 이벤트 상태를 선택하여 조사 상태 편집 페이지를 염.
4. 상태 전환에서 해결됨 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제.
5. 종료됨 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제.
6. 저장을 클릭하여 새 이벤트 상태의 변경 사항을 저장함.
7. ess_analyst 역할이 새 이벤트 또는 종료됨으로 전환하지 못하도록 진행중 및 보류중 상태에서 전환을 제한함.
8. 조사 탭을 클릭하고 진행중 상태를 선택.
9. 상태 전환에서 새 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. 종료됨 상태에 대해 반복함.
10. 저장을 클릭하여 진행중 상태의 변경 사항을 저장함. 보류중 상태에 대해 이 단계들을 반복함.
11. 해결됨 상태를 제한함. 조사 탭을 클릭하고 해결됨 상태를 선택.
12. 상태 전환에서 새 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. 진행중 및 보류중 상태에 대해 반복함.
13. 저장을 클릭하여 해결됨 상태의 변경 사항을 저장함.
14. 종료됨 상태에 대한 전환을 제한함. 조사 탭을 클릭하고 종료됨 상태를 선택.
15. 상태 전환에서 새 이벤트 상태에 대한 역할을 선택하고 ess_analyst 역할에 대한 체크박스를 선택 해제. 진행중, 보류중 및 해결됨 상태에 대해 반복함.
16. 저장을 클릭하여 종료됨 상태의 변경 사항을 저장함.