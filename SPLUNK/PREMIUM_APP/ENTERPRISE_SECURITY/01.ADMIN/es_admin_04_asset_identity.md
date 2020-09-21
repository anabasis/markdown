# Asset 및 Identity

## Splunk Enterprise Security에 Asset 및 Identity 데이터 추가

Splunk Enterprise Security는 Asset 및 Identity 시스템을 사용해 Asset 및 Identity 정보를 이벤트와 연결하여 데이터를 보강하고 데이터에 컨텍스트를 제공. 이 시스템은 외부 데이터 원본에서 정보를 가져와서 Lookup을 채우고, Enterprise Security는 이 Lookup을 검색 시간에 이벤트와 연결.
Asset 및 Identity 데이터를 Splunk Enterprise Security에 추가하여 Asset 및 Identity 상관을 이용.

1. Splunk Enterprise Security에서 Asset 및 Identity 데이터를 수집하고 추출.
2. (선택 사항) Splunk Enterprise Security의 ID 설정 페이지에서 ID 형식을 정의.
3. Splunk Enterprise Security에서 Asset 및 Identity 리스트를 Lookup 형식으로 지정.
4. Splunk Enterprise Security에서 새 Asset 및 Identity 리스트를 설정.
5. Asset 및 Identity 데이터가 Splunk Enterprise Security에 추가되었는지 확인.
6. Splunk Enterprise Security에서 Asset 및 Identity 상관을 설정.

참고 항목은 아래와 같음.

Splunk Enterprise Security가 Asset 및 Identity 데이터를 상관, 처리, 병합하는 방법
병합된 Asset 및 Identity 데이터를 저장하는 Lookup

## Splunk Enterprise Security에서 Asset 및 Identity 데이터 수집 및 추출

Asset 및 Identity 데이터를 Splunk Enterprise Security에 추가하기 위해 수집 및 추출할 수 있음. Splunk Cloud 배포에서는 Splunk 프로페셔널 서비스와 협력하여 Asset 및 Identity 컬렉션 솔루션을 설계 및 구현함. Asset 및 Identity 데이터를 추가하는 예는 Asset 및 Identity 데이터를 Splunk Enterprise Security에 추가하는 방법의 예를 참조.

1. 환경에서 Asset 및 Identity 데이터가 저장되어 있는 위치를 확인.
2. Asset 및 Identity 데이터를 자동으로 수집 및 업데이트하여 수동 업데이트에 필요한 경비와 유지관리 작업을 줄이고 데이터 무결성을 개선함.
    - Splunk DB Connect 또는 기타 Splunk 플랫폼 추가 기능을 사용하여 외부 데이터베이스 또는 리포지토리에 연결.
    - 스크립트 기반 입력을 사용하여 리스트를 가져오고 형식을 지정.
    - Splunk 플랫폼의 인덱싱된 이벤트를 검색과 함께 사용하여 데이터를 수집 및 정렬하고 리스트로 내보냄.

권장되는 Asset 및 Identity 수집 방법

|기술|Asset or Identity data|수집 방법|
|:--:|:--:|:--|
|Active Directory|두 가지 모두|SA-ldapsearch와 사용자 지정 검색|
|LDAP|두 가지 모두|SA-ldapsearch와 사용자 지정 검색|
|CMDB|자산|DB Connect와 사용자 지정 검색|
|ServiceNow|두 가지 모두|ServiceNow용 Splunk 추가 기능|
|Asset Discovery|자산|Asset Discovery 앱|
|Bit9|자산|Bit9용 Splunk 추가 기능과 사용자 지정 검색|
|Cisco ISE|두 가지 모두|Cisco ISE용 Splunk 추가 기능과 사용자 지정 검색|
|Microsoft SCOM|자산|Microsoft SCOM용 Splunk 추가 기능과 사용자 지정 검색|
|Okta|ID|Okta용 Splunk 추가 기능과 사용자 지정 검색|
|Sophos|자산|Sophos용 Splunk 추가 기능과 사용자 지정 검색|
|Symantec Endpoint Protection|자산|Symantec Endpoint Protection용 Splunk 추가 기능과 사용자 지정 검색|
|Splunk 플랫폼|자산|Splunk 플랫폼에서 인덱싱된 이벤트로부터 자산 데이터를 추가|
|Amazon Web Services (AWS)|Asset|SecKit AWS Add On for ES Asset and Identities|
|Configuration Management Database (CMDB)|Asset|SecKit SA Common Tools for ES Asset and Identities|

## Splunk Enterprise Security에서 ID 형식 정의

ID Lookup 설정 페이지의 환경에서 사용자를 식별하는 ID 형식을 정의. ID Lookup 설정 페이지에서 변경 사항을 적용하면 identityLookup.conf 파일이 수정.

전제 조건

Splunk Enterprise Security에서 Asset 및 Identity 데이터 수집 및 추출

절차

1. Splunk ES 메뉴 모음에서 설정 > 데이터 보강 > ID Lookup 설정을 선택.
2. (선택 사항) 환경에서 이메일 주소로 사용자가 식별되지 않는 경우 이메일 체크박스를 선택 해제.
3. (선택 사항) 이메일 주소의 사용자 이름이 환경에서 사용자를 식별하지 않는 경우 짧은 이메일 체크박스를 선택 해제.
4. (선택 사항) 사용자를 식별하는데 사용할 사용자 지정 규칙을 정의하려면 규칙 체크박스를 선택. 새 규칙 추가를 클릭하여 사용자 지정 규칙을 추가.
    예를 들어 first(3)last(3) 규칙을 사용하여 이름과 성의 첫 세 글자로 사용자를 식별할 수 있음.
5. (선택 사항) ID의 대소문자가 일치해야 한다고 요구하려면 대소문자 구분 체크박스를 선택. 대소문자를 구별하여 일치하는 ID를 검색하면 일치하는 결과가 더 적게 생성.
6. 저장을 클릭.

## Splunk Enterprise Security에서 Asset 및 Identity 리스트를 Lookup 형식으로 지정

수집된 Asset 및 Identity 데이터를 Splunk Enterprise Security에서 처리할 수 있게 Lookup 파일 형식으로 지정할 수 있음.

전제 조건

