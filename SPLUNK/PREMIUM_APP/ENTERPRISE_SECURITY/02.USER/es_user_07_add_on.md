포함된 포함된 추가추가 기능기능
Enterprise Security에서에서 Splunk UBA 데이터 데이터 보기보기
Splunk Enterprise Security와 Splunk UBA(User Behavior Analytics)를 통합한 후에는 두 앱이 정보를 공유할 수 있고, 사 용자는 환경에서 조직이 직면하는 다양한 유형의 보안 위협을 식별할 수 있습니다.
위협과 이상을 Splunk UBA에서 Splunk Enterprise Security로 전송하여 위험 점수를 조정하고 주요 이벤트를 만듭 니다. 상관(correlation) 검색 결과를 Splunk Enterprise Security에서 Splunk UBA로 전송하여 이상을 처리합니다. Splunk UBA에서 사용자 및 장치 연결 데이터를 검색하여 Splunk Enterprise Security에서 확인합니다. 특정 세션 중 에 장치와 연결되는 사용자 계정과 장치를 식별하고, 특정 세션 중에 사용자와 연결되는 장치를 식별합니다.
Enterprise Security의 여러 위치에서 Splunk UBA에서 가져온 데이터를 볼 수 있습니다.
UBA 이상 대시보드에서 이상을 봅니다. Asset 및 Identity Investigator 대시보드에서 위협 및 이상 스윔 레인(Swim lane)을 봅니다. 세션 센터 대시보드에서 세션 관련 사용자 및 장치 연결 데이터를 봅니다.
Splunk add-on for Splunk UBA에서 Splunk Enterprise Security와 Splunk UBA 통합을 참조하십시오.
보안보안 보스처 보스처 및 인시던트 인시던트 검토에서 검토에서 위협위협 보기보기
Splunk UBA에서 Splunk Enterprise Security로 전송된 위협은 인시던트 검토 및 보안 포스처 대시보드에 주요 이벤트로 표시됩니다. 위협으로 인해 생성된 주요 이벤트 수를 보안 포스처 대시보드에서 핵심 보안 지표(KSI)로 볼 수 있습니다.
인시던트 검토에서 이벤트 세부정보를 확장하여 설명, 위협 범주, Splunk UBA를 참조하는 상관(correlation) 검색 및 추가 세부정보를 볼 수 있습니다. 이벤트의 워크플로 작업을 사용하여 Splunk UBA에서 기여기여 이상을 이상을 보고보고 위협 세부정보 페이지 를 여십시오. Splunk UBA 사용자 매뉴얼에서 위협 세부정보를 참조하십시오.
UBA 이상이상 대시보드에서 대시보드에서 이상이상 보기보기
UBA 이상 대시보드를 사용하여 Splunk UBA의 이상 정보를 Enterprise Security에서 보고 환경 내 이상 작업에 대해 알 수 있습니다. 대시보드를 보려면 보안보안 인텔리전스 인텔리전스 > 사용자 사용자 인텔리전스 인텔리전스 > UBA 이상이상을 선택하십시오.
핵심 지표를 사용하여 환경의 여러 메트릭 카운트가 지난 48시간 동안 어떻게 변했는지 확인합니다. UBA 주요 사항 수, UBA 이상 행위자, UBA 이상 서명, 위협당 UBA 이상, 그리고 총 UBA 이상 수를 검토합니다. 시간에 시간에 따른따른 이상이상 패널에서 이상 작업 급증에 대해 조사하고 일정 기간의 행위자 수를 이상 수와 비교합니다. 가장가장 활동적인 활동적인 서명서명 패널에서 가장 일반적인 이상 작업 유형을 식별합니다. 가장가장 활동적인 활동적인 행위자 행위자 패널에서 가장 많은 이상 작업에 대한 책임이 있는 사용자, 장치, 앱 및 기타 행위자를 확인합니 다. 최근최근 UBA 이상이상 패널에서 최근 이상 작업을 봅니다.
대시보드에서 값을 클릭해 검색까지 드릴다운하여 Splunk UBA에서 이상을 보십시오. 특정 이상 이벤트의 이벤트 작업을 사용하여 기여기여 이상을 이상을 보고보고 Splunk UBA를 열어 이상이상 세부정보 세부정보 뷰를 표시하십시오. Splunk UBA 사용자 매뉴얼에서 이상 세부정보를 참조하십시오.
Asset 및 Identity Investigator 대시보드에서 대시보드에서 위협위협 및 이상이상 스윔스윔 레인레인(Swim lane) 보기보기
Asset 및 Identity Investigator 대시보드에서 스윔 레인(Swim lane)을 사용해 UBA 위협 및 이상 수를 ES의 기타 주요 이벤 트와 상관할 수 있습니다.
검색하는 각 자산 또는 ID와 연관된 이상 및 위협 정보를 보려면 UEBA 위협 및 UBA 이상 스윔 레인(Swim lane)을 Asset Investigator 및 Identity Investigator 대시보드에 추가하십시오. 스윔 레인(Swim lane) 편집을 참조하십시오.
스윔 레인(Swim lane)을 클릭하여 추가 세부정보가 포함된 검색을 열어 Splunk UBA에서 이상을 볼 수 있습니다. 이벤트 작업을 사용하여 기여기여 이상을 이상을 보고보고 Splunk UBA를 열어 이상이상 세부정보 세부정보 또는 위협위협 세부정보를 세부정보 보십시오. 자세한 내용은 현 재 위협 검토를 참조하십시오.
이상과 이상과 위협은 위협은 위험위험 점수를 점수를 수정함 수정함 Enterprise Security는 Splunk UBA의 이상 및 위협 위험 점수를 사용하여 위협과 이상과 연결된 자산과 ID의 위험을 수정 합니다. 위험 점수 수정자는 Splunk UBA의 이상 또는 위협 위험 점수의 10배입니다.
예:
1. Splunk UBA가
10.11.12.123
호스트에 해당되는 이상을 Enterprise Security로 보냅니다. 이상의 위험 점수는 8입니 다.

