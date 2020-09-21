# 콘텐츠 관리

## Splunk Enterprise Security에서 콘텐츠 관리

Splunk Enterprise Security 관리자는 콘텐츠 관리 페이지를 사용하여 상관(correlation)검색, 핵심 지표, 저장된 검색, 스윔레인(Swim lane) 검색 같은 Splunk Enterprise Security만의 고유 콘텐츠를 표시하고, 만들고, 설정하고, 편집할 수 있음.

- 상관(correlation)검색 작성
- 데이터 모델 작성 및 관리
- 키 표시기 검색 작성 및 관리
- Lookup 작성 및 관리
- 저장된 검색 작성 및 관리
- 검색 기반 Lookup 작성 및 관리
- 스윔 레인(Swim lane) 검색 작성 및 관리
- 뷰 작성 및 관리
- 콘텐츠를 앱으로 내보내기

참고 항목은 아래와 같음.

Splunk Enterprise Security에서 위험 개체 만들기 및 편집

## Splunk Enterprise Security에서 데이터 모델 작성 및 관리

Splunk Enterprise Security에서 콘텐츠 관리 페이지를 사용하여 데이터 모델을 작성하고 관리함.

- Splunk Enterprise Security에서 데이터 모델의 리스트를 검토함.
- 다음 예약 시간, 가속 상태를 검토하고 데이터 모델을 가속할 것인지 여부를 선택함.
- 데이터 모델을 편집하려면 데이터 모델 이름을 클릭함.

### 데이터 모델 만들기

1. Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 데이터 모델을 선택함.
3. Splunk 플랫폼 매뉴얼의 지침에 따라 데이터 모델을 만듬.

- Splunk Enterprise의 경우 Splunk Enterprise 지식 관리자 매뉴얼에서 데이터 모델 만들기를 참조.
- Splunk Cloud의 경우 Splunk Cloud 지식 관리자 매뉴얼에서 데이터 모델 만들기를 참조.

## Splunk Enterprise Security에서 키 표시기 검색 작성 및 관리

Splunk Enterprise Security의 콘텐츠 관리에서 핵심 지표 검색을 설정함. 필터를 사용하여 핵심 지표 유형을 선택하여 핵심 지표 검색만 볼 수 있음.

### 사용자 지정 핵심 지표 검색 만들기

대시보드 또는 글래스 테이블에 보안 메트릭으로 추가할 수 있는 핵심 지표를 만들기 위한 핵심 지표 검색을 만들기함.

1. Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 핵심 지표 검색을 선택함.
3. 핵심 지표 이름을 입력.
    핵심 지표가 글래스 테이블의 보안 메트릭 리스트에 표시되도록 하려면 범주나 보안 영역을 핵심 지표 이름의 시작 부분에 입력한 후 하이픈을 입력. 예: APT - 핵심 지표 예 또는 액세스 - 핵심 지표 샘플.
4. 검색과 기타 세부 정보를 입력.
    Enterprise Security와 함께 제공되는 핵심 지표는 데이터 모델을 사용하여 결과 반환 속도를 높임.
5. (선택 사항) 데이터 모델 가속를 사용자 지정 핵심 지표에 사용하려면 예약을 선택함.
6. 핵심 지표 값에 해당하는 필드의 이름을 값 필드에 입력.
7. 핵심 지표 변동에 해당하는 필드의 이름을 델타 필드에 입력.
8. (선택 사항) 핵심 지표의 임계값을 입력. 임계값은 핵심 지표 색상 변경 여부를 제어함. 임계값을 대시보드와 글래스 테이블에서도 설정할 수 있음.
9. 핵심 지표 뒤에 오는 단위나 다른 단어를 나타내는 값 접미사를 입력.
10. 핵심 지표 색상을 반전하려면 반전 체크박스를 선택함. 높은 값이 좋고 낮은 값이 나쁨을 나타내려면 이 체크박스를 선택.
11. 저장을 클릭함.

### 핵심 지표 검색 예약

Splunk Enterprise Security에 포함된 핵심 지표에는 데이터 모델 가속가 사용. 가속를 활성화하고 검색이 예약된 보고서로 실행되도록 예약함. 예약된 보고서 결과는 캐시에 저장되므로, 지표가 대시보드에 결과를 더 빨리 표시할 수 있음.

1. 설정 > 콘텐츠 관리를 선택함.
2. 가속할 핵심 지표 검색을 찾음.
3. 작업 컬럼에서 가속를 클릭함.
4. 가속 편집 창에서 가속 체크박스를 선택함.
5. Enterprise Security가 캐시에 저장된 결과를 업데이트해야 하는 빈도에 해당하는 새로 고침 빈도를 선택함.
6. 저장을 클릭함.

핵심 지표 가속 후 콘텐츠 관리 페이지의 다음 예약 시간이 채워지고 해당 지표의 번개 표시가 회색에서 노란색으로 변함.

### 핵심 지표 검색 편집

핵심 지표 검색을 변경함.

1. ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 핵심 지표 검색을 선택함.
3. (선택 사항) 검색 이름을 변경함.
4. (선택 사항) 검색이 저장되는 대상 앱을 변경함.
5. (선택 사항) 핵심 지표 제목을 변경함. 제목이 대시보드의 핵심 지표 위에 표시되거나 글래스 테이블의 보안 메트릭 옆에 표시됨.
6. (선택 사항) 대시보드에서 핵심 지표 함수의 유형을 설명하는데 사용되는 핵심 지표의 부제를 변경함.
7. (선택 사항) 핵심 지표를 채우는 검색 문자열을 변경함.
8. (선택 사항) 사용자 지정 검색 또는 대시보드 링크 같은 드릴다운 URL을 추가하여 기본 드릴다운 동작을 재정의함. 기본적으로, 핵심 지표 드릴다운은 핵심 지표 값을 생성한 검색 결과를 염. 글래스 테이블에 핵심 지표를 추가하는 경우 사용자 지정 드릴다운을 설정할 수 있음.
9. (선택 사항) 예약 체크박스를 선택하여 핵심 지표 가속를 활성화하고 지표가 대시보드에 더 빨리 로드될 수 있게 함.
10. (선택 사항) 표준 크론 표기법을 사용하여 크론 스케줄 빈도를 변경함.
11. (선택 사항) 임계값 동작을 변경하여 값 지표에 배정되는 색을 결정. 기본적으로 검정색 값 지표를 생성하는 임계값은 없으며, 임계값 숫자가 값 지표 카운트보다 크면 녹색 값 지표가 생성되고 임계값 숫자가 값 지표 카운트보다 작으면 빨간색 값 지표가 생성.
12. (선택 사항) 값 지표를 설명하는 값 접미사를 추가함. 예를 들어 단위를 지정할 수 있음. 대시보드에서 값 접미사는 값 지표와 추세 지표 사이에 나타남.
13. (선택 사항) 추세 지표 임계값의 기본 색을 변경하려면 반전 체크박스를 선택함. 이 체크박스를 선택할 경우, 임계값 숫자가 값 지표 카운트보다 크면 빨간색 값 지표가 생성되고 임계값 숫자가 값 지표 카운트보다 작으면 녹색 값 지표가 생성.
14. 저장을 클릭함.

## Splunk Enterprise Security에서 저장된 검색 작성 및 관리

Splunk Enterprise Security에서 예약된 보고서라고도 하는 저장된 검색을 작성함.

1. Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 저장된 검색을 선택함.
3. Splunk 플랫폼 매뉴얼의 지침에 따라 예약된 보고서라고도 하는 저장된 검색을 작성함.

- Splunk Enterprise의 경우, Splunk Enterprise 보고 매뉴얼에서 새 보고서 만들기를 참조.
- Splunk Cloud의 경우, Splunk Cloud 보고 매뉴얼에서 새 보고서 만들기를 참조.

- Enterprise Security에서 검색을 보고 관리할 수 있도록 Splunk 플랫폼 매뉴얼의 지침에 따라 보고서 권한을 수정하여 보고서를 Enterprise Security와 공유함.
      - Splunk Enterprise의 경우, Splunk Enterprise 보고 매뉴얼에서 보고서 권한 설정을 참조.
      - Splunk Cloud의 경우, Splunk Cloud보고 매뉴얼에서 보고서 권한 설정을 참조.

## Splunk Enterprise Security에서 검색 기반 Lookup 작성 및 관리

검색 기반 Lookup을 사용하면 주기적으로 예약된 간격에 실행되는 검색의 결과를 기반으로 Lookup을 만들 수 있음. 검색을 데이터 모델이나 기존 Lookup에 저장된 데이터에 대해서만 실행할 수 있음. 검색 기반 Lookup은 번들 복제에서 제외되고 인덱서로 전송되지 않음.

### 검색 기반 Lookup을 사용하는 경우

환경에서 새로운 일이 일어날 때 알고 싶거나 데이터 모델 또는 다른 Lookup에서 계속 변하는 정보에 따라 Lookup을 지속적으로 업데이트해야 하는 경우 검색 기반 Lookup을 만듬.
검색 기반 Lookup은 데이터 모델이나 기타 Lookup에서 정보를 수집하고 저장함. Lookup에 저장된 데이터는 이벤트에서 수집된 선택된 필드의 과거 요약을 나타냄. 대시보드에서 변경 사항을 보거나 상관(correlation)검색을 사용하여 검색 기반 Lookup의 데이터를 새 이벤트와 비교하고, 일치 항목이 있는 경우 경고할 수 있음. (예: 새로운 사용자가 웹 서버에 로그인할 때 알기 위해)

1. where 명령어를 사용하여 사용자 데이터를 인증 데이터 모델에서 검색하고 웹 서버 호스트 이름을 기준으로 필터링함.
2. 검색 결과가 환경에서 알려진 호스트 및 사용자와 일치하는지 확인.
3. 가이드식 검색 기반 Lookup을 만들어 웹 서버에 로그인하는 사용자에 대한 정보를 반복적인 일정에 따라 수집하고 저장함.
4. 검색 기반 Lookup의 이력 정보를 기반으로 사용자가 과거에 액세스한 적이 없는 웹 서버 중 하나에 로그인하면 알리는 상관(correlation)검색을 만듬.

### 검색 기반 Lookup 만들기

검색 기반 Lookup을 만듬.

1. Splunk Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 검색 기반 Lookup을 선택함.
3. (선택 사항) 앱을 선택함. 기본 앱은 SplunkEnterpriseSecuritySuite임. SA-NetworkProtection 같은 특정앱이나 사용자 지정 앱에서 Lookup을 만들 수 있음. 검색 기반 Lookup을 저장한 후에 앱을 변경할 수 없음.
4. (선택 사항) 검색에 대한 설명을 입력.
5. Lookup 레이블을 입력. 이 레이블은 콘텐츠 관리에 표시되는 검색 기반 Lookup 이름임.
6. Lookup 이름을 입력. Lookup을 저장한 후에 이름을 변경할 수 없음.
7. 검색을 실행할 빈도를 정의하는 크론 스케줄을 입력.
8. 검색에 대해 실시간 또는 연속 예약을 선택함. 실시간 예약에서는 검색 성능이 우선이지만, 연속 예약에서는 데이터 무결성이 우선임.
9. 저장된 검색의 이름을 정의하는 검색 이름을 입력. Lookup을 저장한 후에 이름을 변경할 수 없음.
10. 가이드 모드를 선택하여 검색 구문을 직접 만들지 않고 검색을 만들거나, 수동을 선택하여 검색을 직접 작성함.가이드식 검색 편집기로 검색을 작성하는데 도움이 되는 예를 참조.
11. 수동 모드에서 검색을 만드는 경우 검색을 입력.
12. 저장을 클릭하여 검색을 저장함.

### 검색 기반 Lookup 예

Splunk Enterprise Security에 포함된 이 검색 기반 Lookup 예에서는 IDS(침입 탐지 시스템)에서 확인된 공격을 추적함.
그런 다음 상관(correlation)검색을 사용하여 새 공격에 대한 알림을 수신하거나, 공격이 환경에 새로운 공격인지 확인할 수 있음. 침입 센터 대시보드에서는 이 검색 기반 Lookup을 새로운 공격 - 지난 30일 패널에 사용. 침입 센터 대시보드를 참조.

1. Splunk Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 검색 기반 Lookup을 선택함.
3. (선택 사항) SA-NetworkProtection을 앱으로 선택함. 검색 기반 Lookup을 저장한 후에 앱을 변경할 수 없음.
4. 설명에 "IDS에서 확인된 공격과 공격이 처음과 마지막으로 확인된 시간의 리스트 유지"를 입력.
5. Lookup 레이블에 IDS 공격 추적기 예를 입력. 이 레이블은 콘텐츠 관리에 표시되는 검색 기반 Lookup 이름임.
6. Lookup을 설명하는 고유 이름을 ids_attack_tracker_example로 입력. Lookup을 저장한 후에 이름을 변경할 수 없음.
7. 검색을 실행할 빈도를 정의하는 크론 스케줄을 입력. IDS에서 데이터를 자주 수집하는 경우, 25 \* \* \* \* 크론 스케줄을 입력하여 검색을 매 시간마다 25분에 매일 실행함.
8. Lookup이 모든 데이터 지점을 추적해야 하므로 연속 예약을 선택함.
9. 검색 이름에 Network - IDS Attack Tracker - Example Lookup Gen을 입력.
10. 가이드 모드를 선택하여 가이드식 검색 편집기를 사용해 검색을 만듬.
11. 가이드식 검색 편집기 열기를 클릭하여 검색을 만들기 시작함.
12. IDS 공격 데이터는 데이터 모델에 저장되므로 데이터 모델을 데이터 소스로 선택함.
13. Intrusion_Detection을 데이터 모델로 선택하고 IDS_Attacks를 데이터 모델 데이터 집합으로 선택함.
14. 요약 전용 필드에 대해 예를 선택하여 가속된 데이터 모델의 데이터에 대해서만 검색을 실행함.
15. 시작 시간인 70분 전에 분이 시작될 때 시작되고 지금 종료되는 상대 시간을 사용하는 시간 범위를 선택함. 적용을 클릭하여 시간 범위를 저장함.
16. 다음을 클릭함.
17. (선택 사항) where 절을 입력하여 데이터 모델의 데이터를 특정 IDS 벤더의 데이터로만 필터링하고 다음을 클릭함.
18. 집계 값을 추가하여 데이터에 대한 특정 통계를 추적하고 해당 정보를 Lookup에 저장함. 하나 이상의 집계가 필요함.
    1. 환경에서 IDS 공격이 처음 확인된 시간을 추적하려면 min 함수와 _time 필드가 있는 새 집계를 추가하고 firstTime으로 저장함.
    2. max 함수와 _time 필드가 있는 집계를 하나 더 추가하고 lastTime으로 저장하여 공격이 마지막으로 확인된 시간을 추적함. 그러면 Lookup에 firstTime과 lastTime이라는 2개 컬럼이 생성.
