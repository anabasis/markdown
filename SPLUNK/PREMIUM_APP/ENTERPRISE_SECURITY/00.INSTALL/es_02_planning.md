# 계획

## 배포 계획

Splunk Enterprise Security를 설정된 Splunk 플랫폼 설치본에 배포합니다. Enterprise Security를 배포하기 전에 시스템 및 하드웨어 요구 사항과 검색헤드 및 인덱서 고려 사항을 검토하십시오.

### 사용 가능한 배포 아키텍처

Splunk Enterprise Security를 단일 인스턴스 배포 또는 분산 검색 배포 아키텍처에서 배포할 수 있습니다. Splunk Enterprise Security는 Splunk Cloud에서도 사용 가능합니다. Splunk Enterprise Security를 사내에서 배포하기 전에 Splunk 플랫폼 배포 구성 요소를 숙지하십시오. 용량 계획 매뉴얼에서 Splunk Enterprise 배포 구성 요소를 참조하십시오.

#### 단일 인스턴스 배포

간단하고 작은 배포 환경에서는 Splunk Enterprise Security를 단일 Splunk 플랫폼 인스턴스에 설치하십시오. 단일 인스턴 스가 검색헤드와 인덱서의 기능을 모두 수행합니다. 포워더를 사용하여 데이터를 수집하고 단일 인스턴스로 전송하여 파싱 및 저장하고 검색할 수 있습니다.
단일 인스턴스 배포를 연구 또는 실험 환경이나 사용자 1-2명이 동시 검색을 실행하는 작은 시스템에 사용할 수 있습니다.

#### 분산 검색 배포

Splunk Enterprise Security의 배포 및 실행에는 분산 검색 배포가 권장됩니다.
Splunk Enterprise Security를 전용 검색헤드 또는 검색헤드 클러스터에 설치합니다. 인덱스 클러스터를 사용하여 검색 데이터 워크로드를 여러 노드로 분산시켜 검색 성능을 개선합니다. 여러 인덱서를 사용하면 포워더에서 수집하는 데이터와 데이터 처리 워크로드를 모두 인덱서 간에 분산시킬 수 있습니다. 포워더를 사용하여 데이터를 수집하고 인덱서로 전송합니다.
분산 검색 배포에서, 그리고 검색헤드 클러스터링을 구현하기 위해 모든 데이터를 인덱서에 전달하도록 검색헤드를 설정하 십시오. 분산 검색 매뉴얼에서 검색헤드 데이터를 인덱서 계층으로 전달을 참조하십시오.
분산 검색 배포를 Splunk Enterprise Security에 적절한 규모로 조정하는 방법은 용량 계획 매뉴얼의 Splunk Enterprise 용 량 계획 소개와 Splunk Enterprise Security 인덱서 확장 고려 사항을 참조하십시오.

#### 클라우드 배포

Splunk Enterprise Security는 Splunk Cloud에서 서비스로 제공됩니다. Splunk Cloud 배포 아키텍처는 데이터와 검색 부 하에 따라 다릅니다. Splunk Cloud 고객은 Splunk 서포트와 협력하여 클라우드 인프라를 설치 및 관리하고 유지합니다. Splunk Cloud 관리 배포에 대한 내용은 Splunk Cloud 사용자 매뉴얼에서 Splunk Cloud 배포 유형을 참조하십시오.

### Splunk Enterprise 시스템 요구요구 사항사항

Splunk Enterprise Security를 사용하려면 모든 검색헤드와 인덱서에 64비트 OS가 설치되어 있어야 합니다. 지원되는 운 영 체제, 브라우저 및 파일 시스템을 나열한 리스트는 Splunk Enterprise 설치 매뉴얼에서 Splunk Enterprise 사내 사용을 위한 시스템 요구 사항을 참조하십시오.
다음 테이블에서 Enterprise Security 5.0.x 버전과 Splunk 플랫폼 버전의 호환성을 확인하십시오.

|Splunk Enterprise Security 버전|Splunk 플랫폼 버전|
|:--:|:--:|
|5.0.0|6.6.x, 7.0.x 이상|

