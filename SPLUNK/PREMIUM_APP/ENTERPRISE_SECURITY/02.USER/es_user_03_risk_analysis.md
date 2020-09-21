위험위험 분석분석
Splunk Enterprise Security의 위험위험 분석분석
위험 점수는 네트워크 환경 내 장치 또는 사용자의 상대적인 위험을 시간에 따라 나타내는 단일 메트릭입니다. Splunk Enterprise Security에서는 장치를 시스템으로, 사용자를 사용자로, 인식되지 않은 장치 또는 사용자를 기타로 분류합니다.
Enterprise Security는 위험 분석을 사용하여 작은 이벤트와 의심스러운 동작을 주지하고 그것이 시간이 지남에 따라 환경에 미치는 위험을 계산합니다. 위험 분석 대시보드에는 이런 위험 점수와 기타 위험 관련 정보가 표시됩니다. Enterprise Security는 모든 위험을
risk
인덱스에 이벤트로 인덱싱합니다.
Splunk Enterprise Security가 위험위험 점수를 점수를 배정하는 배정하는 방법방법
위험 점수는 네트워크 환경 내 장치 또는 사용자 개체의 상대적인 위험을 시간에 따라 나타내는 단일 메트릭입니다. 개체는 시스템, 시스템 사용자, 사용자 또는 지정되지 않은 기타기타를 나타냅니다.
Enterprise Security는 상관(correlation) 검색을 사용하여 머신 데이터를 네트워크 환경의 장치와 사용자 개체를 구성하는 자산 및 ID 데이터와 상관합니다. 상관(correlation) 검색에서는 질문에 대한 조건부 일치 항목을 검색합니다. 일치 항목을 찾 으면 경고가 주요 이벤트 및/또는 위험 수정자로 생성됩니다.
주요 이벤트는 작업이 됩니다. 이는 배정, 검토 또는 종료해야 하는 이벤트입니다. 위험 수정자는 숫자가 됩니다. 이는 장치 또는 사용자 개체의 위험 점수에 더해질 이벤트입니다.
위험위험 점수점수 계산계산 예
RLOG-10 호스트는 주요 이벤트를 몇 개 생성하는 점프 서버입니다. 로그인 로그인 시도시도 횟수횟수 초과초과 및 기본기본 계정계정 작업작업 탐지됨 탐지됨 상 관(correlation) 검색은 해당 시스템을 위해 주요 이벤트를 하루에 하나씩 만듭니다. RLOG-10은 점프 서버이므로, 여러 네트 워크 자격 증명이 이 호스트에 사용되고, 소프트웨어 또는 기타 유틸리티가 설치되었을 수 있습니다. 점프 서버에서 이 동작 은 동일한 동작이 프로덕션 DNS 서버에서 관찰되는 경우보다 덜 흥미롭습니다. 점프 서버에서 생성된 주요 이벤트를 무시 하거나 제거하지 않고 점프 서버 관련 규칙을 만들어서 해당 서버를 다르게 모니터링할 수 있습니다.
상관에서 점프 서버로 사용되는 호스트가 일치하면 위험 수정자를 배정하는 상관(correlation) 검색을 만들어서 이렇게 할 수 있습니다.
1. 점프 서버를 허용 리스트를 사용해 기존 상관(correlation) 검색에서 격리하십시오. 자세한 내용은 Splunk Enterprise
Security 관리에서 허용 리스트 이벤트를 참조하십시오. 2. 로그인 로그인 시도시도 횟수횟수 초과초과를 기반으로 새 상관(correlation) 검색을 만들고 예약하되, 검색을 점프 서버 호스트로 한정하
고 위험 수정자 경고 유형만 배정합니다. 3. 위험 점수를 서서히 높여서 위험 수정자가 점프 서버 호스트에 적용되는지 확인합니다. 새 상관(correlation) 검색을 사
용하면 실패된 로그인 기반 주요 이벤트가 해당 호스트에 대해 생성되지 않습니다.
상대적인 위험 점수가 높아짐에 따라 RLOG-10을 모든 네트워크 서버와 기타 점프 서버에 비교할 수 있습니다. RLOG-10의 상대적인 위험 점수가 피어 서버들보다 높아지면 해당 호스트를 애널리스트가 조사합니다. 모든 점프 서버의 위험 점수가 다 른 네트워크 호스트보다 높은 경우, 내부 보안 정책을 검토하거나 다르게 시행해야 할 수 있습니다. 추가 예는 Enterprise Security 3.1로 위험 분석 블로그 게시물을 참조하십시오.
개체에 개체에 위험위험 배정배정
위험 분석 대응 작업 또는 위험 수정자를 만들어 개체에 위험을 배정합니다. 몇 가지 방법으로 위험을 개체에 배정할 수 있습 니다.
위험을 상관(correlation) 검색의 일부로 자동 배정. Splunk Enterprise Security 관리에서 위험 수정자를 사용하여 위 험 점수 수정을 참조하십시오. 인시던트 검토에서 위험을 임시 adaptive response 작업으로 배정. 이 매뉴얼에서 위험 수정자를 사용하여 위험 점수 수정을 참조하십시오. 위험 분석 대시보드에서 임시 위험 항목 만들기. 이 매뉴얼에서 Splunk Enterprise Security에서 임시 위험 항목 만들 기를 참조하십시오. 검색을 통해 위험 배정. 아래 예를 참조하십시오.
검색을 검색을 통해통해 위험위험 배정배정
경고가 아니라 검색을 사용하여 위험을 배정할 수 있습니다. 이렇게 하여 여러 위험 개체의 위험을 수정하거나 검색의 결과 를 기반으로 개체의 위험 점수를 변경할 수 있습니다.
사용자, 시스템 또는 사용자 지정 상관(correlation) 검색의 다른 위험 개체를 배정하려면 이 검색 예를 참조하십시오. 한 필드 에만, 또는 임시로 위험을 배정하려면 위험 adaptive response 작업을 대신 사용하십시오.
각 예에서는
...
을 사용하여 위험을 배정하려는 필드가 결과에 포함된 검색을 나타냅니다.
21
appendpipe 명령어어를 명령어어를 사용하여 사용하여 위험위험 배정배정
여러 개체에 위험을 추가하려면
appendpipe
를 사용합니다.
<your_risk_score_integer>
를 필드에 적용할 위험 점수로 대체합 니다.
... | eval risk_score=<your_risk_score_integer> | eval
risk_object=if(isnotnull(dest),dest,null()),risk_object_type=if(isnotnull(dest),"system",null()) | appendpipe [|
eval risk_object=if(isnotnull(user),user,null()),risk_object_type=if(isnotnull(user),"user",null())] | sendalert
risk param._risk_score=<your_risk_score_integer>
예를 들어,
mysystem
및
myuser
에 위험 점수 15를 배정하려면 이 검색을 실행합니다.
| makeresults | eval dest="mysystem", user="myuser" | eval
risk_object=if(isnotnull(dest),dest,null()),risk_object_type=if(isnotnull(dest),"system",null()) | appendpipe [|
eval risk_object=if(isnotnull(user),user,null()),risk_object_type=if(isnotnull(user),"user",null())] | sendalert
risk param._risk_score=15
sendalert를 사용하여 사용하여 위험위험 배정배정
appendpipe
없이
sendalert
를 사용하여 필드 값을 조건부로 평가하지 않고 필드 값에 직접 위험을 배정할 수 있습니다.
... | sendalert risk param._risk_object_type="system" param._risk_score=<your_risk_score_integer> | eval
risk_object=user | sendalert risk param._risk_object_type="user" param._risk_score=<your_risk_score_integer>
예:
| makeresults | eval dest="mysystem", user="myuser" | sendalert risk param._risk_object="dest"
param._risk_object_type="system" param._risk_score=15 | sendalert risk param._risk_object="user"
param._risk_object_type="user" param._risk_score=20
위험위험 점수점수 계산계산 및 배정배정
고정된 정수로 설정하는 대신 검색에서 수행된 계산 결과를 기반으로 위험 점수를 설정할 수도 있습니다.
예를 들어, 감염된 여러 자산에 로그인하고, 감염된 자산에 로그인한 사용자를 수집한 다음 결과의 사용자 수를 계산하는 검 색을 작성하는 사용자에게 더 높은 위험 점수를 설정하려면 사용자별로 분할하여 각 사용자의 로그인 시도 횟수를 확인하십 시오.
... | stats count by user | eval risk_score=(count*2) | sendalert risk param._risk_object="user"
param._risk_object_type="user" param._risk_score="risk_score"
예를 들어 위협위협 활동활동 탐지됨 탐지됨 상관(correlation) 검색에서는 검색 배정 위험을 경고 유형 위험 수정자와 함께 사용합니다. 설 정된 위협 리스트와 일치하는 호스트와 통신하는 자산 또는 ID를 검색에서 찾을 경우, 위험 점수가 적절히 수정됩니다. 이 경 우, 위험 수정자는 시스템 또는 사용자가 위협 리스트와 통신한 횟수에 위협 리스트의 가중치를 곱한 수를 나타냅니다. 공식 은 시스템 또는 사용자의 위험 점수 + (위협 리스트 가중치 x 이벤트 수) = 추가 위험입니다.
... | eval risk_score=case(isnum(record_weight), record_weight, isnum(weight), weight, 1=1, null()) | fields - *time
| eval risk_object_type=case(threat_match_field="query" OR threat_match_field=="src" OR
threat_match_field=="dest","system",threat_match_field=="src_user" OR threat_match_field=="user","user",1=1,"other")
| eval risk_object=threat_match_value
검색의 검색의 위험위험 점수점수 확인확인
데이터 모델 또는 위험 상관(correlation) 룩업을 검색하여 변경한 내용을 확인하십시오.
| from datamodel:Risk.All_Risk | search (risk_object=myuser OR risk_object=mysystem)
또는
| makeresults | eval dest="mysystem" | `risk_correlation`
위험위험 점수점수 범위범위
위험 점수는 위험 수정자를 사용하여 자산 또는 ID의 활동을 메트릭 하나로 수집 및 집계하는 방법을 제공합니다.
Enterprise Security에 포함된 상관(correlation) 검색에는 상관(correlation) 검색에서 찾은 활동의 상대적인 심각도에 따라 20과 100 사이의 위험 점수가 배정됩니다. 검색에서는 기본 점수의 범위를 실제 범위로 지정합니다. 이 범위는 업계 표준을 나타내지 않습니다. Enterprise Security는 ID 또는 자산의 총 위험 점수에 대해 상한을 정의하지 않지만, 운영 체제에서 제 한을 가할 수 있습니다. 예를 들어 32비트 운영체제는 위험 점수를 2백만으로 제한합니다.
22
위험 점수 레벨에는 이벤트 심각도와 동일한 명명 규칙이 사용됩니다. 역할 및 자산 우선순위와 유사 호스트를 비교하여 상 대 위험 점수를 평가할 수 있습니다.
20 - 정보용 40 - 낮음 60 - 중간 80 - 높음 100 - 중요
ES 관리자는 상관(correlation) 검색을 편집하여 위험 분석 대응 작업에서 개체에 배정하는 위험 점수를 수정할 수 있습니다. Splunk Enterprise Security 관리에서 Splunk Enterprise Security에 포함된 adaptive response 작업을 참조하십시오.
위험위험 개체개체 관리관리
Enterprise Security는 위험 수정자를 위험 개체와 연결합니다.
위험위험 개체개체 필드필드
위험 개체 필드는 상관(correlation) 검색에서 반환되는 검색 필드와 관련된 용어입니다. 상관(correlation) 검색에서는
src
및
dest
같은 필드를 사용하여 일치하는 결과를 보고합니다. 위험 개체 필드는 시스템, 호스트, 장치, 사용자, 역할, 자격 증명, 또 는 상관(correlation) 검색이 보고하도록 고안된 개체를 나타냅니다. 위험 점수가 지정되는 필드의 예를 보려면 위험 점수를 배정하는 상관(correlation) 검색을 검토하십시오.
위험위험 개체개체 유형유형
Splunk Enterprise Security는 세 가지 위험 개체 유형을 정의합니다.
개체개체 유형유형 설명설명
시스템 시스템 네트워크 장치 또는 기술. 자산 룩업의 장치를 나타낼 수 있음
사용자 사용자 네트워크 사용자, 자격 증명, 또는 역할. ID 룩업의 ID를 나타낼 수 있음
기타기타 데이터 원본의 필드로 표시되는 정의되지 않은 개체
위험 개체가 자산 또는 ID 테이블과 일치하는 경우, Enterprise Security는 개체를 연결된 유형으로 매핑합니다. 예를 들어 자산 룩업의 자산과 일치하는 개체는 시스템의 위험 개체 유형에 매핑됩니다. 그러나 장치와 사용자가 시스템 또는 사용자 위험 개체로 식별되기 위해 해당 자산 및 ID 테이블에 표시되어야 하는 것은 아닙니다. ES는 정의되지 않거나 실험적인 개체 유형을 기타기타 위험 개체 유형으로 분류합니다.
Splunk Enterprise Security에서에서 임시임시 위험위험 항목항목 작성작성
임시 위험 항목을 만들면 개체의 위험 점수를 수동으로 한 번 조정할 수 있습니다. 이 점수를 사용하여 개체의 위험 점수에 양 수나 음수를 더할 수 있습니다.
1. 보안보안 인텔리전스 인텔리전스 > 위험위험 분석분석을 선택합니다. 2. 임시임시 위험위험 항목항목 작성작성을 클릭합니다. 3. 양식을 작성합니다.
임시임시 위험위험 점수점수 필 드
설명설명
점수 위험위험 개체개체에 추가되는 숫자. 양의 정수나 음의 정수일 수 있습니다.
설명
개체의 위험 점수를 수동으로 조정한 이유나 관련 메모. 임시 위험 점수의 경우 설명설명 필드는 필수입 니다.
위험 개체 텍스트 필드. 별표(*)가 있는 와일드카드
위험 개체 유형 드롭다운: 필터 기준으로 선택