- Splunk Enterprise Security에서 사용할 Asset 및 Identity 데이터 수집 및 추출
- (선택 사항) Splunk Enterprise Security에서 ID 형식 정의

절차

1. Unix 줄 마감과 .csv 파일 확장자가 있는 일반 텍스트, CSV 형식 파일을 만듬.
2. CSV 파일에 올바른 헤더를 사용. Splunk Enterprise Security에서 기대하는 헤더는 Asset Lookup 헤더 또는 ID Lookup 헤더를 참조.
3. CSV 행을 Asset 및 Identity 필드로 채움. Asset Lookup 필드 또는 ID Lookup 필드를 참조.

자산 리스트의 예는 데모 Asset Lookup을 참조.

- 설정 > 데이터 보강 > 리스트 및 Lookup으로 이동하여 Splunk Web에서 리스트를 찾음.
- 파일 시스템에서 리스트를 찾음. demo_assets.csv 파일은 SA-IdentityManagement/package/lookups에 있음.

사용자 지정 검색을 사용하여 Lookup을 생성하는 경우, 검색 결과에 의해 생성되는 Lookup에 헤더와 일치하는 필드가 있는지 확인.

### Asset Lookup 헤더

ip,mac,nt_host,dns,owner,priority,lat,long,city,country,bunit,category,pci_domain,is_expected,should_timesync,should_update,requires_av

### Asset Lookup 필드

Asset Lookup의 다음 필드를 채움.

멀티홈 지원은 제한적이며, 서로 다른 네트워크에 IP 주소가 같은 여러 호스트가 있으면 병합 프로세스에서 충돌이 발생할 수 있음.

<table>
    <tr><td>필드</td><td>데이터유형</td><td>설명</td><td>예제 값</td></tr>
    <tr><td>ip<td></td>파이프로구분된 숫자</td><td>단일 IP 주소나 IP 범위의 파이프로 구분된 리스트임. 자산의 ip,mac, nt_host 또는 dns 필드에 항목이 있어야 함. 자산별로 2개 이상의 필드에 파이프 구분을 사용하지 않음.</td><td>2.0.0.0/8|1.2.3.4|192.168.15.9-192.169.15.27|5.6.7.8|10.11.12.13</td></tr>
    <tr><td>mac</td><td>파이프로구분된 문자열</td><td>MAC 주소의 파이프로 구분된 리스트임. 자산의 ip, mac, nt_host 또는 dns 필드에 항목이 있어야 함. 자산별로 2개 이상의 필드에 파이프 구분을 사용하지 않음.</td><td>00:25:bc:42:f4:60|00:50:ef:84:f1:21|00:50:ef:84:f1:20</td></tr>
    <tr><td>nt_host</td><td>파이프로구분된 문자열</td><td>Windows 컴퓨터 이름의 파이프로 구분된 리스트임. 자산의 ip,mac, nt_host 또는 dns 필드에 항목이 있어야 함. 자산별로 2개 이상의 필드에 파이프 구분을 사용하지않음.</td><td>    ACME-0005|SSPROCKETS-0102|COSWCOGS-013</td></tr>
    <tr><td>dns</td><td>파이프로구분된 문자열</td><td>DNS 이름의 파이프로 구분된 리스트임. 자산의 ip, mac, nt_host 또는 dns 필드에 항목이 있어야 함. 자산별로 2개 이상의 필드에 파이프 구분을 사용하지 않음.</td><td>acme-0005.corp1.acmetech.org|SSPROCKETS-0102.spsp.com|COSWCOGS-013.cwcogs.com</td></tr>
    <tr><td>owner</td><td>문자열</td><td>장치와 연관된 사용자 또는 부서</td><td>f.prefect@acmetech.org, DevOps, Bill</td></tr>
    <tr><td>priority</td><td>문자열</td><td>권장. 인시던트 검토에서 주요 이벤트의 긴급도 필드를 계산하기 위해장치에 배정된 우선 순위임. 우선 순위가 "알 수 없음"인 경우 배정되는 긴급도가 기본적으로 낮아짐. 자세한 내용은 SplunkEnterprise Security에서 긴급도가 주요 이벤트에 배정되는 방법을 참조.</td><td>알 수 없음, 낮음, 중간, 높음 또는 중요.</td></tr>
    <tr><td>lat</td><td>문자열</td><td>자산의 위도</td><td>41.040855</td></tr>
    <tr><td>long</td><td>문자열</td><td>자산의 경도</td><td>28.986183</td></tr>
    <tr><td>city</td><td>문자열</td><td>자산이 위치한 도시</td><td>Chicago</td></tr>
    <tr><td>국가</td><td>문자열</td><td>자산이 위치한 국가</td><td>USA</td></tr>
    <tr><td>bunit</td><td>문자열</td><td>권장. 자산의 사업 부문. Splunk Enterprise Security에서 대시보드기준 필터링에 사용.</td><td>EMEA, NorCal</td></tr>
    <tr><td>category</td><td>파이프로구분된 문자열</td><td>권장. 자산의 논리적 분류를 파이프로 구분한 리스트임. Asset 및 Identity상관 및 분류에 사용. 범주를 참조.</td><td>server|web_farm|cloud</td></tr>
    <tr><td>pci_domain</td><td>파이프로구분된 문자열</td><td>PCI 도메인의 파이프로 구분된 리스트임. 설치 및 설정 매뉴얼에서 Splunk App for PCI Compliance의 자산 설정을 참조.</td><td>cardholder, trust|dmz, untrust 비워 둘 경우 untrust기 기본값으로 지정됨.</td></tr>
    <tr><td>is_expected</td><td>부울</td><td>이 자산의 이벤트를 항상 기대해야 하는지 나타냄. True로 설정할 경우, 이 자산이 이벤트 보고를 중지하면 Expected Host Not Reporting 상관(correlation)검색이 adaptive     response 작업을 수행함.</td><td>"true" 또는 "false"를 나타내는 공백</td></tr>
    <tr><td>should_timesync</td><td>부울</td><td>이 자산에서 시간 동기화 이벤트를 모니터링해야 하는지 여부를 나타냄. True로 설정할 경우, 이 자산이 지난 24시간의 시간 동기화 이벤트를 보고하지 않으면 Should Timesync Host Not Syncing 상관 (correlation) 검색이 adaptive response 작업을 수행함.</td><td>"true" 또는 "false"를 나타내는 공백</td></tr>
    <tr><td>should_update</td><td>부울</td><td>이 자산에서 시스템 업데이트 이벤트를 모니터링해야 하는지 여부를 나타냄.</td><td>"true" 또는 "false"를 나타내는 공백</td></tr>
    <tr><td>requires_av</td><td>부울</td><td>이 자산에 바이러스 백신 소프트웨어가 설치되어 있어야 하는지 여부를 나타냄.</td><td>"true" 또는 "false"를 나타내는 공백</td></tr>