### 하드웨어 요구 사항

Splunk Enterprise Security를 사용하기 위해 최소한으로 요구되는 하드웨어 사양이 있으며, 필요와 Splunk Enterprise Security 용도에 따라 사양을 높여야 할 수 있습니다. 다음 사양은 Splunk Enterprise Security의 단일 배포 인스턴스에도 해당됩니다.

|컴퓨터 역할|최소 CPU|최소 RAM|
|:--:|:--:|:--:|
|검색헤드|16 코어|32GB|
|인덱서|16 코어|32GB|

인덱싱은 I/O 집약적인 프로세스입니다. 인덱서에는 검색 요청에 대응하면서 데이터를 효율적으로 수집 및 파싱하는데 충분한 디스크 I/O가 필요합니다. Splunk Enterprise 실행을 위한 최신 IOPS 요구 사항은 용량 계획 매뉴얼에서 참조 하드웨어: 인덱서를 참조하십시오.

환경에 따라, Enterprise Security 배포 하드웨어 사양을 최소 하드웨어 요구 사양보다 높게 올려야 할 수 있습니다. 시스템 구성에 따라, Splunk 플랫폼 참조 하드웨어의 중급 또는 고성능 사양을 참조하십시오. 용량 계획 매뉴얼에서 중급 사양 및 고 성능 사양을 참조하십시오.

배포의 인덱서 CPU 코어 수가 최소 하드웨어 사양보다 많은 경우, 특정 이용 사례에서 인덱서 성능을 개선하기 위해 병렬화 설정 중 하나를 구현할 수 있습니다. 용량 계획 매뉴얼에서 병렬 처리 설정을 참조하십시오.

### Splunk Enterprise Security 검색헤드 고려사항

Splunk Enterprise Security를 전용 검색헤드 또는 전용 검색헤드 클러스터에 설치하십시오. CIM(Common Information Model) 호환 앱 또는 추가 기능만 Splunk Enterprise Security와 동일한 검색헤드에 설치할 수 있습니다. 예를 들어 Splunk App for PCI Compliance(Splunk Enterprise Security용) 또는 Facebook ThreatExchange용 Splunk 추가 기능은 모두 Splunk Enterprise Security와 동일한 검색헤드에 설치할 수 있습니다.
Splunk Enterprise Security의 모든 실시간 실시간 검색검색에서는 인덱싱된 실시간 설정을 사용하여 인덱싱 성능을 개선합니다. 검색 매뉴얼에서 실시간 검색 및 보고서를 참조하십시오. 인덱싱된 실시간 검색 설정을 비활성화하면 인덱서의 전체 인덱싱 용량 이 감소합니다. 실시간 검색 유형이 성능에 미치는 영향을 검토하려면 검색 매뉴얼에서 실시간 검색의 알려진 제한 사항을 참조하십시오.

Splunk Enterprise Security에서 KV 스토어는 필수합니다. 시스템 요구 사항을 포함한 KV 스토어에 대한 자세한 내용은 Splunk Enterprise 관리자 매뉴얼에서 앱 key value 스토어를 참조하십시오. Splunk Enterprise Security는 일부 룩업 파 일을 KV 스토어에 저장합니다. 검색헤드 클러스터 환경에서는 큰 KV 스토어 룩업을 클러스터 구성원 간에 동기화하는 작업 에 실패하여 KV 스토어가 동기화되지 않을 수 있습니다. 작업 로그 크기를 늘려 이 문제를 완화할 수 있습니다. Splunk Enterprise 관리자 매뉴얼에서 작업 로그 크기를 늘려 동기화되지 않은 구성원 방지를 참조하십시오.

Splunk Enterprise Security는 검색헤드 풀링을 지원하지 않습니다.

### Splunk Enterprise Security와 검색헤드 클러스터링