19. 분할 기준 절을 추가하여 더 많은 데이터 지점을 Lookup에서 추적함. 모든 분할 기준 절이 Lookup에 컬럼으로 표시됨.
    1. IDS_Attacks.ids_type 분할 기준 절을 추가하고 ids_type으로 이름을 변경하여 Lookup에서 IDS 유형을 모니터링함.
    2. IDS_Attacks.signature의 이름을 signature로 변경하는 분할 기준 절을 추가함.
    3. IDS_Attacks.vendor_product의 이름을 vendor_product로 변경하는 분할 기준 절을 추가함.
20. 다음을 클릭함.
21. Lookup에 데이터를 저장할 기간을 정의하는 보존 기간을 선택함. 예를 들어 이 Lookup에 5년 치 IDS 공격 증거를 저장해 두려고 함. IDS에서 공격을 마지막으로 확인한 시간을 기준으로 보존하려면 lastTime 시간 필드를 선택함. -5y를 시작 시간으로 입력하고 입력한 시간 값의 형식을 나타냄. %s. 시간 서식에 대한 설명은 Splunk 플랫폼 매뉴얼에서 확인할 수 있음.

- Splunk Enterprise의 경우, Splunk Enterprise 검색 참조 매뉴얼에서 날짜 및 시간 형식 변수를 참조.
- Splunk Cloud의 경우, Splunk Cloud 검색 참조 매뉴얼에서 날짜 및 시간 형식 변수를 참조.

다음을 클릭함.
마법사에서 만든 검색을 검토하고 완료를 클릭하여 가이드식 검색 편집기 사용을 마침.
저장을 클릭하여 검색을 저장함.

### 검색 기반 Lookup 수정

1. Splunk Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 검색 기반 Lookup을 유형으로 선택함.
3. 편집할 Lookup을 클릭함.
4. 변경한 후 저장을 클릭함.

### 검색 기반 Lookup을 채우는 검색 활성화 또는 비활성화

검색 기반 Lookup의 검색을 활성화하거나 비활성화하여 검색이 Lookup을 업데이트하지 않게 할 수 있음. 검색 기반 Lookup을 채우는 검색을 비활성화하면 검색이 Lookup 업데이트를 중지하고, Lookup의 데이터가 업데이트되지 않음. Lookup 안에 있는 데이터에 의존하는 상관(correlation)검색이나 대시보드가 최신 상태로 유지되지 않음.

1. 설정 > 콘텐츠 관리를 선택함.
2. 검색 기반 Lookup 유형을 기준으로 필터링하고 활성화하거나 비활성화할 검색 기반 Lookup을 염.
3. 검색 기반 Lookup의 검색 이름을 찾음.
4. Splunk 플랫폼 메뉴 모음에서 설정 > 검색, 보고서, 경고를 선택함.
5. 검색을 찾아서 활성화하거나 비활성화함.

## Splunk Enterprise Security에서 스윔 레인(Swim lane) 검색 작성 및 관리

스윔 레인(Swim lane) 검색을 만들어 Asset Investigator 또는 Identity Investigator 대시보드에 추가할 수 있는 스윔 레인(Swim lane)을 만듬. Investigator 대시보드의 스윔 레인(Swim lane)은 일정 기간 동안 특정 자산 또는 ID별로 작업을 프로파일링하는데 도움이 됨.

1. Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 스윔 레인(Swim lane) 검색을 선택함.
3. 검색 이름을 입력.
4. 대상 앱을 선택함.
5. 대시보드에 표시되는 스윔 레인(Swim lane)의 제목을 입력.
6. 스윔 레인(Swim lane)을 채우는 검색을 입력.
7. 사용자가 스윔 레인(Swim lane) 항목을 클릭하면 실행되는 드릴다운 검색을 입력. 스윔 레인(Swim lane) 항목 드릴다운은 기본적으로 원시 이벤트를 표시.
8. 색상을 선택함.
9. 자산 또는 ID를 엔티티 유형으로 선택함.
10. 제약조건 필드를 입력. 검색에 제약조건을 지정하는 필드를 입력. 이런 제약조건 필드를 검색에 사용하려면 검색에 where $constraints$가 포함되어야 함. 각 스윔 레인(Swim lane) 검색 유형에는 특정 제약조건만 유효함. 예를 들어 Malware 데이터 모델과 Malware_Attacks 데이터 모델 데이터 집합을 사용하는 Asset Investigator 스윔레인(Swim lane) 검색에서는 Malware_Attacks.user 필드를 제약조건으로 지정할 수 있음.
11. 저장을 클릭함.

예

예를 들어 특정 자산과 관련된 인증 이벤트를 모두 식별하는 스윔 레인(Swim lane)을 만들 수 있음.

1. 검색 이름에 자산별 인증 - 예를 입력.
2. DA-ESS-AccessProtection을 대상 앱으로 선택함.
3. 대시보드에 표시되는 스윔 레인(Swim lane)의 제목을 입력. 모든 인증.
4. 스윔 레인(Swim lane)을 채우는 검색을 입력.
    ```sql
    | tstats `summariesonly` values(Authentication.action) as action,values(Authentication.app) as
    app,values(Authentication.src) as src,values(Authentication.dest) as dest,values(Authentication.user) as
    user,count from datamodel=Authentication.Authentication where $constraints$ by _time span=$span$
    ```
5. 드릴다운 검색을 입력.
    ```sql
    | `datamodel("Authentication","Authentication")` | search $constraints$
    ```
6. 보라색을 선택함.
7. 모든 인증 이벤트를 자산별로 조사하고 이 스윔 레인(Swim lane)을 Asset Investigator 대시보드에 추가할 수 있기를 원하므로, 자산을 엔티티 유형으로 선택함. 이렇게 지정하면 제약조건 필드로 지정된 모든 제약조건이 자산을 식별하는 다른 필드를 대상으로 역 Lookup을 수행함.
8. 제약조건 필드에 Authentication.src 및 Authentication.dest를 입력하여 특정 자산에서 비롯되거나 특정 자산을 대상으로 하는 인증을 식별함.

IP 주소가 1.2.3.4고 dns가 server.example.com이고 nt_host가 server1인 자산 Lookup 항목이 있다고 가정하면, 이 스윔 레인(Swim lane)에 대한 검색에서는 인증 이벤트의 원본과 대상이 1.2.3.4, server.example.com 또는 server1인 인증 이벤트를 모두 검색함.

```sql
... Authentication.src=1.2.3.4 OR Authentication.src=server.example.com OR Authentication.src=server1 OR
Authentication.dest=1.2.3.4 OR Authentication.dest=server.example.com OR Authentication.dest=server1
```

## Splunk Enterprise Security에서 뷰 작성 및 관리

콘텐츠 관리에서 단순 XML을 사용하여 새 뷰 또는 대시보드를 만듬.

전제 조건