</table>

### ID Lookup 헤더

identity,prefix,nick,first,last,suffix,email,phone,phone2,managedBy,priority,bunit,category,watchlist,startDate,endDate,work_city,work_country

### ID Lookup 필드

<table>
    <tr width=10%><td width=10%>필드</td><td width=50%>데이터유형</td><td>설명</td><td width=30%>예제 값</td></tr>
    <tr><td>ID</td><td>파이프로구분된 문자열</td><td>필수 사항. ID를 나타내는 사용자 이름 문자열을 파이프로 구분한 리스트임.병합 프로세스가 완료된 후 이 필드에 ID Lookup 구성 설정에 따라 생성된 값이 포함됨.</td><td>a.vanhelsing|abraham.vanhelsing|a.vanhelsing@acmetech.org</td></tr>
    <tr><td>prefix</td><td>문자열</td><td>ID의 접두사.</td><td>Ms., Mr.</td></tr>
    <tr><td>nick</td><td>문자열</td><td>ID의 닉네임.</td><td>Van Helsing</td></tr>
    <tr><td>first</td><td>문자열</td><td>ID의 이름.</td><td>Abraham</td></tr>
    <tr><td>last</td><td>문자열</td><td>ID의 성.</td><td>Van Helsing</td></tr>
    <tr><td>suffix</td><td>문자열</td><td>ID의 접미사.</td><td>M.D., Ph.D</td></tr>
    <tr><td>email</td><td>문자열</td><td>ID의 이메일 주소.</td><td>a.vanhelsing@acmetech.org</td></tr>
    <tr><td>phone</td><td>문자열</td><td>ID의 전화 번호.</td><td>123-456-7890</td></tr>
    <tr><td>phone2</td><td>문자열</td><td>ID의 보조 전화 번호.</td><td>012-345-6789</td></tr>
    <tr><td>managedBy</td><td>문자열</td><td>ID 관리자를 나타내는 사용자 이름.</td><td>phb@acmetech.org</td></tr>
    <tr><td>priority</td><td>문자열</td><td>권장. 인시던트 검토에서 주요 이벤트의 긴급도 필드를 계산하기 위해 ID에 배정된 우선 순위임. 우선 순위가 "알 수 없음"인 경우 배정되는 긴급도가 기본적으로 낮아짐. 자세한 내용은 Splunk Enterprise Security에서 긴급도가 주요 이벤트에 배정되는 방법을 참조.</td><td>알 수 없음, 낮음, 중간, 높음 또는 중요.</td></tr>
    <tr><td>bunit</td><td>문자열</td><td>권장. ID의 그룹 또는 부서분류 Splunk Enterprise Security에서 대시보드 기준 필터링에 사용.</td><td>Field Reps, ITS, Products, HR</td></tr>
    <tr><td>category</td><td>파이프로구분된 문자열</td><td>권장. ID의 논리적 분류를 파이프로 구분한 리스트임. Asset 및 Identity 상관 및 분류에 사용. 범주를 참조.</td><td>Privileged|Officer|CISO</td></tr>
    <tr><td>watchlist</td><td>부울</td><td>작업 모니터링에 사용되는 ID를 표시.</td><td>허용 값: "true" 또는 비워 둠. 이 매뉴얼에서 사용자 작업 모니터링을 참조.</td></tr>
    <tr><td>startDate</td><td>문자열</td><td>ID의 시작 또는 채용 날짜</td><td>형식: %m/%d/%Y %H:%M, %m/%d/%y %H:%M, %s</td></tr>
    <tr><td>endDate</td><td>문자열</td><td>ID의 종료 또는 만료 날짜</td><td>형식: %m/%d/%Y %H:%M, %m/%d/%y %H:%M, %s</td></tr>
    <tr><td>work_city</td><td>문자열</td><td>ID의 주 작업장 도시</td><td></td></tr>
    <tr><td>work_country</td><td>문자열</td><td>ID의 주 작업장 국가</td><td></td></tr>
    <tr><td>work_lat</td><td>문자열</td><td>DD와 나침반 방향 형식으로 나타낸 주 작업장 도시의 위도</td><td>37.78N</td></tr>
    <tr><td>work_long</td><td>문자열</td><td>DD와 나침반 방향 형식으로 나타낸 주 작업장 도시의 경도</td><td>122.41W</td></tr>
</table>

## Splunk Enterprise Security에서 새 Asset 및 Identity 리스트 설정

Splunk Enterprise Security에서 새 Asset 및 Identity Lookup을 설정. 이 다단계 프로세스에서는 Splunk Enterprise Security에 Lookup을 추가하고 병합 프로세스에 사용할 Lookup을 정의.

전제 조건 Splunk Enterprise Security에서 Asset 및 Identity 리스트를 Lookup으로 지정.

절차

1. 새 Lookup 테이블 파일 추가
2. Splunk Enterprise Security와 공유하도록 Lookup 테이블 파일에 대한 권한 설정
3. 새 Lookup 정의 추가
4. Splunk Enterprise Security와 공유하도록 Lookup 정의에 대한 권한 설정
5. Lookup 원본에 대한 입력 스탠자 추가
6. (선택 사항) 병합 적용

### 새 Lookup 테이블 파일 추가

1. Splunk 메뉴 모음에서 설정 > Lookup > Lookup 테이블 파일을 선택.
2. 새로 만들기를 클릭.
3. SA-IdentityManagement의 대상 앱을 선택.
4. 업로드할 Lookup 파일을 선택.
5. Lookup 테이블 파일의 검색 헤드에 있어야 하는 대상 파일 이름을 입력. 이름에 파일 이름 확장자가 포함되어야 함.
    예: network_assets_from_CMDB.csv