Splunk Enterprise Security는 Linux 기반 검색헤드 클러스터에만 설치할 수 있습니다. 현재 Windows 검색헤드 클러스터 는 Splunk Enterprise Security에서 지원되지 않습니다.
검색헤드 클러스터는 인덱서의 검색 부하를 늘립니다. 검색헤드 클러스터를 구현하는 경우 인덱서를 더 많이 추가하거나 인덱서에 추가 CPU 코어를 배정하십시오. Splunk Enterprise 분산 검색 매뉴얼의 검색헤드 클러스터에 대한 시스템 요구 사항 및 기타 배포 고려 사항과 분산 검색 매뉴얼의 검색헤드 클러스터링 아키텍처를 참조하십시오.

### Splunk Enterprise Security용 검색헤드 확장 고려사항

|요인|이 사양 확장|
|:--:|:--:|
|많은 동시 검색 수|CPU 코어 증설 RAM 증설|
|많은 수의 실시간 검색 실행 동시에 로그인하는 많은 사용자 수|CPU 코어 증설|
|많은 수의 상관(correlation)검색 활성화|RAM 증설|
|큰 자산 및 ID 룩업 파일|RAM 증설|

### Splunk Enterprise Security용 인덱서 확장 고려 사항

검색 부하와 동시 검색이 증가하면 배포의 인덱서 수를 늘려서 확장합니다. 인덱서 컬렉션 하나를 2개 이상의 검색헤드가 사 용할 수 있으므로, 동일한 인덱서를 Enterprise Security를 호스트하는 검색헤드로 사용하는 추가 검색헤드가 있으면 인덱 서 계층의 전체 성능이 저하되고 Enterprise Security에 제공되는 리소스가 감소합니다.

Splunk 플랫폼은 인덱서를 사용하여 수평으로 확장합니다. Enterprise Security 배포에 필요한 인덱서 수는 데이터 용량, 데 이터 유형, 보존 요구 사항, 검색 유형, 검색 동시성에 따라 다릅니다.

하루에 1테라바이트(1TB) 이상의 데이터를 Enterprise Security에 수집할 계획인 경우, Splunk 프로페셔널 서비스와 협력 하여 배포 아키텍처 견적을 받으십시오.

#### 성능 테스트 결과

다음 성능 테스트 결과를 검토하여 Splunk 플랫폼과 Enterprise Security 배포의 데이터 배합에 따라 기대할 수 있는 인프라 성능을 더 정확하게 추정하십시오. 다음 성능 테스트에 사용된 인덱서는 RAM이 32GB고 CPU 코어가 16개인 참조 하드웨 어와 일치합니다.

Splunk Enterprise Security의 크기를 지정할 때 두 가지 큰 요인을 고려해야 합니다.

- 배포에서 활성화된 상관(correlation)검색의 수 및 지원 검색의 수를 기반으로 한 상관(correlation)검색헤드.
- 가속되는 데이터 모델의 수, 모델링되는 데이터의 유형, 모델링되는 데이터의 카디널리티 및 가속되는 데이터 볼륨을 기반으로 한 데이터 모델 가속 로드.

데이터 배합에 따라, 확장 용량은 인덱서당 40GB에서 100GB에 이를 수 있으며, 그렇지 않으면 데이터 모델 가속이 데이터 수집 이후로 지연될 수 있습니다.

일반적인 데이터 모델 데이터 배합을 사용한 시나리오에 근거한 이 테스트에서는 각 인덱서가 데이터 모델 가속 및 UI 응답 성 대기 시간을 낮게 유지하면서 하루 100GB의 데이터 수집 속도를 유지하는 것으로 나타났습니다.

<table>
<tr><td colspan=6>데이터 모델 데이터 배합 테스트</td></tr>
<tr><td>데이터 유형</td><td colspan=5>4개의 데이터 모델에서 비율이 서로 다른 데이터 원본을 선택하여 수집하고 가속했습니다.

- 웹: 전체 데이터의 65%
- 네트워크 트래픽: 전체 데이터의 25%
- 변경 사항 분석: 전체 데이터의 1%
- 인증: 전체 데이터의 1%. 나머지 백분율은 어떤 데이터 모델에도 해당하지 않음