콘텐츠 관리에서 새 뷰 또는 대시보드를 만들려면 단순 XML에 대해 잘 알고 있어야 함. 단순 XML을 사용한 작업 등 대시보드 작성 및 편집에 대한 개요는 Splunk 플랫폼 매뉴얼을 참조.

- Splunk Enterprise의 경우, Splunk Enterprise 대시보드 및 시각화에서 대시보드 개요를 참조.
- Splunk Enterprise의 경우, Splunk Enterprise 대시보드 및 시각화에서 대시보드 개요를 참조.

작업

1. Enterprise Security 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기를 클릭하고 뷰를 선택함.
3. 단순 XML을 사용하여 새 대시보드를 만듬.
4. 권한을 수정하여 새 뷰를 Enterprise Security에서 보고 관리할 수 있도록 Enterprise Security와 공유함.
    1. Splunk 메뉴 모음에서 설정 > 사용자 인터페이스 > 뷰를 선택함.
    2. 좀 전에 만든 뷰 이름을 찾음.
    3. 권한을 클릭하고 뷰를 Enterprise Security와 공유하도록 권한을 수정함.
    4. 저장을 클릭함.

대화형 대시보드 편집기를 사용하여 새 대시보드를 만들 수도 있음. 검색 > 대시보드를 선택하여 대시보드 페이지를 염. 대시보드 편집기에 대한 내용은 Splunk 플랫폼 매뉴얼에서 확인할 수 있음.

- Splunk Enterprise의 경우, Splunk Enterprise 대시보드 및 시각화에서 대시보드 편집기 열기를 참조.
- Splunk Cloud의 경우, Splunk Cloud 대시보드 및 시각화에서 대시보드 편집기 열기를 참조.

탐색 편집기를 사용하여 배포의 메뉴에 표시되는 대시보드를 변경함. 자세한 내용은 Splunk Enterprise Security의 메뉴 모음 사용자 지정을 참조.

## Splunk Enterprise Security에서 콘텐츠를 앱으로 내보내기

Content Management 페이지에서 Splunk Enterprise Security의 콘텐츠를 앱으로 내보냄. 내보내기 옵션을 사용하여 사용자 지정 검색을 배포 또는 테스트 환경에서 실제 환경으로 마이그레이션하는 등 사용자 지정 콘텐츠를 다른 ES 인스턴스와 공유함. 콘텐츠 관리 페이지에서 상관(correlation)검색, 글래스 테이블, 데이터 모델 및 뷰 같은 모든 유형의 콘텐츠를 내보낼 수 있음.

기본적으로 관리자 사용자만 콘텐츠를 내보낼 수 있음. 내보내기 기능을 다른 역할에 추가하는 방법은 설치 및 업그레이드 매뉴얼에서 역할에 기능 추가를 참조.

1. ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택함.
2. 내보낼 콘텐츠의 체크박스를 선택함.
3. 선택 사항 편집을 클릭하고 내보내기를 선택함.
4. 앱 이름을 입력. 입력한 이름은 파일 시스템에서 앱 이름이 됨.
    예: SOC_custom.
5. 앱 이름 접두사를 선택함. 기본 앱 가져오기 규칙을 수정하지 않고 콘텐츠를 Splunk Enterprise Security로 다시 가져오려면 DA-ESS-를 선택함. 그렇지 않으면 접두사 없음을 선택함.
6. 레이블을 입력. 레이블은 앱의 이름임.
    예: Custom SOC 앱.
