# Splunk Stream Overview

## Splunk Stream 정보

Splunk Stream을 사용하면 네트워크 이벤트 데이터 스트림을 캡처, 필터링, 인덱싱 및 분석 할 수 있습니다.

"스트림"은 특정 네트워크 프로토콜 및 필드 집합에 의해 정의 된 이벤트 그룹입니다. 로그, 메트릭 및 기타 정보와 결합 할 경우 Splunk Stream으로 캡처 한 스트림은 네트워크 인프라 전반의 활동 및 의심스러운 동작에 대한 귀중한 통찰력을 제공 할 수 있습니다.

Splunk Stream을 사용하여 다음을 수행하십시오.

- 네트워크 이벤트 데이터의 라이브 스트림을 수동적으로 캡처
- 여러 네트워크 프로토콜에 대한 메타 데이터 및 전체 패킷 스트림 캡처
- NetFlow 프로토콜 데이터 수집
- 이벤트 데이터의 통계 분석을위한 집계 방법 적용
- 인덱서 요구 사항 최소화를위한 필터 적용
- 문자열에서 콘텐츠 추출 및 해시 생성
- 네트워크 트래픽에서 파일 추출
- 사전 구축 된 대시보드에서 네트워크 추세 및 앱 성능 모니터링
- 독립적 인 Stream Forwarder를 배포하여 원격 Linux 시스템에서 데이터를 캡처합니다.
- 태깅이나 계측이 필요없이 빠르고 눈에 잘 띄지 않게 확장
- Splunk Stream 구성 요소에 대한 자세한 개요는 Splunk Stream 설치 패키지 개요를 참조하십시오 .

Splunk Stream을 사용하여 네트워크 메타 데이터 및 전체 네트워크 패킷 캡처를 시작하려면 Splunk Stream 사용 설명서의 스트림 구성을 참조하십시오 .

## Splunk Stream 구성 요소

Splunk Stream을 배포하려면 기존 Splunk Enterprise 인스턴스 및 / 또는 호환되는 Linux 시스템에 3 개의 Stream 패키지를 설치하십시오.

Splunk App for Stream, 패키지 splunk_app_stream
Splunk Add-on for Stream Forwarders, 패키지 Splunk_TA_stream
Splunk Add-on for Stream Wire Data, 패키지 Splunk_TA_stream_wire_data

Splunk Stream은 Independent Stream Forwarder를 배포하는 기능도 제공합니다. 이것은 Splunk App for Stream 설치 내에서 바이너리 파일 `<streamfwd>`로 패키징됩니다.

Splunk Stream 구성 요소에 대한 자세한 내용 은이 설명서의 Splunk Stream 설치 패키지 개요 를 참조하십시오 .

## 지원되는 Splunk 소프트웨어 구성

Splunk Stream은 대부분의 Splunk 소프트웨어 구성을 지원합니다.

- Splunk Cloud 배포 .
- 배포 서버 및 인덱서 클러스터를 포함한 분산 배포 구성
- Splunk Enteprise의 단일 인스턴스가 인덱서이자 검색 헤드 인 단일 인스턴스 배포
- 호환되는 Linux 시스템의 독립 스트림 포워더 .

## Splunk Stream 설치 패키지 개요

Splunk Stream 버전 7.3부터 Splunk Stream은 각각 다운로드 및 설치해야하는 세 개의 패키지로 구성됩니다.

<table>
<tr><td>상품명</td><td>설치 패키지 이름</td><td>설치된 파일 이름</td><td>위치</td><td>기술</td></tr>
<tr><td>Splunk App for Stream</td><td>splunk_app_stream</td><td>splunk_app_stream/</td><td>Search Header
 </td><td>이 패키지를 검색 헤드에 설치하면 다음이 제공됩니다.<br/>
- 구성 사용자 인터페이스<br/>
- 네트워크 이벤트 및 흐름 데이터 분석을 위한 컨테이너 대시보드 및 대시보드<br/>
- 데이터 캡처를 미세 조정하기위한 필터</td></tr>
<tr><td>Splunk Add-on for Stream Forwarders</td><td>Splunk_TA_stream</td><td>Splunk_TA_stream/</td><td>Heavy Forwarder</td><td>이 패키지를 Splunk 포워더에 설치하고이를 사용하여 유니버설 포워더를 확장
검색 헤드 및 인덱서에 배포하여 로컬 트래픽을 수집
Splunk App for Stream과 동일한 서버에 Stream forwarder를 설치하면 사용자 인터페이스에서 PCAP 데이터를 업로드
</td></tr>
<tr><td>Splunk Add-on for Stream Wire Data</td><td>Splunk_TA_stream_wire_data</td><td>Splunk_TA_stream_wire_data/</td><td>Search Header
Indexer
Heavy Forwarder</td><td>이 패키지를 검색 헤드에 설치하면 검색 및 인덱싱 시 필요한 모든 지식 개체가 제공
분산 배포에서 패키지는 검색 헤드 및 인덱서에 배포 할 수 있으며 구성에있는 경우 Heavy 포워더에 배포
</td></tr>
</table>