</td></tr>
<tr><td>검색 부하</td><td colspan=5>상관(correlation)검색 60개가 활성화됨. 사용자 부하가 더 추가되지 않음</td></tr>
<tr><td>확장 결과: 데이터 용량</td><td>100</td><td>300</td><td>500</td><td>800</td><td>1000</td></tr>
<tr><td>확장 결과: 인덱서 수</td><td>1</td><td>3</td><td>5</td><td>8</td><td>10</td></tr>
</table>

Enterprise Security에 카디널리티가 높거나 매우 고유한 데이터가 포함된 데이터 모델 하나의 데이터만 있는 시나리오에 근 거한 이 테스트에서는 각 인덱서가 데이터 모델 가속 및 UI 응답성 대기 시간을 낮게 유지하면서 하루 40GB의 데이터 수집 속도를 유지하는 것으로 나타났습니다.

<table>
<tr><td colspan=6>테스트 시나리오: 데이터 모델 1개 최대 사용</td></tr>
<tr><td>데이터 유형</td><td colspan=5>네트워크 트래픽 데이터 모델의 데이터만 수집 및 가속</td></tr>
<tr><td>검색 부하</td><td colspan=5>상관(correlation)검색 60개가 활성화됨. 사용자 부하가 더 추가되지 않음</td></tr>
<tr><td>확장 결과: 데이터 용량</td><td>40</td><td>120</td><td>200</td><td>320</td><td>400</td></tr>
<tr><td>확장 결과: 인덱서 수</td><td>1</td><td>3</td><td>5</td><td>8</td><td>10</td></tr>
</table>

#### 인덱서 클러스터링 지원

Splunk Enterprise Security는 단일 사이트 및 멀티 사이트 인덱서 클러스터 아키텍처를 모두 지원합니다. 인덱서와 인덱서 클러스터 관리에서 인덱서 클러스터 아키텍처와 멀티 사이트 클러스터 아키텍처의 기본 사항을 참조하십시오.

단일 사이트 또는 멀티 사이트 인덱서 클러스터 아키텍처에는 검색헤드 하나 또는 Enterprise Security 실행 인스턴스가 있 는 검색헤드 클러스터 하나가 포함될 수 있습니다. 추가적인 단일 인스턴스 검색헤드는 Enterprise Security를 실행할 수 없습니다.

멀티 사이트 인덱서 클러스터 아키텍처에 대해, Splunk는 다음을 권장합니다.

- 요약 복제를 활성화합니다. 인덱서 및 인덱서 클러스터 관리에서 복제된 요약을 참조하십시오. - Enterprise Security 검색헤드를 site0으로 설정하여 인접노드 검색을 비활성화합니다. 인덱서 및 인덱서 클러스터 관리에서 인접노드 검색 비활성화를 참조하십시오.

인덱서 클러스터링을 사용하는 경우, 앱과 설정 파일을 인덱서 피어에 배포하는 데 사용하는 방법이 다릅니다. 인덱서 및 인 덱서 클러스터 관리에서 모든 클러스터 피어에서 공통 설정 관리와 모든 클러스터 피어에서 앱 배포 관리를 참조하십시오.

#### 데이터 모델 가속

Splunk Enterprise Security는 데이터 모델을 가속하여 대시보드, 패널 및 상관(correlation)검색 결과를 제공합니다. 데이 터 모델 가속에서는 인덱서를 처리와 저장에 사용하여 가속된 데이터를 각 인덱스에 저장합니다.

데이터 모델 가속의 성능을 높이고 특히 인덱서 부하의 규모를 줄이려면 특정 데이터 모델의 데이터 모델 가속을 특정 인덱스로 제한하십시오. 데이터 모델을 특정 인덱스로 제한하는 방법에 대한 자세한 내용은 Splunk CIM(Common Information Model) 추가 기능 설정을 참조하십시오.

데이터 모델 가속에 필요한 추가 저장 공간을 계산하는 방법은 데이터 모델 가속 저장 및 보존을 참조하십시오.

#### 인덱스 TSIDX 축소 호환성