7. 앱 버전과 빌드 번호를 입력.
8. 내보내기를 클릭함.
9. 지금 앱 다운로드를 클릭하여 앱 패키지를 $SPLUNK_HOME/etc/apps/SA-Utils/local/data/appmaker/* 위치에 있는 검색헤드에 다운로드함.
10. 닫기를 클릭하여 콘텐츠 관리로 돌아감.

### 내보낸 콘텐츠의 한계

내보낸 콘텐츠는 오래된 Enterprise Security 버전에서 사용하지 못할 수 있음. 다음 항목이 내보낸 콘텐츠에 포함되거나 포함되지 않을 수 있음.

<table>
<tr><td>내보낸 항목</td><td>내보내기에 포함됨</td><td>내보내기에 포함되지 않음</td></tr>
<tr><td>데이터 모델</td><td>datamodels.conf 및 데이터모델 JSON 정의.</td><td>해당 없음</td></tr>
<tr><td>상관(correlation), 핵심 지표,스윔 레인(Swim lane) 검색을 포함한 저장된 검색</td><td>
savedsearches.conf<br/>
governance.conf<br/>
위험 배정, 스크립트 이름, 이메일 주소를 포함한 경고 작업 및 대응 작업</td><td>매크로, 스크립트 파일, Lookup 또는 검색 개체에서 참조되는 바이너리 파일 콘텐츠 생성 검색, 컨텍스트 또는 검색개체에서 참조되는 개념 같은 극단 검색 개체</td></tr>
<tr><td>검색 기반 Lookup</td><td>
savedsearches.conf<br/>
governance.conf<br/>
managed_configurations.conf<br/>
collections.conf<br/>
transforms.conf</td><td>매크로, 스크립트 파일, Lookup 또는 검색 개체에서 참조되
는 바이너리 파일</td></tr>
<tr><td>관리형 Lookup</td><td>Lookup CSV 파일.<br/>
managed_configurations.conf<br/>
collections.conf<br/>
transforms.conf</td><td>해당 없음</td></tr>
<tr><td>뷰</td><td>뷰에 대한 XML 또는 HTML,CSS 및 JS 파일.</td><td>해당 없음</td></tr>
</table>

## Splunk Enterprise Security에서 Lookup 작성 및 관리

Splunk Enterprise Security는 자산 및 ID의 이벤트와의 상관을 관리하고 위협 지표를 이벤트와 일치시키고 정보를 사용하여 대시보드와 패널을 보강하기 위한 Lookup을 제공함.

관리자는 Splunk Enterprise Security에 Lookup을 추가할 수 있음. Splunk Enterprise Security에 Lookup을 추가한 후에 Lookup을 검색에 사용하고 편집하고 설명을 추가하고 내보낼 수 있음.

### Splunk Enterprise Security에 Lookup 추가

Splunk Enterprise Security에서 Lookup을 업데이트하고 만듬.

1. 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기 > 관리형 Lookup을 클릭함.
3. 새로 만들기를 클릭함.
4. 업로드할 Lookup 파일을 선택함.
5. (선택 사항) 파일의 기본 앱을 변경함.
6. (선택 사항) 파일 이름을 수정함.
7. (선택 사항) 정의 이름을 수정함.
8. (선택 사항) 기본 Lookup 유형을 변경함.
9. Lookup 레이블을 입력. 레이블은 콘텐츠 관리 페이지의 Lookup 이름으로 표시됨.
10. Lookup 설명을 입력.
11. (선택 사항) Lookup 파일의 편집이 가능하도록 옵션을 변경함.
12. 저장을 클릭함.

### Splunk Enterprise Security에 기존 Lookup 추가

Lookup 파일 및 정의가 이미 Splunk 플랫폼에 존재하면 Splunk Enterprise Security에 추가하여 편집할 수 있음.

1. 설정 > 콘텐츠 관리를 선택함.
2. 새 콘텐츠 만들기 > 관리형 Lookup을 클릭함.
3. 기존 항목 선택을 클릭함.
4. 드롭다운 리스트에서 Lookup 정의를 선택함.
5. (선택 사항) Lookup 유형을 수정함.
6. Lookup 레이블을 입력. 레이블은 콘텐츠 관리 페이지의 Lookup 이름으로 표시됨.
7. Lookup 설명을 입력.
8. (선택 사항) Lookup 파일의 편집이 가능하도록 옵션을 변경함.
9. 저장을 클릭함.

### Lookup을 성공적으로 추가했는지 확인

inputlookup 검색 명령어를 사용해 리스트를 표시하여 Lookup 파일이 성공적으로 추가되었는지 확인. 예를 들어, 애플리케이션 프로토콜 Lookup을 검토하려면:

```sql
| inputlookup append=T application_protocol_lookup
```

### Splunk Enterprise Security에서 Lookup 편집

적절한 권한을 보유한 사용자만 Lookup을 편집할 수 있음. Splunk Enterprise Security에서 권한 관리를 참조. Lookup에는 정규식을 사용할 수 없으며 Lookup 편집기는 항목의 정확성을 검사하지 않음. 빈 헤더 필드가 있는 Lookup 파일을 저장할 수 없음.

### Lookup 관리 중지

관리 중지를 클릭하여 콘텐츠 관리 페이지에서 Lookup 관리를 중지할 수 있음. Lookup 관리를 중지하면 더 이상 Splunk Web에서 Lookup을 편집할 수 없지만 Lookup이 삭제되지는 않음.

#### Splunk Enterprise Security에서 Lookup 내보내기

1. 콘텐츠 관리에서 내보낼 Lookup을 찾음.
2. 작업 컬럼 아래에서 내보내기를 클릭하여 파일의 복사본을 CSV 형식으로 내보냄.
    여러 Lookup 파일 및 기타 knowledge object를 앱의 일부로 내보낼 수 있음. Splunk Enterprise Security 관리에서 Splunk Enterprise Security에서 콘텐츠를 앱으로 내보내기를 참조.

### Lookup 파일의 변경 사항 감사(audit)

Lookup 파일을 마지막으로 편집한 시간과 사용자를 확인하려면 검색을 사용. 예:

```sql
index=_internal uri_path="/splunk-es/en-US/app/SplunkEnterpriseSecuritySuite/ess_lookups_edit"
```

## Splunk Enterprise Security에서 내부 Lookup 관리

Splunk Enterprise Security는 대시보드, 검색 및 기타 내부 프로세스를 지원하기 위해 내부 Lookup을 제공하고 유지함.

이런 Lookup은 여러 가지 방법으로 만듬.

- 정적 Lookup 테이블에 의해 채워짐
- 검색 기반 Lookup이라고 하는 검색 명령어에 의해 내부적으로 채워짐
- 인터넷에서 수집한 정보로 채워짐

인터넷에서 수집한 정보로 채워지는 내부 Lookup은 일부 상관(correlation)검색에서 SANS Institute 같은 여러 온라인 소스에 서 악성이거나 수상한 것으로 확인된 호스트를 식별하는데 사용. Splunk Enterprise Security가 인터넷에 연결되어 있지 않은 경우, 이러한 Lookup 파일이 업데이트되지 않고 Lookup에 의존하는 상관(correlation)검색이 올바르게 작동하지 않을 수 있음. 인터넷 정보로 채워지는 내부 Lookup은 대부분 위협 인텔리전스 소스임. 이 매뉴얼에서 Splunk Enterprise Security에 포함된 위협 인텔리전스 소스 설정을 참조.

Splunk Enterprise Security에서 편집할 수 있는 기존 Lookup을 보려면 설정 > 콘텐츠 관리를 선택함.

Splunk Enterprise Security에서는 내부 Lookup을 여러 방법으로 사용.

<table>
<tr><td>Lookup type</td><td>Description</td><td>Example</td></tr>
<tr><td>List</td><td>Small, relatively static lists used to enrich dashboards.</td><td>Categories</td></tr>
<tr><td>Asset or identity list</td><td>Maintained by a modular input and searches. See How Splunk Enterprise Security processes and merges asset and identity data.</td><td>Assets</td></tr>
<tr><td>Threat intelligence collections</td><td>Maintained by several modular inputs. See Threat intelligence framework in Splunk ES on the Splunk developer portal.</td><td>Local Certificate Intel</td></tr>
<tr><td>Tracker</td><td>Search-driven lookups used to supply data to dashboard panels.</td><td>Malware Tracker</td></tr>
<tr><td>Per-panel filter lookup</td><td>Used to maintain a list of per-panel filters on specific dashboards.</td><td>HTTP Category Analysis Filter</td></tr>
</table>

<table>
<tr><td>Lookup 유형</td><td>설명</td><td>예</td></tr>
<tr><td>리스트</td><td>대시보드를 보강하는데 사용되는 비교적 정적인 작은 리스트.</td><td>범주</td></tr>
<tr><td>자산 또는 ID 리스트</td><td>모듈식 입력 및 검색에 의해 유지됨. Splunk Enterprise Security가 자산 및 ID 데이터를 처리 및 병합하는 방법을 참조.</td><td>자산</td></tr>
<tr><td>위협 인텔리전스 컬렉션</td><td>여러 모듈식 입력에 의해 유지됨. Splunk 개발자 포털의 Splunk ES의 위협 인텔리전스 프레임워크를 참조.</td><td>로컬 인증서 인텔리전스</td></tr>
<tr><td>추적기</td><td>대시보드 패널에 데이터를 제공하는데 사용되는 검색 기반 Lookup.</td><td>멀웨어 추적기</td></tr>
<tr><td>패널별 필터 Lookup</td><td>특정 대시보드에서 패널별 필터의 리스트를 유지하는데 사용됨.</td><td>HTTP 범주 분석 필터</td></tr>
</table>

### 수정할 수 있는 내부 Lookup

일부 Lookup은 검색(검색 기반 Lookup)에 의해 관리되며 일부 Lookup은 수동으로 업데이트. 이 테이블에는 Splunk Enterprise Security에서 수정해야 할 수도 있는 Lookup이 나열되어 있음.

<table>
<tr><td>Lookup name</td><td>Type</td><td>Description</td><td>Usage details</td></tr>
<tr><td>Action History Search Tracking Whitelist</td><td>List</td><td>Add searches to this whitelist to prevent them from creating action history items for investigations.</td><td>Type a start_time of 1 to whitelist the search. Type a start_time and an end_time to whitelist the search for a specific period of time.</td></tr>
<tr><td>Administrative Identities</td><td>List</td><td>You can use this lookup to identify privileged or administrative identities on relevant dashboards such as the Access Center and Account Management dashboards.</td><td>Modify the category column to indicate the privileged status of an account. Specify privileged default accounts with default|privileged, or type privileged for privileged accounts that are not default accounts, or default for default accounts that are not privileged.</td></tr>
<tr><td>Application Protocols</td><td>List</td><td>Used by the Port and Protocol dashboard.</td><td>See Application Protocols.</td></tr>
<tr><td>Asset/Identity Categories</td><td>List</td><td>You can use this to set up categories to use to organize an asset or identity. Common categories for assets include compliance and security standards such as PCI or functional categories such as server and web_farm. Common categories for identities include titles and roles.</td><td>See Asset/Identity Categories.</td></tr>
<tr><td>Assets</td><td>Asset list</td><td>You can manually add assets in your environment to this lookup to be included in the asset lookups used for asset correlation.</td><td>See Manually add new asset or identity data.</td></tr>
<tr><td>Demonstration Assets</td><td>Asset list</td><td>Provides sample asset data for demonstrations or examples.</td><td>Disable the lookup for use in production environments. See Disable the demo asset and identity lookups.</td></tr>
<tr><td>Demonstration Identities</td><td>Identity list</td><td>Provides sample identity data for demonstrations or examples.</td><td>Disable the lookup for use in production environments. See Disable the demo asset and identity lookups.</td></tr>
<tr><td>ES Configuration Health Filter</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the ES Configuration Health dashboard.</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>Expected Views</td><td>List</td><td>Lists Enterprise Security views for analysts to monitor regularly.</td><td>See Expected Views.</td></tr>
<tr><td>HTTP Category Analysis Filter</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the HTTP Category Analysis dashboard</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>HTTP User Agent Analysis</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the HTTP User Agent Analysis dashboard</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>Identities</td><td>Identity list</td><td>You can manually edit this lookup to add identities to the identity lookup used for identity correlation.</td><td>See Manually add new asset or identity data.</td></tr>
<tr><td>IIN Lookup</td><td>List</td><td>Static list of Issuer Identification Numbers (IIN) used to identify likely credit card numbers in event data.</td><td>Used to detect Personally-Identifiable Information (PII) in your events.</td></tr>
<tr><td>Interesting Ports</td><td>List</td><td>Used by correlation searches to identify ports that are relevant to your network security policy.</td><td>See Interesting Ports.</td></tr>
<tr><td>Interesting Processes</td><td>List</td><td>Used by a correlation search to identify processes running on hosts relevant to your security policy.</td><td>See Interesting Processes.</td></tr>
<tr><td>Interesting Services</td><td>List</td><td>Used by a correlation search to identify services running on hosts relevant to your security policy.</td><td>See Interesting Services.</td></tr>
<tr><td>Local * Intel</td><td>Threat intelligence lookup</td><td>Used to manually add threat intelligence.</td><td>See Add and maintain threat intelligence locally in Splunk Enterprise Security.</td></tr>
<tr><td>Modular Action Categories</td><td>List</td><td>Used to categorize the types of adaptive response actions available to select.</td><td>Add a custom category to categorize a custom adaptive response action on Incident Review or the correlation search editor.</td></tr>
<tr><td>New Domain Analysis</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the New Domain Analysis dashboard.</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>PCI Domain Lookup</td><td>Identity list</td><td>Used by the Splunk App for PCI Compliance to enrich the pci_domain field. Contains the PCI domains relevant to the PCI standard.</td><td>See Set up asset categories.</td></tr>
<tr><td>Primary Functions</td><td>List</td><td>Identifies the primary process or service running on a host. Used by a correlation search.</td><td>See Primary Functions.</td></tr>
<tr><td>Prohibited Traffic</td><td>List</td><td>Identifies process and service traffic prohibited in your environment. Used by a correlation search.</td><td>See Prohibited Traffic.</td></tr>
<tr><td>Risk Object Types</td><td>List</td><td>The types of risk objects available. Edit the lookup to create a custom risk object type. You can then filter on the new risk object type or add a new risk entry on the Risk Analysis dashboard.</td><td>See Create risk and edit risk objects in Splunk Enterprise Security.</td></tr>
<tr><td>Security Domains</td><td>List</td><td>Lists the security domains that you can use to categorize notable events when created and on Incident Review.</td><td>Edit the lookup and add a custom security domain.</td></tr>
<tr><td>Threat Activity Filter</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the Threat Activity dashboard.</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>Traffic Size Analysis</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the Traffic Size Analysis dashboard.</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
<tr><td>Urgency Levels</td><td>List</td><td>Urgency Levels contains the combinations of priority and severity that dictate the urgency of notable events.</td><td>See How urgency is assigned to notable events in Splunk Enterprise Security in Use Splunk Enterprise Security.</td></tr>
<tr><td>URL Length Analysis</td><td>Per-panel filter lookup</td><td>Per-panel filtering for the URL Length Analysis dashboard.</td><td>See Configure per-panel filtering in Splunk Enterprise Security.</td></tr>
</table>

<table>
<tr><td>Lookup이름</td><td>유형</td><td>설명</td><td>사용법</td></tr>
<tr><td>작업이력 검색 추적 허용 리스트</td><td>리스트</td><td>이 허용 리스트에 검색을 추가하여 조사를 위한 작업이력 항목을 만들 수 없도록 함.</td><td>검색을 허용 리스트에 추가하려면 start_time을 1로 입력. 특정 기간에 대한 검색을 허용 리스트에 추가하려면 start_time 및 end_time을 입력.</td></tr>
<tr><td>관리 ID</td><td>리스트</td><td>이 Lookup을 사용하여 액세스 센터, 계정 관리 대시보드등 관련 대시보드에서 권한 ID 또는 관리 ID를 식별할수 있음.</td><td>계정의 권한 상태를 나타내도록 범주 컬럼을 수정함. default|privileged로 권한이 있는 기본 계정을 지정하거나 기본 계정이 아닌 권한 계정의 경우 privileged, 권한이 없는 기본 계정의 경우 default를 입력.</td></tr>
<tr><td>애플리케이션 프로토콜</td><td>리스트</td><td>포트 및 프로토콜 대시보드에 의해 사용됨.</td><td>애플리케이션 프로토콜을 참조.</td></tr>
<tr><td>자산/ID 범주</td><td>리스트</td><td>자산 또는 ID를 구성하는데 사용할 범주를 설정하기 위해 사용할 수 있음. 일반적으로 선택되는 자산 범주로는 PCI 같은 컴플라이언스 및 보안 표준이나 서버 및 web_farm 같은 기능적 범주 등이 있음. 일반적으로 선택되는 ID 범주로는 직함과 역할 등이 있음.</td><td>자산/ID 범주를 참조.</td></tr>
<tr><td>자산</td><td>자산 리스트</td><td>환경의 자산을 이 Lookup에 수동으로 추가하여 자산 상관(correlation)에 사용될 자산 Lookup에 포함할 수 있음.</td><td>수동으로 새 자산 또는 ID 데이터 추가를 참조.</td></tr>
<tr><td>데모 자산</td><td>자산 리스트</td><td>데모 또는 예시용 샘플 자산 데이터를 제공함.</td><td>프로덕션 환경에 사용하기 위해 Lookup을 비활성화함. 데모 자산 및 ID Lookup 비활성화를 참조.</td></tr>
<tr><td>데모 ID</td><td>ID 리스트</td><td>데모 또는 예시용 샘플 ID 데이터를 제공함.</td><td>프로덕션 환경에 사용하기 위해 Lookup을 비활성화 함. 데모 자산 및 ID Lookup 비활성화를 참조.</td></tr>
<tr><td>ES 설정 상태 필터</td><td>패널별 필터 Lookup</td><td>ES 설정 상태 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>예상된 뷰</td><td>리스트</td><td>애널리스트가 정기적으로 모니터링할 수 있도록 Enterprise Security 뷰를 나열.</td><td>예상된 뷰를 참조.</td></tr>
<tr><td>HTTP 범주 분석 필터</td><td>패널별 필터 Lookup</td><td>HTTP 범주 분석 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>HTTP 사용자 에이전트 분석</td><td>패널별 필터 Lookup</td><td>HTTP 사용자 에이전트 분석 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>ID</td><td>ID 리스트</td><td>이 Lookup을 수동으로 편집하여 ID 상관(correlation)에 사용될 ID Lookup에 ID를 추가할 수 있음.</td><td>수동으로 새 자산 또는 ID 데이터 추가를 참조.</td></tr>
<tr><td>IIN Lookup</td><td>리스트</td><td>이벤트 데이터의 가능한 신용카드 번호를 식별하는데 사용되는 발급자 ID 번호(IIN)의 고정 리스트.</td><td>이벤트에서 개인 식별 정보(PII)를 탐지하는데 사용됨.</td></tr>
<tr><td>관심 포트</td><td>리스트</td><td>상관(correlation)검색에서 네트워크 보안 정책과 관련된 포트를 식별하는데 사용됨</td><td>관심 포트를 참조.</td></tr>
<tr><td>관심 프로세스</td><td>리스트</td><td>상관(correlation)검색에서 보안 정책과 관련된 호스트에서 실행되는 프로세스를 식별하는데 사용됨.</td><td>관심 프로세스를 참조.</td></tr>
<tr><td>관심 서비스</td><td>리스트</td><td>상관(correlation)검색에서 보안 정책과 관련된 호스트에서 실행되는 서비스를 식별하는데 사용됨.</td><td>관심 서비스를 참조.</td></tr>
<tr><td>로컬 *인텔리전스</td><td>위협 인텔리전스 Lookup</td><td>수동으로 위협 인텔리전스를 추가하는데 사용됨.</td><td>Splunk Enterprise Security에서 위협 인텔리전스를 로컬에서 추가 및 유지 관리를 참조.</td></tr>
<tr><td>모듈식 작업 범주</td><td>리스트</td><td>선택 가능한 adaptive response 작업 유형을 범주로 구분하는데 사용됨.</td><td>인시던트 검토 또는 상관(correlation)검색 편집기에서 사용자 지정 adaptive response 작업 유형을 범주로 구분하려면 사용자 지정 범주를 추가함.</td></tr>
<tr><td>새로운 도메인 분석</td><td>패널별 필터 Lookup</td><td>새로운 도메인 분석 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>PCI 도메인 Lookup</td><td>ID 리스트</td><td>Splunk App for PCI Compliance에서 pci_domain 필드를 보강하는데 사용됨. PCI 표준과 관련된 PCI 도메인이 포함됨.</td><td>자산 범주 설정을 참조.</td></tr>
<tr><td>주요 기능</td><td>리스트</td><td>호스트에서 실행되는 주요 프로세스 또는 서비스를 식별함. 상관(correlation)검색에 의해 사용됨.</td><td>주요 기능을 참조.</td></tr>
<tr><td>금지된 트래픽</td><td>리스트</td><td>환경에서 금지된 프로세스 및 서비스 트래픽을 식별함. 상관(correlation)검색에 의해 사용됨.</td><td>금지된 트래픽을 참조.</td></tr>
<tr><td>위험 개체 유형</td><td>리스트</td><td>사용 가능한 위험 개체의 유형.사용자 지정 위험 개체 유형을 만들려면 Lookup을 편집함. 그런 다음 새 위험 개체 유형을 기준으로 필터링하거나 위험 분석 대시보드에서 새 위험항목을 추가함.</td><td>Splunk Enterprise Security에서 위험 작성 및 위험 개체 편집을 참조.</td></tr>
<tr><td>보안 도메인</td><td>리스트</td><td>만들 때 및 인시던트 검토 시 주요 이벤트의 범주를 구분하는데 사용할 수 있는 보안 도메인을 나열.</td><td>Lookup을 편집하고 사용자 지정 보안 도메인을 추가함.</td></tr>
<tr><td>위협 활동 필터</td><td>패널별 필터 Lookup</td><td>위협 활동 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>트래픽 크기 분석</td><td>패널별 필터 Lookup</td><td>트래픽 크기 분석 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
<tr><td>긴급도 레벨</td><td>리스트</td><td>긴급도 레벨에는 주요 이벤트의 긴급도를 나타내는 우선순위와 심각도의 조합이 포함됨.</td><td>Splunk Enterprise Security 사용의 Splunk Enterprise Security에서 긴급도가 주요 이벤트에 배정되는 방법을 참조.</td></tr>
<tr><td>URL 길이 분석</td><td>패널별 필터 Lookup</td><td>URL 길이 분석 대시보드용 패널별 필터링.</td><td>Splunk Enterprise Security에서 패널별 필터링 설정을 참조.</td></tr>
</table>

### 애플리케이션 프로토콜

애플리케이션 프로토콜 리스트는 포트 및 프로토콜 조합과 각 조합의 조직 내 승인 상태가 나열된 리스트임. 이 리스트는 포트 및 프로토콜 추적기 대시보드에서 사용. 포트 및 프로토콜 추적기 대시보드를 참조.

이 파일에서 사용 가능한 필드는 다음과 같음.

|필드|설명|
|:--:|:--|
|dest_port|대상 포트 번호. 0~65535 사이의 숫자여야 함.|
|transport|네트워크 트래픽 프로토콜 예: icmp, tcp, or udp.|
|app|포트를 사용하는 애플리케이션의 이름.|

### 자산/ID 범주

범주 리스트에는 자산 또는 ID를 정리하기 위해 선택하는 범주 집합이 포함될 수 있음. 범주는 자산 및 ID에 사용되는 논리적 분류 또는 그룹임. 일반적으로 선택되는 자산으로는 PCI 같은 컴플라이언스 및 보안 표준이나 서버 및 web_farm같은 기능적 범주 등이 있음. 일반적으로 선택되는 ID로는 직함과 역할 등이 있음. 더 많은 예는 Splunk Enterprise
Security에서 자산 또는 ID 리스트를 Lookup 형식으로 지정을 참조.

자산 및 ID 상관에서 이벤트를 범주 정보로 보강하려면 category 필드를 자산/ID 범주 리스트 대신 자산 및 ID 리스트에 유지해야 함. Splunk Enterprise Security에서 자산 또는 ID 리스트를 Lookup 형식으로 지정을 참조.

두 가지 방법으로 자산/ID 범주 리스트를 유지할 수 있음.

#### 저장된 검색을 실행하여 범주 리스트 유지

Splunk Enterprise Security에는 자산 및 ID 리스트에서 정의된 범주를 자산/ID 범주 리스트에 추가하는 저장된 검색이 포함되어 있음. 검색은 기본적으로 예약되지 않음.

1. Splunk 플랫폼 메뉴 모음에서 설정 > 검색, 보고서, 경고를 선택함.
2. Identity - Make Categories - Lookup Gen 저장된 검색을 찾음.
3. 편집 > 활성화를 클릭함.

#### 수동으로 범주 리스트 유지

범주를 Lookup에 직접 추가하여 범주 리스트를 수동으로 유지함. 기본적으로 리스트를 수동으로 유지해야 함.

1. 설정 > 콘텐츠 관리를 선택함.
2. 자산/ID 범주 리스트를 클릭함.
3. 새 범주를 리스트에 추가함.
4. 저장을 클릭함.

예상된 뷰

예상된 뷰 리스트는 정기적으로 모니터링되는 Splunk Enterprise Security 뷰를 지정함. 이 Lookup은 뷰 감사(audit) 대시보드에서 사용. 대시보드에 대한 자세한 내용은 뷰 감사(audit)를 참조.

아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|
|:--:|:--|
|app|뷰가 포함된 애플리케이션. 일반적으로 SplunkEnterpriseSecuritySuite로 설정됨.|
|is_expected|"true" 또는 "false". 지정하지 않으면 Splunk Enterprise Security는 기본적으로 뷰가 모니터링될 것으로 예상되지 않는다고 가정함.|
|view|뷰의 이름. URL 또는 콘텐츠 관리 대시보드에서 사용할 수 있음.|

뷰의 이름을 찾는 방법:

1. Enterprise Security에서 뷰로 이동함.
2. URL의 마지막 세그먼트를 보고 뷰 이름을 찾음.

예를 들어 다음 URL에 있는 뷰의 이름은 incident_review임.

<https://127.0.0.1:8000/en-US/app/SplunkEnterpriseSecuritySuite/incident_review>

관심 포트

관심 포트에는 배포에서 필수, 금지 또는 비보안으로 확인된 TCP 및 UDP 포트의 리스트가 포함됨. 관리자는 허용되고 허용되지 않는 포트를 정의하는 정책을 설정하고 Lookup을 정책에 맞게 수정할 수 있음. 환경에서 이런 포트가 보일 때 경고를 수신하려면 각 포트에 대한 경고를 트리거하는 Prohibited Port Activity Detected 같은 상관(correlation)검색을 활성화함.

아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|예|
|:--:|:--|:--|
|app|포트를 사용하는 애플리케이션 또는 서비스의 이름.|Win32Time|
|dest|네트워크 서비스의 대상 호스트. 모든 호스트를 일치시키려면 와일드카드 *를 사용.|DARTH*, 10.10.1.100,my_host.|
|dest_pci_domain|선택적인 PCI 도메인. 와일드카드를 사용할 수 있음.|trust, untrust|
|dest_port|대상 포트 번호. 와일드카드를 사용할 수 있음.|443, 3389, 5900|
|transport|전송 프로토콜. 와일드카드를 사용할 수 있음.|tcp 또는 udp|
|is_required|서비스를 실행해야 하며 서비스가 실행되지 않을 경우 상관(correlation)검색을 통해 경고를 만들려면 true로 설정함.|true 또는 false|
|is_prohibited|포트를 네트워크에서 사용하지 않고 서비스가 실행 중일 경우 상관(correlation)검색을 통해 경고를 만들려면 true로 설정함.|true 또는 false|
|is_secure|포트를 통해 전송된 트래픽이 안전한 경우 true로 설정함.|true 또는 false|
|note|포트를 사용하는 서비스 및 포트 정책을 설명.|암호화되지 않은 텔넷 서비스는 안전하지 않음.|

### 관심 프로세스

관심 프로세스에는 프로세스의 리스트 및 프로세스의 필수, 금지 또는 환경에서 실행하기에 안전한지 여부가 포함됨.
Splunk Enterprise Security는 이 리스트를 Prohibited Process Detected 상관(correlation)검색에 사용.

아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|
|:--:|:--|
|app|애플리케이션 이름|
|dest|프로세스의 대상|
|dest_pci_domain|PCI 도메인(사용 가능한 경우)|
|is_required|대상 호스트에서 프로세스를 실행해야 하는 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|is_prohibited|대상 호스트에서 프로세스가 금지된 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|is_secure|프로세스가 안전한 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|note|이 프로세스에 대한 추가 정보를 설명. 예를 들어, 텔넷 애플리케이션은 안전하지 않은 인증으로 인해 금지됨.|

### 관심 서비스

관심 서비스에는 배포의 서비스 리스트가 포함됨. Prohibited Service Detected 상관(correlation)검색에서는 이 Lookup을 사용하여 서비스의 필수, 금지 및/또는 보안 여부를 확인.

아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|
|:--:|:--|
|app|애플리케이션 이름|
|dest|서비스가 실행되는 대상 호스트.|
|dest_pci_domain|호스트의 PCI 도메인(사용 가능한 경우)|
|is_required|호스트에서 서비스를 실행해야 하는 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|is_prohibited|호스트에서 서비스 실행이 금지되는 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|is_secure|서비스가 안전한 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|note|이 서비스에 대한 추가 정보.|

### 주요 기능

주요 기능에는 주 프로세스 및 서비스와 배포에서 각각 수행하는 기능의 리스트가 포함됨. 이 리스트를 사용하여 어떤 서비스가 중요하고 서비스에서 어떤 포트와 트랜스포트를 사용해야 하는지 정의. 이 Lookup은 Multiple Primary
Functions Detected 상관(correlation)검색에 의해 사용.

아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|
|:--:|:--|
|process|프로세스의 이름|
|service|서비스의 이름|
|dest_pci_domain|대상 호스트의 PCI 도메인(사용 가능한 경우)|
|transport|프로세스의 전송에 사용되는 프로토콜. 가능한 값은 tcp 또는 udp임.|
|port|프로세스에 의해 사용되는 포트 번호.|
|is_primary|프로세스가 호스트의 기본 프로세스인 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|function|프로세스가 수행하는 기능. 예: 프록시, 인증, 데이터베이스, 도메인 이름 서비스(DNS), 웹, 또는 메일|

금지된 트래픽

금지된 트래픽은 네트워크 트래픽에 표시될 경우 악의적인 행동을 나타낼 수 있는 프로세스가 나열됨. 이 리스트는 시스템 센터 대시보드에 사용되고 IRC, 데이터 파괴 도구, 파일 전송 소프트웨어, 또는 최근에 창궐한 것으로 의심되는 멀웨어처럼 악성이라고 알려진 소프트웨어 등 보안 정책에 따라 금지되는 소프트웨어를 탐지하는데 유용.
아래 테이블에는 이 파일의 필드가 설명되어 있음.

|필드|설명|
|:--:|:--|
|app|프로세스 이름(echo, chargen 등)|
|is_prohibited|환경에서 프로세스가 금지된 경우 true로 설정함. 가능한 값은 true 또는 false임.|
|note|프로세스가 거부된 이유에 대한 설명을 추가함.|

## Splunk Enterprise Security에서 위험 작성 및 위험 개체 편집

ES 관리자는 위험 개체를 만들고 편집할 수 있음.

### 새 위험 개체 만들기

1. Enterprise Security 메뉴에서 설정 > 데이터 보강 > 리스트 및 Lookup을 선택하고 위험 개체 유형 리스트를 선택함.
2. 테이블에서 risk_object_type 셀을 강조 표시하고 마우스 오른쪽 단추를 클릭하여 테이블 편집기를 표시.
3. 새 행을 테이블에 삽입함.
4. 새 행을 두 번 클릭하여 편집한 다음 새 개체 유형 이름을 추가함.
5. 변경 사항을 저장함.

### 기존 위험 개체 편집

1. Enterprise Security 메뉴에서 설정 > 데이터 보강 > 리스트 및 Lookup을 선택함.
2. 위험 개체 유형리스트를 선택함.
3. 위험 개체 유형을 강조 표시하고 이름을 변경함.
4. 변경 사항을 저장함.