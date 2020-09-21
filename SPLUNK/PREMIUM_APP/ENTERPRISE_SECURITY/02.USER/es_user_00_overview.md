® Splunk
Enterprise Security 5.0.0
Splunk Enterprise Security 사용사용
생성일: 2018-02-06 오후 12:05
Copyright (c) 2018 Splunk Inc. All Rights Reserved
Table of Contents
소개소개
Splunk Enterprise Security 소개
인시던트 인시던트 검토검토
Splunk Enterprise Security의 인시던트 검토 개요 Splunk Enterprise Security의 인시던트 검토에서 주요 이벤트 분류 Splunk Enterprise Security의 인시던트 검토에서 주요 이벤트 조사 Splunk Enterprise Security의 인시던트 검토에서 주요 이벤트에 대한 작업 수행 Splunk Enterprise Security에 포함된 adaptive response 작업 Splunk Enterprise Security에서 긴급도가 주요 이벤트에 배정되는 방법
조사조사
Splunk Enterprise Security의 조사 Splunk Enterprise Security에서 조사 시작 Splunk Enterprise Security의 조사 워크벤치에서 잠재적인 보안 인시던트 조사 Splunk Enterprise Security에서 조사에 세부 정보 추가 Splunk Enterprise Security에서 조사 변경 Splunk Enterprise Security에서 조사에 협력 Splunk Enterprise Security에서 조사 검토 Splunk Enterprise Security에서 조사 공유 또는 인쇄 Splunk Enterprise Security에서 작업 이력 참조 Splunk Enterprise Security에서 조사 요약 검토
위험위험 분석분석
Splunk Enterprise Security의 위험 분석 Splunk Enterprise Security에서 임시 위험 항목 작성
글래스 글래스 테이블
테이블 Splunk Enterprise Security에서 글래스 테이블 만들기 Splunk Enterprise Security에서 글래스 테이블 관리
대시보드 대시보드 개요개요
Splunk Enterprise Security에서 사용 가능한 대시보드 소개 이용 사례에 맞게 Splunk Enterprise Security 대시보드 사용자 지정 Splunk Enterprise Security의 핵심 지표
대시보드 대시보드 참조참조
보안 포스처 대시보드 감사(audit) 대시보드 예측 분석 대시보드
4 4
5 5 5 7 7
9 11
12 12 12 13
15 17 18 19 19 19 20
21 21 23
24 24 25
27 27 28 28
31 31 31 36
액세스 대시보드 Endpoint 대시보드 자산 및 ID 대시보드 Asset 및 Identity Investigator 대시보드 사용자 작업 모니터링 위험 분석 네트워크 대시보드 웹 센터 및 네트워크 변경 사항 대시보드 포트 및 프로토콜 추적기 대시보드 프로토콜 인텔리전스 대시보드 위협 인텔리전스 대시보드 웹 인텔리전스 대시보드
포함된 포함된 추가추가 기능기능
Enterprise Security에서 Splunk UBA 데이터 보기
37 41 46 48 51 52 53 57 58 59 63 65
70 70
소개소개
Splunk Enterprise Security 소개소개
Splunk Enterprise Security는 현재 기업 인프라에서 발견되는 보안 관련 위협을 볼 수 있는 가시성을 보안 실무자에게 부여 합니다. Splunk 운영 인텔리전스 플랫폼을 기반으로 제작되고 검색 및 상관 기능을 사용하는 Splunk Enterprise Security를 통해, 사용자는 보안 장치, 시스템 및 애플리케이션의 데이터를 캡처하고 모니터링하고 보고할 수 있습니다. 문제가 확인되 면, 보안 애널리스트는 전체 액세스, endpoint 및 네트워크 보호 도메인에서 보안 위협을 신속히 조사하고 해결할 수 있습니 다.
Splunk Enterprise Security 액세스 액세스
1. 웹 브라우저를 열고 Splunk Web으로 이동합니다. 2. 사용자 이름 및 암호를 사용하여 로그인합니다. 3. 앱 리스트에서 Enterprise Security를 클릭합니다.
시작시작
Splunk Enterprise Security에서 일반적인 애널리스트 워크플로를 시작합니다.
사용 가능한 대시보드의 개요 및 사용 방법은 Splunk Enterprise Security에서 사용 가능한 대시보드 소개를 참조하 십시오. 주요 이벤트를 사용하는 방법에 대해 알아보려면 Splunk Enterprise Security에서 인시던트 검토 개요를 참조하십시 오. 조사에서 작업을 추적하는 데 대한 소개는 Splunk Enterprise Security의 조사를 참조하십시오, Splunk Enterprise Security가 개체에 위험을 배정하는 방법을 알아보려면 Splunk Enterprise Security의 위험 분석 을 참조하십시오. 글래스 테이블 시각화를 만들고 사용하는 방법을 알아보려면 Splunk Enterprise Security에서 글래스 테이블 만들기 를 참조하십시오.
Splunk Enterprise Security 관리자인 경우, Splunk Enterprise Security 관리에서 관리자 워크플로 관련 매뉴얼을 참조하 십시오.