Splunk Enterprise 6.4.x에서는 인덱스의 TSIDX 파일에 대한 보존 정책을 사용할 수 있습니다. 자세한 내용은 Splunk Enterprise 인덱서 및 인덱서 클러스터 관리 매뉴얼에서 tsidx 디스크 사용량 줄이기를 참조하십시오. TSIDX 파일 보존 정 책 설정은 데이터 모델 가속 보존에 영향을 미치지 않습니다.

Enterprise Security와 함께 제공되는 일부 검색은 축소된 TSIDX 파일이 있는 버킷에 사용할 수 없습니다.

|패널/검색 이름|기본 시간 범위|해결 방법|
|:--:|:--:|:--:|
|포워더 감사 패널: 호스트별 시간에 따른 이벤트 수|-30d|TSIDX 보존을 시간 범위보다 큰 값으로 설정|
|저장된 검색: 감사 - 상위 10개 호스트별 시간에 따른 이벤트 수|-30d|TSIDX 보존을 시간 범위보다 큰 값으로 설정|
|저장된 검색: 감사 - 일별 이벤트 수 - 룩업 생성|-1d|TSIDX 보존을 기본 시간 범위보다 큰 값으로 설정|
|저장된 검색: 엔드포인트 - 인덱스 시간 델타 2 - 요약 생성|-1d|TSIDX 보존을 기본 시간 범위보다 큰 값으로 설정|

### 배포 서버를 Splunk Enterprise Security와 함께 사용

Splunk Enterprise Security에는 앱과 추가 기능이 포함되어 있습니다. 이런 앱이나 추가 기능을 배포 서버에서 관리하는 경 우, Enterprise Security 설치가 완료되지 않습니다.

- Splunk Enterprise Security에 포함된 추가 기능은 분산 설정 관리 도구를 사용하여 배포합니다. 이 매뉴얼에서 Splunk Enterprise Security에 포함된 추가 기능 배포를 참조하십시오.
- 환경에 설치된 다른 앱과 추가 기능은 적절할 경우 배포 서버를 사용하여 배포합니다. Splunk Enterprise 인스턴스 업 데이트에서 배포 서버와 포워더 관리를 참조하십시오.

Enterprise Security 패키지에 포함된 추가 기능이 배포 서버에서 관리되는 경우, Enterprise Security를 설치하기 전에 배포 클라이언트 설정을 제거하십시오.

1. 배포 서버에 대한 참조가 포함된 deploymentclient.conf파일을 제거합니다.
2. Splunk 서비스를 다시 시작합니다.

### 가상화된 하드웨어

Splunk Enterprise Security를 가상화된 환경에 설치하는 경우, 가상화되지 않은 베어메탈 환경과 동일한 메모리 및 CPU 배정량이 필요합니다.

- 모든 CPU 및 메모리 리소스를 예약합니다.
- 하드웨어를 초과 예약하지 않습니다.
- 모든 Splunk 플랫폼 인덱서 노드에 걸쳐 저장소 IOPS를 동시에 테스트하여 IOPS가 환경에서 사용하는 참조 하드웨 어 사양과 일치하는지 확인합니다. 용량 계획 매뉴얼에서 참조 하드웨어를 참조하십시오.

불충분한 저장 성능은 Splunk 플랫폼을 가상화된 환경에서 확장하는 경우에 열악한 검색 응답과 시간 초과의 일반적인 원인 입니다.

VMware 설정 세부 정보는 Splunk 리소스에서 제공되는 기술 적요: "가상 환경 내 Splunk 배포: Splunk를 실행하도록 VMware 가상 머신 설정"을 참조하십시오.

### 모니터링  콘솔

Enterprise Security 검색헤드에서 모니터링 콘솔을 활성화하는 경우 독립형 모드로 유지해야 합니다. 분산 환경에서 모니 터링 콘솔을 설정하는 경우와 방법에 대한 자세한 내용은 Splunk Enterprise 모니터링에서 어떤 인스턴스가 콘솔을 호스트 해야 합니까?를 참조하십시오.

### Enterprise Security와 다른다른 앱의 호환성