2. Enterprise Security는 이상에 대응하여
10.11.12.123
호스트의 위험을 수정합니다. 위험 수정자는 10 * UBA 위험 점 수이므로 위험 수정자는 80입니다.
위험 분석 대시보드에서 위험 점수를 분석할 때 위험 증가 원인을 볼 수 있습니다.
세션세션 센터센터 대시보드에서 대시보드에서 사용자 사용자 및 장치장치 세션세션 연결연결 보기보기
세션 센터에서 자산이나 ID를 검색하는 경우, 세션 데이터를 기반으로 Splunk UBA에서 장치와 연결되었을 수 있는 사용자 나 사용자와 연결되었을 수 있는 장치에 대한 추가 데이터를 검색할 수 있습니다. 세션 센터 대시보드를 참조하십시오.
주요 이벤트를 분류할 때 인시던트 검토 대시보드에서 세션 센터 대시보드를 바로 열 수도 있습니다. 주요 이벤트의 추가 세 부 정보를 볼 때 사용자 또는 장치 필드의 워크플로 작업을 클릭하고 세션 센터 대시보드를 여십시오.
Splunk UBA로 상관상관(correlation) 검색검색 결과결과 전송전송
Enterprise Security와 Splunk UBA를 설정한 후 상관(correlation) 검색 결과를 Splunk UBA로 전송하기 시작할 수 있습니 다. 상관(correlation) 검색 결과를 자동으로 전송하거나, 인시던트 검토 대시보드에서 주요 이벤트를 전송하여 상관 (correlation) 검색 결과를 직접 전송할 수 있습니다.
Splunk UBA로 상관상관(correlation) 검색검색 결과결과 자동자동 전송전송
기존 상관(correlation) 검색을 편집하거나 새 상관(correlation) 검색을 만들어서 UBA로 전송전송 대응 작업을 추가해 상관 (correlation) 검색 결과를 자동으로 Splunk UBA로 전송하십시오.
1. Enterprise Security 메뉴 모음에서 설정설정 > 콘텐츠 콘텐츠 관리관리를 선택합니다. 2. 상관(correlation) 검색의 이름을 클릭하거나, 새로새로 만들기를 만들기 클릭하여 새 상관(correlation) 검색을 만듭니다. 3. 새 대응대응 작업작업 추가추가를 클릭하고 UBA로 전송전송을 선택합니다. 4. 심각도를 심각도 입력하여 상관(correlation) 검색 결과에서 발생할 수 있는 이상의 점수를 Splunk UBA에서 설정합니다.
예를 들어 높은 심각도를 나타내려면 7을 입력하십시오. 5. 상관(correlation) 검색을 저장합니다.
인시던트 인시던트 검토에서 검토에서 직접직접 상관상관(correlation) 검색검색 결과결과 전송전송
상관(correlation) 검색 결과에 따라 생성된 주요 이벤트를 인시던트 검토 대시보드에서 Splunk UBA로 직접 전송할 수 있습 니다.
1. 인시던트 검토 대시보드에서, Splunk UBA로 전송할 주요 이벤트를 찾습니다. 2. 작업작업 컬럼에서 Adaptive Response 작업작업 실행실행을 선택합니다. 3. 새 대응대응 작업작업 추가추가를 클릭하고 UBA로 전송전송을 선택합니다. 4. (선택 사항) 심각도를 심각도 입력하여 주요 이벤트에서 발생할 수 있는 이상의 점수를 Splunk UBA에서 설정합니다. 입력된
심각도가 주요 이벤트의 기본 심각도보다 우선합니다. 5. 실행실행을 클릭하여 대응 작업을 실행하고 주요 이벤트 세부정보를 Splunk UBA로 전송합니다.
Splunk UBA로 전송할 전송할 결과결과 유형유형
일부 상관(correlation) 검색 결과만 Splunk UBA에서 이상을 초래합니다. Splunk UBA는 상관(correlation) 검색 결과를 외 부 알람으로 파싱하며, 결과에 소스, 대상 또는 사용자가 포함된 상관(correlation) 검색은 Splunk UBA에서 이상을 초래할 가능성이 큽니다. Enterprise Security에서 전송한 상관(correlation) 검색 결과가 모두 Splunk UBA에서 이상으로 나타나지 는 않습니다. 상관(correlation) 검색 결과에 관련 데이터가 포함된 경우에만 Splunk UBA에서 이상이 발생하고, 나머지 상 관(correlation) 검색 결과는 무시됩니다.