6. 저장을 클릭하여 Lookup 테이블 파일을 저장하고 Lookup 테이블 파일 리스트로 돌아감.

### Splunk Enterprise Security와 공유하도록 Lookup 테이블 파일에 대한 권한 설정

1. Lookup 테이블 파일에서 새 Lookup 테이블 파일을 찾고 권한을 선택.
2. 개체 표시 위치를모든 앱으로 설정.
3. 읽기 권한을 모든 사용자로 설정.
4. 쓰기 권한을 admin 또는 기타 역할로 설정.
5. 저장을 클릭.

### 새 Lookup 정의 추가

1. Splunk 메뉴 모음에서 설정 > Lookup > Lookup 정의를 선택.
2. 새로 만들기를 클릭.
3. SA-IdentityManagement의 대상 앱을 선택.
4. Lookup 원본의 이름을 입력. 이 이름은 나중에 ID 관리 대시보드의 입력 스탠자 정의에서 정의하는 이름과 일치해야 함.
    예: network_assets_from_CMDB.
5. 유형으로 파일 기반을 선택.
6. 만든 Lookup 테이블 파일을 선택.
    예를 들어 network_assets_from_CMDB.csv를 선택.
7. 저장을 클릭.

### Splunk Enterprise Security와 공유하도록 Lookup 정의에 대한 권한 설정

1. Lookup 정의에서 새 Lookup 정의를 찾고 권한을 선택.
2. 개체 표시 위치를모든 앱으로 설정.
3. 읽기 권한을 모든 사용자로 설정.
4. 쓰기 권한을 admin 또는 기타 역할로 설정.
5. 저장을 클릭.

### Lookup 원본에 대한 입력 스탠자 추가

1. Splunk Enterprise Security로 돌아감.
2. Splunk ES 메뉴 모음에서 설정 > 데이터 보강 > ID 관리를 선택.
3. 새로 만들기를 클릭.
4. Lookup의 이름을 입력.
    예: network_assets_from_CMDB.
5. 새 Asset 및 Identity 리스트를 설명하는 범주를 입력.
    예: CMDB_network_assets.
6. 리스트의 내용에 대한 설명을 입력.
    예: CMDB의 네트워크 자산.
7. Asset 및 Identity를 입력하여 리스트 유형을 정의.
8. Lookup 정의 이름을 참조하는 원본을 입력.
      예: lookup://network_assets_from_CMDB.
9. 저장을 클릭.
10. 5분간 기다림. Splunk Enterprise Security가 5분마다 Asset 및 Identity 리스트를 저장된 검색과 병합함. 이 프로세스에 대한 설명은 Splunk Enterprise Security가 Asset 및 Identity 데이터를 처리하고 병합하는 방법을 참조.

병합 적용

주 저장된 검색을 직접 실행하여 예약된 검색이 실행될 때까지 5분 동안 기다리지 않고 즉시 병합을 적용할 수도 있음.

1. 검색 페이지를 염.
2. 주 저장된 검색을 실행.
    ```sql
   | from savedsearch:"Identity - Asset String Matches - Lookup Gen"
   | from savedsearch:"Identity - Asset CIDR Matches - Lookup Gen"
   | from savedsearch:"Identity - Identity Matches - Lookup Gen"
    ```

### Asset 및 Identity 데이터가 Splunk Enterprise Security에 추가되었는지 확인

대시보드를 검색하고 확인하여 Asset 및 Identity 데이터가 Splunk Enterprise Security에 추가되었는지 확인.

전제 조건

Splunk Enterprise Security에서 새 Asset 및 Identity 리스트 설정

절차
Asset Lookup 데이터를 확인.

1. 특정 자산 레코드가 Asset Lookup에 있는지 확인.
    1. ip, mac, nt_host 또는 dns 필드에 데이터가 있는 자산 레코드를 자산 리스트에서 선택.
    2. 선택한 레코드를 Splunk Web에서 검색함.
    ```sql
    | makeresults | eval src="1.2.3.4" | `get_asset(src)`
    ```

- 다음 방법 중 하나를 사용하여 인스턴스의 사용 가능한 자산을 모두 봄. 행 수를 자산 데이터 원본와 비교하여 자산 레코드 수가 예상한 수와 일치하는지 확인하거나, 특정 레코드를 임의로 조사.
      - 자산 센터 대시보드를 봄. Splunk Enterprise Security 사용에서 자산 센터 대시보드를 참조.
      - 자산 매크로를 사용.
      ```sql
      | `assets`
      ```
      - 데이터 모델을 검색함.
      ```sql
      |`datamodel("Identity_Management", "All_Assets")` |`drop_dm_object_name("All_Assets")`
      ```

ID Lookup 데이터를 확인.

1. 특정 ID 레코드가 ID Lookup에 있는지 확인.
    1. identity 필드에 데이터가 있는 ID 레코드를 선택.
    2. 선택한 레코드를 Splunk Web에서 검색함.
    ```sql
    | makeresults | eval user="VanHelsing" | `get_identity4events(user)`
    ```

다음 방법 중 하나를 사용하여 인스턴스의 사용 가능한 ID를 모두 봄. 행 수를 ID 데이터 원본와 비교하여 ID 레코드 수가 예상한 수와 일치하는지 확인하거나, 특정 레코드를 임의로 조사.

- ID 센터 대시보드를 봄. Splunk Enterprise Security 사용에서 ID 센터 대시보드를 참조.
- ID 매크로를 사용.
    ```sql
    | `identities`
    ```
- 데이터 모델을 검색함.
    ```sql
    |`datamodel("Identity_Management", "All_Identities")` |`drop_dm_object_name("All_Identities")`
    ```

## Splunk Enterprise Security에서 Asset 및 Identity 상관 설정

Asset 및 Identity 데이터를 Splunk Enterprise Security에 추가한 후 Splunk Enterprise Security에서 Asset 및 Identity 상관을 설정.

전제 조건

Asset 및 Identity 데이터가 Splunk Enterprise Security에 추가되었는지 확인

절차

