# Hibernate 사용시 주의해야하는 설정

- hbm2ddl.auto 설정은 hibernate에 포함된 매핑 설정으로 Database 스키마 생성 스크립트를 만드는데 사용
- 시작시마다 Mapping 설정대로 Schema를 세팅할 수 있는데 설정 값에 따라 다음과 같이 동작

  - Create : Session Factory 가 실행될 때 스키마를 지우고 다시 생성. 생성 후에는 classpath에 import.sql 파일이 있는지 찾아 등록된 쿼리문을 실행
  - Create-drop : 기본적으로 create와 동일하지만 session 팩토리가 내려갈 때 db 스키마를 삭제
  - Update ; 도메인 객체구성과 스키마를 비교해서 도메인 객체에 맞춰서 db 스키마를 변경
  - Validate : 도메인 객체구성과 db스키마가 같은지만 확인할 뿐 변경하지 않음 다를 시 session Factory가 시작시 확인하고 예외를 발생

다음은 Hibernate 사용시 알아두어야 하는 JPA 프로퍼티들

<table>
<tr><td>spring.data.jpa.repositories.enabled</td><td>true</td><td>JPA 저장소 활성화 여부</td></tr>
<tr><td>spring.jpa.database</td><td></td><td>대상 데이터베이스(기본적으로 자동탐지됨. 또는 "databasePlatform" 프로퍼티로 설정 가능</td></tr>
<tr><td>spring.jpa.database-platform</td><td></td><td>대상 데이터베이스의 이름(기본적으로 자동탐지됨. 또는 "Database" enum으로 설정 가능</td></tr>
<tr><td>spring.jpa.generate-ddl</td><td>false</td><td>시작시 스키마 초기화 여부</td></tr>
<tr><td>spring.jpa.hibernate.ddl-auto</td><td></td><td>DDL모드. 실제로는 "hibernate.hbm2ddl.auto" 프로퍼티의 바로가기임. 기본값은 내장 데이터베이스 사용시 "create-drop" 아니면 "none"</td></tr>
<tr><td>spring.jpa.hibernate.naming-strategy</td><td></td><td>FQN 네이밍 전략</td></tr>
<tr><td>spring.jpa.open-in-view</td><td>true</td><td>OpenEntityManagerInViewInterceptor 등록. 요청처리하는 스레드에 JPA EntityManager를 연결</td></tr>
<tr><td>spring.jpa.properties.*</td><td></td><td>JPA 프로바이더를 설정하는 추가 네이티브 프로퍼티</td></tr>
<tr><td>spring.jpa.jpa.show-sql</td><td>false</td><td>SQL문장의 로깅 활성화 여부</td></tr>
</table>