Enterprise Security는 추가 기능에서 제공하는 검색 지식과 CIM(Common Information Model) 지원에 의존합니다. 추가 기능은 보안 데이터를 CIM과 사용할 수 있게 최적화하고 정규화하고 분류하는 데 필요한 이벤트 처리를 정의하는 책임을 집니다. CIM 호환 앱만 Splunk Enterprise Security와 호환 가능합니다. CIM과 호환되지 않는 기타 앱과 추가 기능에는 CIM 에 정규화되지 않아 해당 필드에 의존하는 검색 및 대시보드가 올바르게 작동하지 않는 원인이 되는 데이터 지식이 포함될 수 있습니다.

Splunk Enterprise Security와 VMware용 Splunk 추가 기능의 SA-VMNetAppUtils 구성 요소는 한 검색헤드에 설치할 수 없습니다. 이름이 동일한 파일이 충돌하여 Splunk Enterprise Security의 일부가 올바로 작동하지 않을 수 있습니다.

## Splunk Enterprise Security 데이터 원본 계획

데이터 원본의 용량, 유형 및 수는 전체 Splunk 플랫폼 아키텍처, 포워더 수와 위치, 추정 부하, 네트워크 리소스에 영향을 미 칩니다.

Splunk Enterprise Security는 모든 데이터 원본이 Splunk CIM(Common Information Model)과 호환되어야 한다고 요구 합니다. Enterprise Security는 대시보드 패널 및 뷰를 채울 데이터를 검색하는 경우와 상관(correlation)검색을 위한 데이터 를 제공하는 경우에 모두 CIM 표준화 데이터 모델을 활용하도록 설계되었습니다.

### 추가 기능을 데이터 원본에 매핑

Splunk Enterprise Security에 포함된 추가 기능은 알려진 데이터 원본과 CIM 준수를 위한 기타 기술을 파싱하고 분류하도 록 설계되었습니다.

각 데이터 원본에 대해,

1. 추가 기능을 식별합니다. 기술을 식별하고 해당 추가 기능을 결정하십시오. 추가 기능의 주요 출처는 Enterprise Security와 함께 제공되는 기술별 추가 기능과 Splunkbase에서 제공되는 CIM 호환 콘텐츠입니다. 사용하고 싶은 추 가 기능이 CIM과 아직 호환되지 않는 경우, CIM 데이터 스키마를 지원하도록 추가 기능을 수정하십시오. 예는 CIM(Common Information Model) 추가 기능 매뉴얼에서 CIM을 사용하여 검색 시간에 데이터 정규화를 참조하십시 오.
2. 추가 기능을 설치합니다. Enterprise Security 검색헤드에 추가 기능을 설치하십시오. 인덱스 시간 처리를 수행하는 추가 기능을 각 인덱서에 설치하십시오. 포워더 아키텍처에서 파싱 또는 헤비 포워더를 통해 데이터를 전송하는 경우, 헤비 포워더에 추가 기능이 필요할 수 있습니다. Splunk Cloud 고객은 Splunk 서포트와 협력하여 추가 기능을 검색헤드와 인덱서에 설치해야 하나, 사내 포워더는 직접 책임집니다.
3. 필요한 경우 서버, 장치 또는 기술을 설정합니다. 장치 또는 애플리케이션에 대해 로깅 또는 데이터 수집을 활성화하고 (활성화하거나) Splunk 인스턴스에서 수집하는 데 사용할 출력을 설정하십시오. 구현 절차는 공급업체 매뉴얼을 참조 하십시오.
4. 필요한 경우 추가 기능을 사용자 지정합니다. 데이터 위치 또는 원본 설정, 데이터 위치(파일 또는 데이터베이스) 선택, 또는 기타 고유 설정 등 추가 기능을 사용자 지정해야 할 수 있습니다.
5. Splunk 데이터 입력을 설정하고 source type 설정을 확인합니다. 추가 기능의 README 파일에는 데이터와 관련된 source type 설정에 대한 정보가 포함되고, 입력 설정에 대한 사용자 지정 참고 사항이 포함될 수 있습니다.

#### 데이터 입력 고려 사항