1. Asset 및 Identity 상관을 활성화할지, 비활성화할지, 아니면 선택된 source type에 대해서만 상관이 수행되도록 제한할지 선택. 확실히 모를 경우 Asset 및 Identity 상관이 활성화된 상태로 유지함. 상관을 통해 검색 시간에 이벤트가 보강되는 방법에 대한 자세한 내용은 Asset 및 Identity 상관 작동 방식을 참조.
2. Splunk ES 메뉴 모음에서 설정 > 데이터 보강 > ID 상관을 선택.
3. 기본적으로 상관 활성화가 선택됨. 이 설정을 상관 비활성화(권장되지 않음) 또는 sourcetype에 따라 선택적으로 활성화로 변경할 수 있음.
4. sourcetype에 따라 선택적으로 활성화를 선택하는 경우, source type을 입력하고 Asset 및 Identity에 해당하는 체크박스를 선택.
5. 저장을 클릭.

Asset 및 Identity 상관을 완전히 비활성화하면 Asset 및 Identity Lookup에서 얻은 Asset 및 Identity 데이터로 이벤트를 보강할 수 없음. 그러면 상관(correlation)검색, 대시보드 및 기타 기능이 정상적으로 작동하지 않을 수 있음. Asset 및 Identity 상관을 비활성화하기 전에 Splunk 프로페셔널 서비스 또는 Splunk 서포트와 상의함.

### Asset 및 Identity 상관 작동 방식

조직이 보안 침입을 효과적으로 감지하려면 로그 데이터의 이벤트를 침입의 원인이 되거나 침입에 영향을 받은 특정 Asset 및 Identity와 상관할 수 있어야 함. Asset 및 Identity 상관이 활성화된 경우, Splunk Enterprise Security는 인덱싱된 이벤트를 자산 및
ID 리스트의 Asset 및 Identity 데이터와 비교하여 데이터 보강과 컨텍스트를 제공. 비교 프로세스에서는 자동 Lookup을 사용. 자동 Lookup에 대한 내용은 Splunk 플랫폼 매뉴얼에서 확인할 수 있음.

- Splunk Enterprise의 경우 Splunk Enterprise 지식 관리자 매뉴얼에서 Lookup 자동화를 참조.
- Splunk Cloud의 경우 Splunk Cloud 지식 관리자 매뉴얼에서 Lookup 자동화를 참조.

Asset 및 Identity 상관은 검색 시간에 이벤트를 Asset 및 Identity 데이터로 보강함.

- 자산 상관에서는 src, dest 또는 dvc 필드에 데이터가 있는 이벤트를 병합된 자산 리스트와 비교하여 일치하는 IP 주소,MAC 주소, DNS 이름 또는 Windows NetBIOS 이름을 찾음. host 또는 orig_host 필드에 대해서는 더 이상 자산 상관이 자동으로 수행되지 않음.
- ID 상관에서는 user 또는 src_user 필드에 데이터가 있는 이벤트를 병합된 ID 리스트와 비교하여 일치하는 사용자 또는 세션을 찾음.
- Enterprise Security는 일치하는 출력 필드를 이벤트에 추가. 예를 들어 자산 src 필드에 대해 상관을 수행하면 src_is_expected 및 src_should_timesync 같은 추가 필드가 생성.

Asset 및 Identity 상관을 사용하면 여러 이벤트가 동일한 Asset 및 Identity와 연관될 수 있는지 결정할 수 있음. 이벤트에 추가된 Asset 및 Identity 필드에 대해 작업을 수행하여 특정 Asset 및 Identity를 범위로 하는 추가 검색 또는 대시보드를 열 수도 있음. 예를 들어 src 필드에 대해 Asset Investigator 대시보드를 열 수 있음.

## Splunk Enterprise Security가 Asset 및 Identity 데이터를 처리 및 병합하는 방법

Splunk Enterprise Security는 사용자가 추가하는 Asset 및 Identity 데이터를 Lookup으로 사용하여 결합 Lookup 파일을 생성.
Splunk Enterprise Security는 생성된 Lookup 파일을 사용해 Asset 및 Identity 데이터를 자동 Lookup을 사용하여 이벤트와 상관함.
아래 절차에서는 이 프로세스를 개괄적으로 설명.

1. Asset 및 Identity 데이터를 추가 기능과 사용자 지정 검색을 사용하여 데이터 원본에서 수집하거나 CSV 파일을 사용하여 수동으로 수집할 수 있음. Asset 및 Identity 데이터 수집 및 추출을 참조.
2. ID Lookup 구성 설정에서 설정을 구성함. ID 설정 페이지에서 ID 형식 정의를 참조.
3. Splunk Enterprise Security ID 관리자 모듈식 입력은 transforms.conf 스탠자 identity_lookup_expanded의 설정을 업데이트.
4. 데이터 형식을 검색을 사용하여 Lookup으로 지정하거나 CSV 파일을 사용하여 수동으로 지정. Asset 및 Identity 리스트를 Lookup 형식으로 지정을 참조.
5. 리스트를 Lookup 테이블, 정의 및 입력으로 설정. 새 Asset 및 Identity 리스트 설정을 참조.
6. Splunk Enterprise Security ID 관리자 모듈식 입력은 다음 두 가지 사항을 감지함. identity_manager://<input_name>에서 변경된 콘텐츠 입력의 스탠자에 적용된 변경 사항
7. Splunk Enterprise Security ID 관리자 모듈식 입력은 inputs.conf에서 현재 활성화되어 있는 스탠자에 따라 입력 원본을 식별하는데 사용되는 매크로를 업데이트. 예를 들어 `generate_identities` 매크로는 ID Lookup 설정 페이지에서 지정된 규칙에 따라 동적으로 업데이트.
8. Splunk Enterprise Security ID 관리자 모듈식 입력은 Asset 및 Identity 리스트를 병합해야 하는 변경 사항이 확인되는 경우 Lookup을 생성하는 저장된 검색을 처리.
9. Lookup을 생성하는 저장된 검색은 설정되고 활성화된 Asset 및 Identity 리스트를 모두 병합함. 주 저장된 검색은 ID 관리자 입력에 의해 참조된 Lookup 테이블을 연결하고, 새 필드를 생성하고, 연결된 자산 및 ID 리스트를 대상 Lookup 테이블 파일에 출력함. 보조 저장된 검색은 자산 범주, ID 범주 및 자산 PCI 도메인(Splunk App for PCI Compliance 내)에 대한 Lookup테이블을 생성.
10. 사용자는 데이터가 예상한 대로 표시되는지 확인. Asset 및 Identity 데이터가 Splunk Enterprise Security에 추가되었는지 확인을 참조. ID 및 Asset Lookup 병합 시 입력의 유효성이 확인되거나 중복이 제거되지 않음. ID 관리자 모듈식 입력에서 발생하는 오류는 identity_manager.log에 기록됨. 이 로그에는 데이터 오류가 표시되지 않음.

