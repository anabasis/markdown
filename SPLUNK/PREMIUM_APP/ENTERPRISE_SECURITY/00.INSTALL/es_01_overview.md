
# 소개

## Splunk Enterprise Security 소개

Splunk Enterprise Security는 Splunk 플랫폼의 검색 및 보고 기능을 사용하여 조직의 보안 포스처를 전체적으로 볼 수 있는 뷰를 보안 실무자에게 제공합니다. Enterprise Security는 상관(correlation)검색을 사용하여 보안 관련 위협을 볼 수 있는 가시성을 부여하고, 확인된 위협을 추적하기 위한 주요 이벤트를 생성합니다. 환경 전체의 장치, 시스템 및 애플리케이션에서 데이터를 캡처하고, 모니터링하고, 보고할 수 있습니다.

이 매뉴얼은 Splunk 소프트웨어를 설치 및 설정하고 관리할 수 있는 사용자를 대상으로 작성되었습니다. Splunk 플랫폼과 Enterprise Security에 대한 교육이 필요한 경우 Enterprise Security 고객 교육 과정을 참조하십시오.

기타 Splunk Enterprise Security 매뉴얼:

- 릴리스 노트
- Splunk Enterprise Security 사용
- Splunk Enterprise Security 관리
- 이용 사례
- REST API 조회

## Splunk Enterprise Security의 공유 데이터

Splunk Enterprise Security를 Splunk Enterprise에 배포하면 Splunk 플랫폼에서 익명 사용 데이터를 Splunk Inc.(이하 "Splunk")로 전송해 향후 릴리스의 Splunk Enterprise Security를 개선할 수 있게 합니다. 동의 또는 동의 철회 방법과 데이터 수집, 저장, 관리 방법에 관한 자세한 정보는 Splunk Enterprise의 공유 데이터를 참조하십시오.

### 데이터 수집 방법

Splunk Enterprise Security는 저장된 검색을 이용하여 익명 사용 데이터를 수집합니다. 이러한 검색은 Splunk로의 사용 데이터 전송 동의 여부에 관계없이 백그라운드에서 실행되며 성능에는 큰 영향이 없습니다.

### 수집되는 데이터

Splunk Enterprise Security는 다음의 기본 사용 정보를 수집합니다.

<table>
<tr><td>이름</td><td>설명</td><td>예</td></tr>
<tr><td>app.SplunkEnterpriseSecuritySuite.active_users</td><td>활성 사용자 수 보고</td><td>
{
"version": "1.0",
"end": 1521483766,
"begin": 1521396000,
"data": {
"analyst_count": 0,
"count": 1,
"admin_count": 1,
"user_count": 0
}
}</td></tr>
<tr><td>app.SplunkEnterpriseSecuritySuite.feature_usage</td><td>페이지 로
드 시간: 페이지를 로드하는데 걸린 시간의 양 보고 기능 사용: 기능 사용 관련 데이터 보고</td><td>
{
"end": 1521483766,
"begin": 1521396000,
"version": "1.0",
"data": {
"count": 1,
"avg_spent": 515,
"view": "ess_home"
}
}</td></tr>
<tr><td>app.SplunkEnterpriseSecuritySuite.search_execution</td><td>
보고는 성능을
측정하는 데 도
움이 되도록 검
색별 실행 시간
의 평균을 산출
합니다.</td><td>
{
"end": 1521483766,
"begin": 1521396000,
"data": {
"avg_run_time": 0.75,
"count": 2,
"search_alias": "Access -
Authentication Tracker - Lookup Gen"
},
"version": "1.0",
}</td></tr>
<tr><td>app.SplunkEnterpriseSecuritySuite.search_actions</td><td>검색된 내용 보
고</td><td>
{
"data": {
"total_scheduled": 70,
"action": "output_message",
"is_adaptive_response": 1,
"count": 6
},
"version": "1.0"
}</td></tr>
<tr><td>app.SplunkEnterpriseSecuritySuite.datamodel_distribution</td><td>
어떤 모델이 가
장 많이 사용됐
는지 확인하기
위해 데이터 모
델 감사(audit)
를 수행합니다.</td><td>
{
"data": {
"size": 2265088,
"datamodel": "Change_Analysis",
"perc": 49.33
},
"version": "1.0"
}</td></tr>
</table>