Splunk 플랫폼 인스턴스는 데이터 수집을 위한 몇 가지 유형의 입력 설정을 제공합니다. 기술이나 수집하는 원본에 따라, 인 프라 요구 사항과 일치하는 입력 방법을 성능 영향, 데이터 액세스 용이성, 안정성, 원본 대기 시간 최소화 및 유지관리 용이 성을 기준으로 선택하십시오.

- 파일파일 모니터링 : 파일을 호스트하는 각 시스템에 Splunk 포워더를 배포하고, 포워더에서 입력 설정을 사용하여 source type을 설정합니다. 파일이 동일한 시스템이 여러 개 있는 경우, Splunk Enterprise 배포 서버를 사용하여 큰 포워더 그룹에 걸쳐 표준화된 파일 입력을 설정하십시오.
- 네트워크  포트 모니터링 : syslog 서버 같은 표준 도구를 사용하거나 수신 포트를 포워더에 만듭니다. 여러 네트워크 source를 동일한 포트 또는 파일로 전송하면 source type 설정이 복잡해집니다. 자세한 내용은 Splunk 플랫폼 매뉴얼 을 참조하십시오.
  - Splunk Enterprise의 경우, Splunk Enterprise 데이터 가져오기에서 TCP 및 UDP 포트에서 데이터 가 져오기를 참조하십시오.
  - Splunk Cloud의 경우,Splunk Cloud 데이터 가져오기에서 TCP 및 UDP 포트에서 데이터 가져오기를 참조하십시오.
- Windows 데이터 모니터링 모니터링: 포워더는 다양한 설정 옵션을 사용하여 Windows 호스트에서 정보를 가져올 수 있습니다. 자세한 내용은 Splunk 플랫폼 매뉴얼을 참조하십시오.
  - Splunk Enterprise의 경우, Splunk Enterprise 데이터 가져오기에서 Splunk Enterprise로 Windows 데 이터를 가져오는 방법을 참조하십시오.
  - Splunk Cloud의 경우, Splunk Cloud 데이터 가져오기에서 Splunk Enterprise로 Windows 데이터를 가 져오는 방법을 참조하십시오.
- 네트워크 네트워크 유선유선 데이터 모니터링 모니터링: Splunk Stream은 실시간 유선 데이터 캡처를 지원합니다. Splunk Stream 설치 및 설정 매뉴얼에서 Splunk Stream을 참조하십시오.
- 스크립트 기반 입력 : 스크립트 기반 입력을 사용하여 API 또는 기타 원격 데이터 인터페이스와 메시지 대기열에서 데 이터를 가져옵니다. 쉘 스크립트, python 스크립트,Windows 배치 파일, PowerShell 또는 인덱싱할 데이터를 포맷하 거나 스트리밍할 수 있는 기타 유틸리티를 호출하도록 포워더를 설정하십시오. 스크립트에 의해 폴링되는 데이터를 파일에 기록하여 포워더에서 직접 모니터링할 수도 있습니다. 자세한 내용은 Splunk 플랫폼 매뉴얼을 참조하십시오.
  - Splunk Enterprise의 경우, Splunk Enterprise 데이터 가져오기에서 스크립트 기반 입력을 통해 API 및 기타 원격 데이터 인터페이스에서 데이터 가져오기를 참조하십시오. - Splunk Cloud의 경우, Splunk Cloud 데이터 가져오기에서 스크립트 기반 입력을 통해 API 및 기타 원격 데이터 인터페이스에서 데이터 가져오기를 참조하십시오.

### 자산자산 및 ID 정보정보 수집수집

Splunk Enterprise Security는 자산 및 ID 데이터를 Splunk 플랫폼의 이벤트와 비교하여 분석을 위한 데이터 보강 및 추가 컨텍스트를 제공합니다. 자산 및 ID 정보를 수집하고 Splunk Enterprise Security에 추가하여 데이터 보강을 이용하십시오. Splunk Enterprise Security 관리에서 Splunk Enterprise Security에 자산 및 ID 데이터 추가를 참조하십시오.