## Splunk Enterprise Security에서 병합된 Asset 및 Identity 데이터를 저장하는 Lookup

Asset 및 Identity 병합 프로세스가 완료된 후 Lookup 4개에 Asset 및 Identity 데이터가 저장.

<table>
    <tr><td>함수</td><td>테이블이름</td><td>저장된 검색</td><td>Lookup 이름</td></tr>
    <tr><td>문자열 기반 자산 상관</td><td>assets_by_str.csv</td><td>Identity - Asset String Matches - Lookup Gen</td><td>LOOKUP-zu-asset_lookup_by_str-dest<br/>
    LOOKUP-zu-asset_lookup_by_str-dvc<br/>
    LOOKUP-zu-asset_lookup_by_str-src
    </td></tr>
    <tr><td>CIDR 서브넷 기반 자산 상관</td><td>assets_by_cidr.csv</td><td>Identity - Asset CIDR Matches -
    Lookup Gen</td><td>LOOKUP-zv-asset_lookup_by_cidrdest<br/>
    LOOKUP-zv-asset_lookup_by_cidrdvc<br/>
    LOOKUP-zv-asset_lookup_by_cidrsrc</td></tr>
    <tr><td>문자열 기반 ID 상관</td><td>identities_expanded.csv</td><td>
    Identity - Identity Matches - Lookup Gen</td><td>
    LOOKUP-zyidentity_lookup_expanded-src_user<br/>
    LOOKUP-zyidentity_lookup_expanded-user</td></tr>
    <tr><td>기본 필드 상관</td><td>identity_lookup_default_fields.csv<br/>
    asset_lookup_default_fields.csv</td><td></td><td>LOOKUP-zz-asset_identity_lookup_default_fields-dest<br/>
    LOOKUP-zz-asset_identity_lookup_default_fields-dvc<br/>
    LOOKUP-zz-asset_identity_lookup_default_fields-src<br/>
    LOOKUP-zz-asset_identity_lookup_default_fields-src_user<br/>
    LOOKUP-zz-asset_identity_lookup_default_fields-user</td></tr>
</table>

Asset 및 Identity 병합 프로세스에 대한 자세한 내용은 Splunk Enterprise Security가 Asset 및 Identity 데이터를 처리하고 병합하는 방법을 참조.

## Splunk Enterprise Security에서 처리한 후의 Asset 및 Identity 필드

다음 테이블에서는 Splunk Enterprise Security가 소스 Lookup 파일의 처리를 완료한 후에 Asset 및 Identity Lookup에 있는 필드에 대해 설명. 이 필드들은 병합된 Asset 및 Identity 데이터를 저장하는 Lookup에 존재하는 필드임. Splunk Enterprise
Security에서 병합된 Asset 및 Identity 데이터를 저장하는 Lookup을 참조.

병합 프로세스에 대한 자세한 내용은 Splunk Enterprise Security가 Asset 및 Identity 데이터를 처리하고 병합하는 방법을 참조.

### 처리 후의 자산 필드

저장된 검색이 병합 프로세스를 수행한 후의 Asset Lookup 자산 필드

|필드|ETL에서 수행하는 작업|
|:--:|:--|
|bunit|변경되지 않음|
|city|변경되지 않음|
|국가|변경되지 않음|
|dns|파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|lat|변경되지 않음|
|long|변경되지 않음|
|mac|파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|nt_host|파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|owner|변경되지 않음|
|priority|변경되지 않음|
|asset_id|dns, ip, mac 및 nt_host 필드의 값으로부터 생성.|
|asset_tag|category, pci_domain, is_expected, should_timesync, should_update, requires_av 및 bunit 필드의 값으로부터 생성.|
|category|값에 "cardholder"가 포함된 경우 "pci"를 추가. 파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|ip|필요에 따라 필드의 유효성을 검사하고 CIDR 서브넷으로 분할함. 파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|pci_domain|특정 필드 값에 따라 "trust" 또는 "untrust"를 추가. 파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|is_expected|부울로 정규화.|
|should_timesync|부울로 정규화.|
|should_update|부울로 정규화.|
|requires_av|부울로 정규화.|
|key|원래 필드 변환 후 ip, mac, nt_host 및 dns 필드에 의해 생성.|

### 처리 후의 ID 필드

저장된 검색이 병합 프로세스를 수행한 후의 ID Lookup ID 필드

|필드|ETL에서 수행하는 작업|
|:--:|:--|
|bunit|변경되지 않음|
|email|변경되지 않음|
|endDate|변경되지 않음|
|first|변경되지 않음|
|last|변경되지 않음|
|managedBy|변경되지 않음|
|nick|변경되지 않음|
|phone|변경되지 않음|
|phone2|변경되지 않음|
|prefix|변경되지 않음|
|priority|변경되지 않음|
|startDate|변경되지 않음|
|suffix|변경되지 않음|
|work_city|변경되지 않음|
|work_country|변경되지 않음|
|work_lat|변경되지 않음|
|work_long|변경되지 않음|
|watchlist|부울로 정규화.|
|category|값에 "cardholder"가 포함된 경우 "pci"를 추가. 파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|ID|ID Lookup 설정에서 지정된 입력 행의 값과 규칙에 따라 생성. 파이프로 구분된 값을 수락하고 다중값 필드로 변환.|
|identity_id|identity, first, last 및 email 값으로부터 생성.|
|identity_tag|bunit, category 및 watchlist 값으로부터 생성.|

## Splunk Enterprise Security에서 Asset 및 Identity 병합 프로세스 테스트

Asset 및 Identity 병합 프로세스를 테스트하여 병합 프로세스에서 생성되는 데이터가 예상과 일치하고 정확한지 확인할 수 있음. 데이터를 병합된 Lookup에 출력하지 않고 병합 프로세스를 수행하는 저장된 검색을 실행하여 실제로 병합을 수행하지 않고
도 병합 후에 데이터가 어떻게 변경될 것인지 확인할 수 있음. 이 단계는 필수 단계는 아니지만 병합이 예상대로 작동하는지 확인하기 위해 수행할 수 있음.

병합을 수행하고 데이터를 Lookup에 출력하지 않고 병합 프로세스를 테스트.

1. Splunk ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택.
2. 주 저장된 검색 3개 중 첫 번째인 Identity - Asset CIDR Matches - Lookup Gen을 찾음.
3. 검색 이름을 클릭하여 염.
4. 검색 필드에서 검색을 복사.
5. 검색 페이지를 염.
6. 검색을 붙여넣고 `output_*` 매크로를 제거.
    예를 들어
    ```sql
    | `asset_sources` | `make_assets_cidr` | `output_assets("SA-IdentityManagement", "assets_by_cidr.csv")`
    ```
    를
    ```sql
    | `asset_sources` | `make_assets_cidr`
    ```
    로 변경.
7. 검색을 실행.
8. 나머지 두 검색인 Identity - Asset String Matches - Lookup Gen과 Identity - Identity Matches - Lookup Gen에 대해 2-7단계를 반복.

## Splunk Enterprise Security에서 Asset 및 Identity 병합 프로세스 사용자 지정

Asset 및 Identity 병합 프로세스를 수행하는 저장된 검색을 수정하여 추가 필드 변환 또는 데이터 정리(sanitization)를 수행할 수 있음. 병합 프로세스에서 변경할 작업을 검색의 `output_*` 매크로 앞에 추가.

저장된 검색에 대한 특정 수정 사항은 지원되지 않으며, 병합 프로세스 또는 Asset 및 Identity 상관을 망가트릴 수 있음.

- 출력에(서) 필드를 추가하거나 삭제하지 않음.
- 출력 위치를 다른 Lookup 테이블이나 KV 스토어 컬렉션으로 변경하지 않음.
- outputlookup 명령어를 사용하여 `output_*` 매크로를 대체하지 않음.

## Splunk Enterprise Security에서 Asset 및 Identity Lookup 수정

Splunk Enterprise Security에서 Asset 및 Identity Lookup을 변경하여 새 Asset 및 Identity를 추가하거나 Lookup 테이블의 기존 값을 변경할 수 있음. 기존 Lookup을 활성화하거나 비활성화할 수 있음.

### Asset 및 Identity Lookup 편집

ID 관리 대시보드에서 Asset 및 Identity Lookup을 편집할 수 있음.

1. Enterprise Security에서 설정 > 데이터 보강 > ID 관리를 선택.
2. 편집할 Asset 및 Identity 리스트의 이름을 찾고 원본을 선택. 리스트가 대화형 편집기에서 열림.
3. 스크롤 막대를 사용하여 테이블의 컬럼과 행을 봄. 셀을 두 번 클릭하여 콘텐츠를 추가, 변경 또는 제거.
4. 마치면 저장을 클릭.

Asset 및 Identity 리스트에 적용한 변경 사항은 다음으로 예약된 병합 후에 검색 결과에 반영. Splunk Enterprise Security가 Asset 및 Identity 데이터를 처리 및 병합하는 방법을 참조.

### Asset 및 Identity Lookup 비활성화 또는 활성화

Asset 및 Identity Lookup 입력을 비활성화하거나 활성화함. 해당 리스트의 콘텐츠가 병합 프로세스에 포함되지 않게 하려면 해당 입력을 비활성화함. 비활성화된 입력이 다음으로 예약된 Asset 및 Identity 데이터 병합 시에 병합될 수 있게 하려면 리스트를 활성화함. 입력을 비활성화해도 Splunk Enterprise Security의 관련 Lookup에서 데이터가 삭제되지 않음.

1. Enterprise Security에서 설정 > 데이터 보강 > ID 관리를 선택.
2. 비활성화할 Asset 및 Identity Lookup을 찾음.
3. 비활성화 또는 활성화를 클릭.

버전 5.0.0부터 Asset 및 Identity Lookup 입력이 설치 후 기본적으로 비활성화됨. 로컬 설정은 업그레이드 후 유지됨.

### 데모 Asset 및 Identity Lookup 비활성화

데모 Asset 및 Identity Lookup을 비활성화하여 데모 데이터가 Splunk Enterprise Security에서 Asset 및 Identity 상관에 사용되는 주 자산

및 ID Lookup에 추가되지 않게 할 수 있음. 데모 데이터 Lookup을 비활성화한 후에는 저장된 검색이 주 Asset 및 Identity Lookup을 업데이트하고 비활성화된 Lookup의 데이터를 주 Lookup에서 제거.

1. Enterprise Security에서 설정 > 데이터 보강 > ID 관리를 선택.
2. demo_assets 및 demo_identities Lookup을 찾음.
3. 각 Lookup에 대해 비활성화를 클릭.

### 복제에(서) Asset 및 Identity Lookup 번들 포함 또는 제거

4.7.0 버전부터는 Asset 및 Identity 원본 Lookup 파일이 인덱서 클러스터의 번들 복제에서 기본적으로 제외됨. Asset 및 Identity 상관을 지원하기 위해 병합된 Lookup 파일은 번들 복제에 계속 포함됨. 번들 복제에 계속 포함되는 Lookup 파일은 Splunk
Enterprise Security에서 병합된 Asset 및 Identity 데이터를 저장하는 Lookup을 참조.

번들 복제에 Asset 및 Identity Lookup 파일을 포함으로 기본값을 변경하면 시스템 성능이 저하될 수 있음.

1. Enterprise Security에서 설정 > 데이터 보강 > ID 관리를 선택.
2. 번들 복제에(서) 포함시키거나 제외할 Lookup을 클릭.
3. 차단 리스트 체크박스를 선택하거나 선택 해제. 선택하면 Lookup 파일이 번들 복제에서 제외됨.

"ID 생성 자동 업데이트 활성화" 설정을 "true"로 설정한 경우에만 이렇게 변경할 수 있음. Splunk Enterprise Security의 일반 설정 구성을 참조.

## Splunk Enterprise Security에서 Asset 및 Identity 데이터를 추가하는 방법의 예

아래의 방법 예에서는 Asset 및 Identity 데이터를 Splunk Enterprise Security에 추가하는 일반적인 방법을 몇 가지 다룸.
Splunk 프로페셔널 서비스와 함께 환경에 가장 적합한 솔루션을 찾으실 수 있음.

### Active Directory의 Asset 및 Identity 데이터 추가

다음 예에서는 Active Directory의 Asset 및 Identity 데이터를 추가하는 방법에 대해 설명.

#### Splunk Support for Active Directory 앱 설치

Splunk Support for Active Directory 앱을 사용하여 Asset 및 Identity 데이터를 수집할 수 있음. 앱 설치 및 설정에 대한 내용
은 Splunk Supporting Add-on for Active Directory 설치를 참조.

#### Active Directory의 Asset 및 Identity 데이터 수집

SA-ldapsearch에서 Active Directory의 Asset 및 Identity 데이터를 검색하여 수집할 수 있음.

1. 다음 절차에 따라 새 Asset 및 Identity 리스트를 설정. Splunk Enterprise Security에서 새 Asset 및 Identity 리스트 설정을 참조.
2. 만든 Lookup 파일을 저장된 검색을 설정하는 작업을 마칠 때까지 비활성화하여 Asset 및 Identity 데이터가 불완전하거나 부정확한 데이터와 병합되지 않도록 함. Asset 및 Identity Lookup 비활성화 또는 활성화를 참조.
3. ldapsearch 명령어를 사용하여 SA-IdentityManagement에 저장된 검색을 만들어 Lookup 파일을 채움. 이 검색의 정확한 구문은 AD 설정에 따라 다름. Active Directory의 ID 데이터 수집을 위한 검색 예와 Active Directory의 자산 데이터 수집을 위한 검색 예의 두 가지 예를 참조.
4. 병합 프로세스를 테스트. Splunk Enterprise Security에서 Asset 및 Identity 병합 프로세스 테스트를 참조.

#### Active Directory의 ID 데이터 수집을 위한 검색 예

이 검색 예에서는 suffix, endDate, category, watchlist 및 priority에 고정 값을 배정함. 이 예를 실제로 작동하는 검색을
구성하고 테스트하기 위한 가이드로 사용한 다음, 고정 값을 AD 환경의 정보로 바꿈. my_identity_lookup Lookup을 환경에
적절한 이름으로 바꿈.

```sql
|ldapsearch domain=<domain_name> search="(&(objectclass=user)(!(objectClass=computer)))"
|makemv userAccountControl
|search userAccountControl="NORMAL_ACCOUNT"
|eval suffix=""
|eval priority="medium"
|eval category="normal"
|eval watchlist="false"
|eval endDate=""
|table
sAMAccountName,personalTitle,displayName,givenName,sn,suffix,mail,telephoneNumber,mobile,manager,priority,department,category,watchlist,whenCreated,|rename sAMAccountName as identity, personalTitle as prefix, displayName as nick, givenName as first, sn as last,
mail as email, telephoneNumber as phone, mobile as phone2, manager as managedBy, department as bunit, whenCreated as
startDate
|outputlookup my_identity_lookup
```

#### Active Directory의 자산 데이터 수집을 위한 검색 예

이 검색 예에서는 여러 필드에 고정 값을 배정함. 이 예를 실제로 작동하는 검색을 구성하고 테스트하기 위한 가이드로 사용한 다음, 고정 값을 AD 환경의 정보로 바꿈. my_asset_lookup Lookup을 환경에 적절한 이름으로 바꿈.

```sql
|ldapsearch domain=<domain name> search="(&(objectClass=computer))"
|eval city=""
|eval country=""
|eval priority="medium"
|eval category="normal"
|eval dns=dNSHostName
|eval owner=managedBy
|rex field=sAMAccountName mode=sed "s/\$//g"
|eval nt_host=sAMAccountName
|makemv delim="," dn
|rex field=dn "(OU|CN)\=(?<org>.+)"
|table
ip,mac,nt_host,dns,owner,priority,lat,long,city,country,bunit,category,pci_domain,is_expected,should_timesync,should_update,requires_av
| outputlookup create_empty=false createinapp=true my_asset_lookup
```

### Splunk 플랫폼에서 인덱싱된 이벤트로부터 자산 데이터 추가

다음은 기존 자산 데이터와 현재 연결되어 있지 않은 인덱싱된 이벤트에 나타나는 호스트를 식별하고 해당 호스트를 자산 룩
업에 추가하는 방법을 보여주는 예임.
이 예를 사용하여 Splunk 플랫폼과 통신하는 호스트를 기존 자산 정보 집합과 비교하고 일치되지 않은 호스트의 테이블을
검토함. 그런 다음 테이블을 자산 리스트 형식으로 내보낼 수 있음.

```sql
| `host_eventcount`
| search host_is_expected=false NOT host_asset_id=*
| fields - firstTime,recentTime,lastTime,_time, host_owner_*,host_asset_tag,host_asset_id
| sort -totalCount,dayDiff
| table
host,ip,mac,nt_host,dns,owner,priority,lat,long,city,country,bunit,category,pci_domain,is_expected,should_timesync,should_update,requires_av
```

### 수동으로 새 Asset 및 Identity 데이터 추가

Asset 및 Identity Lookup을 편집하여 새 Asset 및 Identity 데이터를 Splunk Enterprise Security에 수동으로 추가할 수 있음. 예를들어 내부 서브넷과, 허용 리스트에 포함할 IP 주소, 그리고 기타 정적 자산 및 식별 데이터를 추가할 수 있음.

1. Splunk ES 메뉴 모음에서 설정 > 콘텐츠 관리를 선택.
2. 자산 데이터를 추가하려면 Asset Lookup을 클릭하여 편집함. ID 데이터를 추가하려면 ID 리스트를 클릭하여 편집함.
3. 스크롤 막대를 사용하여 테이블의 컬럼과 행을 봄. 셀을 두 번 클릭하여 콘텐츠를 추가, 변경 또는 제거.
4. 변경 사항을 저장함.