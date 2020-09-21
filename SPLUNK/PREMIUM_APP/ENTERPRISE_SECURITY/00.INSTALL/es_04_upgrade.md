업그레이드 업그레이드
Splunk Enterprise Security 업그레이드 업그레이드 계획계획
사내 Splunk Enterprise Security 업그레이드를 계획합니다. Splunk Cloud 고객은 Splunk 서포트와 협력하여 Enterprise Security 업그레이드를 조율해야 합니다.
4.5.x 버전 이상에서만 이 Splunk Enterprise Security 버전으로 업그레이드할 수 있습니다. 더 오래된 버전에서 업그레이드 하려면 중간 업그레이드를 수행하십시오.
Splunk Enterprise Security를 업그레이드하기 업그레이드하기 전에전에
1. 호환 가능한 Splunk 플랫폼 버전을 검토합니다. Splunk Enterprise 시스템 요구 사항을 참조하십시오. 2. 하드웨어 요구 사항을 검토하여 서버 하드웨어가 Splunk Enterprise Security를 지원하는지 확인합니다. 하드웨어 요
구 사항을 참조하십시오. 3. 최신 Splunk Enterprise Security 릴리스의 알려진 문제를 검토합니다. Splunk Enterprise Security 릴리스 노트에서
알려진 문제를 참조하십시오. 4. 최신 Splunk Enterprise Security 릴리스에서 지원이 중단된 기능을 검토합니다. Splunk Enterprise Security 릴리스
노트에서 지원이 중단된 기능을 참조하십시오. 5. 검색 헤드를 백업합니다(KV 스토어 포함). 이 업그레이드 프로세스에서는 업그레이드 전에 기존 설치본을 백업하지
않습니다. 검색 헤드의 KV 스토어를 백업하는 방법에 대한 설명은 KV 스토어 백업을 참조하십시오.
Splunk Enterprise Security 업그레이드 업그레이드 권장권장 사항사항
Splunk 플랫폼을 현재 Splunk Enterprise Security 버전과 호환되지 않은 버전으로 업그레이드하려면 Splunk 플랫폼과 Splunk Enterprise Security를 모두 동일한 유지보수 기간에 업그레이드하십시오.
Splunk 플랫폼과 Splunk Enterprise Security를 동시에 업그레이드할 수 없는 경우, Splunk Enterprise와 Splunk Enterprise Security의 호환 가능 버전을 검토하여 업그레이드 경로를 결정하십시오.
1. (선택 사항) 필요한 경우, Splunk Enterprise를 호환 가능한 버전으로 업그레이드합니다. Splunk Enterprise 설치 매
뉴얼에서 분산 Splunk Enterprise 환경 업그레이드를 참조하십시오. 2. Splunk 플랫폼 인스턴스를 업그레이드합니다. 3. Splunk Enterprise Security를 업그레이드합니다. 4. 추가 기능을 검토 및 업그레이드하고 배포합니다.
검색 헤드 클러스터에 배포된 Enterprise Security를 업그레이드하는 프로세스에는 여러 단계가 수반됩니다. 권장되는 절차 는 검색 헤드 클러스터에서 Enterprise Security 업그레이드에 자세히 설명되어 있습니다.
업그레이드 업그레이드 관련관련 참고참고 사항사항
배포 서버가 Enterprise Security 패키지에 포함된 앱이나 추가 기능을 관리하는 경우 업그레이드가 실패합니다. 업그 레이드를 시작하기 전에 배포 서버 참조가 포함된
deploymentclient.conf
파일을 제거하고 Splunk 서비스를 다시 시작 하십시오. 업그레이드하면 앱의
/local
및
/lookups
경로에 저장된 설정 변경 사항과 파일이 상속됩니다. 로컬에서 적용된 메뉴 탐색 변경 사항은 업그레이드 후에도 유지됩니다. 업그레이드 프로세스를 통해 상속된 설정 변경 사항이 업그레이드 후에 새 설정에 영향을 미치거나 새 설정을 재정의 할 수 있습니다. ES 설정 상태 대시보드를 사용하여 새 설정과 상충될 수 있는 구성 설정을 검토하십시오. 사용자 매뉴 얼에서 ES 설정 상태를 참조하십시오. 업그레이드 프로세스는
$SPLUNK_HOME/var/log/splunk/essinstaller2.log
로그에 기록됩니다.
Splunk Enterprise Security에 포함된 추가 기능의 업그레이드에 관한 참고 사항:
업그레이드 프로세스에서는 앱 및 추가 기능의 이전 및 기존 버전을 모두 덮어씁니다. 환경에 설치된 더 새로운 앱 또는 추가 기능 버전은 업그레이드 중에 덮어쓰지 않습니다. 이전 버전에서 비활성화한 앱 또는 추가 기능은 업그레이드 후에 비활성화된 상태로 유지됩니다. 업그레이드하면 지원이 중단된 앱이나 추가 기능이 비활성화됩니다. 지원이 중단된 앱 또는 추가 기능은 Enterprise Security 설치본에서 수동으로 제거해야 합니다. 업그레이드 후에는 지원이 중단된 항목을 모두 식별하는 경고가 메시 지에 표시됩니다.
추가추가 기능기능 변경변경 사항사항
이 Enterprise Security 릴리스에 포함된 추가 기능의 리스트는 Enterprise Security와 함께 제공되는 기술별 추가 기능을 참 조하십시오.
분산된 분산된 추가추가 기능기능 업그레이드
업그레이드 25
Splunk Enterprise Security에는 기본 제공 추가 기능의 최신(이 버전의 릴리스 시점 기준) 버전이 포함되어 있습니다.
최신 추가 기능의 복사본이 Splunk Enterprise Security에 포함되어 있습니다. Enterprise Security를 업그레이드할 때 모 든 추가 기능을 검토하고 업데이트된 추가 기능을 인덱서와 포워더에 필요에 따라 배포하십시오. 인덱서나 포워더에 배포된 설정은 Enterprise Security 설치 프로세스 중에 자동으로 업그레이드 또는 마이그레이션되지 않습니다. Splunk Enterprise Security에 포함된 추가 기능 배포를 참조하십시오.
이전 추가 기능 버전에 적용된 사용자 지정은 수동으로 마이그레이션해야 합니다.
Splunk Enterprise Security 업그레이드 업그레이드 이 항목에서는 사내 검색 헤드의 Splunk Enterprise Security를 4.0 버전 이상에서 최신 릴리스로 업그레이드하는 방법에 대해 설명합니다. Splunk Cloud 고객은 Splunk 서포트와 협력하여 Enterprise Security 업그레이드를 조율합니다.
1 단계단계. 계획계획 항목항목 검토검토
1. 업그레이드 프로세스와 전제 조건의 개요는 이 매뉴얼에서 업그레이드 계획을 참조하십시오. 2. 업그레이드하기 전에 검색 헤드 전체 백업을 수행합니다.
업그레이드를 취소하려면 백업본을 사용하여 이전 Splunk Enterprise Security 버전을 복원해야 합니다.
2 단계단계. Splunk Enterprise Security 다운로드
다운로드 1. splunk.com을 열고 Splunk.com ID를 사용하여 로그인합니다. 라이선스가 있는 Enterprise Security 고객만 제품을
다운로드할 수 있습니다. 2. 최신 Splunk Enterprise Security 제품을 다운로드합니다. 3. 다운로드를 다운로드 선택하고 Splunk Enterprise Security 제품 파일을 데스크톱에 저장합니다. 4. Enterprise Security 검색 헤드에 관리자 권한으로 로그인합니다.
3 단계단계. 최신최신 Splunk Enterprise Security 설치설치
1. Splunk Enterprise 검색 페이지에서 앱 > 앱 관리관리를 선택하고 파일에서 파일에서 앱 설치설치를 선택합니다. 2. Splunk Enterprise Security 제품 파일을 선택합니다. 3. 파일파일 선택선택을 클릭하고 Splunk Enterprise Security 제품 파일을 선택합니다. 4. 앱 업그레이드를 업그레이드 클릭하여 기존의 Splunk Enterprise Security 설치본을 덮어씁니다. 5. 업로드를 업로드 클릭하여 설치를 시작합니다. 6. 프롬프트가 나타나면 Splunk Enterprise를 다시 시작합니다.
파일 업로드 완료 후에 설치 절차를 곧바로 시작하지 않으면 오류가 표시됩니다.
4 단계단계. Splunk Enterprise 보안보안 설치설치
Splunk Web이 다시 시작된 후 Splunk Enterprise Security를 설정합니다.
1. 앱 설정설정 페이지로 페이지로 계속계속을 클릭하여 ES 설정을 시작합니다. 2. 시작시작을 클릭합니다. 3. 설치 단계가 진행됨에 따라 Splunk Enterprise Security 설치설치 후 설정설정 페이지에 업그레이드 상태가 표시됩니다. 4. 선택된 추가 기능을 설치에서 제외할지 설치하고 비활성화할지 선택합니다.
설치가 완료되면 Splunk 플랫폼 서비스를 다시 시작하라는 메시지가 페이지에 표시됩니다. 5. Splunk 다시다시 시작시작을 클릭하여 설치를 마칩니다.
5 단계단계. 업그레이드 업그레이드 확인확인
이제 Splunk Enterprise Security 업그레이드 프로세스가 완료되었습니다. 업그레이드 프로세스 중에 비활성화된 개체들이 자동으로 활성화됩니다.
1. Enterprise Security 메뉴 모음에서 감사감사 > ES 설정설정 상태상태를 선택합니다. 2. 잠재적인 충돌과 기본 설정 변경 사항을 검토합니다. 사용자 매뉴얼에서 ES 설정 상태를 참조하십시오. 3. Splunk Web에 액세스하는 데 사용하는 브라우저의 브라우저 캐시를 삭제하여 업그레이드 후에 새로운 Splunk Web
버전에 액세스하도록 보장합니다. 브라우저 캐시를 삭제하지 않으면 일부 페이지가 로드되지 않을 수 있습니다.
Splunk는 업그레이드를
$SPLUNKHOME$/var/log/splunk/essinstaller2.log
로그에 기록합니다.
버전별 버전별 업그레이드 업그레이드 참고참고 사항사항
Enterprise Security를 버전 5.0.x로 업그레이드한 후 룩업룩업 편집편집 권한 체크박스를 다시 선택해야 합니다. 룩업룩업 편집편집 권한에 는 이제 추가 기능이 포함되므로 권한이 기본적으로 확인되지 않습니다. 역할에는 여전히 edit_lookups 기능이 포함됩니 다. 설치 및 업그레이드 매뉴얼에서 사용자 및 역할 설정을 참조하십시오.
26
Enterprise Security를 4.1.x 이전 버전에서 4.1.x 이후 버전으로 업그레이드한 후에 검색 마이그레이션 프로세스가 계속 실 행되는 경우 상관(correlation)검색 편집기에 업그레이드 전 설정과 일치하지 않는 설정이 표시될 수 있습니다. 내부 인덱스 를 검색하여 성공적으로 마이그레이션된 검색을 찾아보고 마이그레이션 작업의 상태를 검토하십시오.
index=_internal sourcetype=configuration_check file="confcheck_es_modactions*" migrated
Enterprise Security를 4.1.x 이전 버전에서 4.1.x 이후 버전으로 업그레이드한 후에 활성화되는 주요 이벤트를 만들도록 설 정되지 않은 상관(correlation)검색은 주요 이벤트를 만드는 설정으로 돌아가지 않습니다. 예를 들어 기본적으로 주요 이벤 트와 위험 한정자를 만들었던 상관(correlation)검색을 위험 한정자만 만들도록 설정한 경우, 업그레이드 후에 상관 (correlation) 검색에서 위험 한정자와 주요 이벤트를 모두 만듭니다.
1. 업그레이드하기 전에, 주요 이벤트를 만들지 않는 활성화된 상관(correlation)검색을 다음 검색을 사용하여 확인합니
다.
| rest splunk_server=local count=0 /services/saved/searches search="name=\"*-Rule\"" | where disabled=0 AND
'action.summary_index'=0 | table 'eai:acl.app',title
2. 업그레이드가 완료된 후에 영향을 받은 상관(correlation)검색을 업데이트하여 검색에서 주요 이벤트를 더 이상 만들
지 않게 합니다.
Splunk Enterprise Security의 업그레이드 업그레이드 및 설정설정 테스트 테스트 전체 업그레이드를 수행하기 전에 Splunk Enterprise Security의 업그레이드 및 설정을 테스트할 수 있습니다. 다음 절차를 따르기 전에 위의 1~3단계를 완료해야 합니다.
1. Splunk Web에서 검색 및 보고 앱을 엽니다. 2. 다음 검색을 입력하여 업그레이드 및 설정의 시험 실행을 수행합니다.
|essinstall --dry-run
3. 추가 옵션을 사용하여 설치할 추가 기능을 지정하거나 설치를 건너뛰거나 설치 후 비활성화할 수 있습니다.
|essinstall --install-ta <ta-name>+ --skip-ta <ta-name>+ --disable-ta <ta-name>+
설치하거나 건너뛰거나 비활성화할 추가 기능의 이름을 지정하거나 *를 와일드카드로 사용합니다. 설치할 여러 추가 기능을 지정하려면
+
를 사용하십시오.
검색검색 헤드헤드 클러스터에서 클러스터에서 Enterprise Security 업그레이드 업그레이드 Splunk Enterprise Security 검색 헤드 클러스터를 업그레이드하기 전에 다음 지침 및 작업 순서를 검토하십시오.
1. 스테이징 인스턴스를 준비합니다. 2. 스테이징 인스턴스를 최신 버전으로 업그레이드합니다. 3. 업그레이드된 설치본을 프로덕션 주 배포 노드로 마이그레이션합니다. 4. 클러스터 구성원에 변경 사항을 배포합니다. 5. 검색 헤드 클러스터에서 설정을 확인합니다.
이 지침의 이전 버전을 정확하게 따를 경우 Splunk Enterprise에 포함된 기본 앱을 주 배포 노드를 사용하여 검색 피어에 배 포하게 될 수도 있습니다. 이 방법으로 기본 앱을 배포하는 것은 권장되지 않습니다.
이 잘못된 구성의 결과로 배포에 문제가 발생하면 독립형 검색 헤드를 검색 헤드 클러스터로 마이그레이션하는 것과 유사한 절차에 따라 설치 설정을 새 주 배포 노드와 검색 헤드 클러스터로 마이그레이션하십시오. 이 페이지의 기존 검색 헤드를 검 색 헤드 클러스터로 마이그레이션을 참조하십시오.
전제전제 조건조건
Splunk Enterprise Security 패키지에 포함된 추가 기능을 검토합니다. 필요한 경우 Splunk Enterprise를 이 Splunk Enterprise Security 버전과 호환되는 최신 버전으로 업그레이드합니다.
호환되는 버전은 Splunk Enterprise 시스템 요구 사항을 참조하십시오. Splunk Enterprise 업그레이드 지침은 Splunk Enterprise 분산 검색 매뉴얼의 검색 헤드 클러스터 업그레이드 를 참조하십시오.
스테이징 스테이징 인스턴스 인스턴스 준비준비
업그레이드하기 전에 주 배포 노드의 Splunk Enterprise Security 복사본을 최신 릴리스와 비교해야 합니다. 스테이징 인스 턴스에서 업그레이드를 수행하면 됩니다. Splunk Enterprise만 설치된 Splunk 환경에 테스트 또는 QA 인스턴스가 있는 경 우, 이 인스턴스를 스테이징에 사용할 수 있습니다.
1. 업그레이드 스테이징에 사용할 단일 Splunk Enterprise 인스턴스를 준비합니다. 인스턴스를 인덱서 또는 검색 피어에
27
연결하지 마십시오. 2. 주 배포 노드 인스턴스 경로
etc/shcluster/apps
의 앱을 스테이징 인스턴스 경로
etc/apps
로 복사합니다. 예를 들어 주 배포 노드 유형에서:
scp -r ~/etc/shcluster/apps <staging_machine>:~/etc/ 주 배포 노드에 검색 앱과 같은 기본 앱이 포함되어 있는 경우 폴더를 스테이징 인스턴스에 복사하기 전에 주 배포 노 드에서 제거하십시오.
주 배포 노드의 Splunk Enterprise Security 복사본에는 검색 헤드 클러스터에 배포된 구성 설정이 포함됩니다. 복사본에는 검색 헤드 클러스터 노드 간에 복제된 런타임 knowledge object 변경 사항이 포함되지 않습니다.
스테이징 스테이징 인스턴스를 인스턴스를 최신최신 버전으로 버전으로 업그레이드
업그레이드 1. Splunk Enterprise Security 업그레이드 프로세스의 1 - 4단계를 진행합니다. 2. ES 설정 상태 대시보드를 검토하여 배포된 버전과 Splunk Enterprise Security의 최신 릴리스 간의 구성 및 설정 변
경 사항을 식별합니다.
설치 프로그램이 지원 중단된 앱이나 추가 기능을 자동으로 비활성화합니다. 지원이 중단된 항목을 모두 식별하는 경고가 스 테이징 인스턴스의 메시지에 표시됩니다. 지원이 중단된 앱이나 추가 기능을 Enterprise Security 설치본에서 수동으로 제거 해야 합니다.
업그레이드된 업그레이드된 ES 설치본을 설치본을 주 배포배포 노드로 노드로 마이그레이션 마이그레이션 Splunk Enterprise Security를 구성하는 앱을 스테이징 인스턴스에서 주 배포 노드로 이동합니다.
1. 스테이징 인스턴스에서 Splunk Enterprise Security Suite와 연결된 앱, SA, DA, TA를
$SPLUNK_HOME/etc/apps
디렉터 리에서 주 배포 노드의
$SPLUNK_HOME/etc/shcluster/apps
디렉터리로 복사합니다. 1. 업그레이드 중에 확인했던 지원이 중단된 앱이나 추가 기능은 복사하지 않습니다. 2. 검색, 시작 관리자 또는 gettingstarted 앱과 같은 기본 앱은 복사하지 않습니다.
Splunk Enterprise에 포함된 앱은 업그레이드 및 배포할 필요가 없으므로
$SPLUNK_HOME/etc/apps
의 모든 앱은 복사하지 마십 시오.
클러스터 클러스터 구성원에 구성원에 변경변경 사항사항 배포배포
1. 주 배포 노드에서,
-preserve-lookups true
를 사용하여 Enterprise Security를 배포하여 검색 헤드 클러스터 구성원에 서 생성된 룩업 파일 콘텐츠를 보존합니다. 분산 검색의 설정 번들 배포를 참조하십시오.
이 설정에 대한 자세한 내용은 Splunk Enterprise 분산 검색 매뉴얼에서 앱 업그레이드 후 룩업 파일 유지를 참조하십시오.
검색검색 클러스터에서 클러스터에서 설정설정 확인확인
주 배포 노드의 Enterprise Security 복사본을 검색 헤드 클러스터 구성원에 배포한 후, ES 설정 상태 대시보드를 사용하여 클러스터 복제된 knowledge object를 최신 Enterprise Security 설치본과 비교합니다.
1. 검색 헤드 클러스터 구성원에서 Splunk Web에 로그인합니다. 2. Enterprise Security를 엽니다. 3. Enterprise Security 메뉴 모음에서 감사감사 > ES 설정설정 상태상태를 선택합니다. 4. 잠재적인 충돌과 기본 설정 변경 사항을 검토합니다.
Splunk Enterprise Security 사용의 ES 설정 상태를 참조하십시오.
기존기존 검색검색 헤드를 헤드를 검색검색 헤드헤드 클러스터로 클러스터로 마이그레이션 마이그레이션 Enterprise Security 독립형 검색 헤드나 검색 헤드 풀 구성원을 검색 헤드 클러스터에 추가할 수 없습니다. ES 설정을 검색 헤드 클러스터로 마이그레이션하는 방법:
1. 이전 ES 설치본에서 사용자 지정 설정 및 수정 사항을 모두 확인합니다. Enterprise Security를 클러스터에 배포할 때
기본 설정과 충돌할 수 있는
ess_setup.conf
의 로컬 복사본이 없는지 확인합니다. 2. 새 검색 헤드 클러스터를 구현합니다. 3. 최신 Enterprise Security 버전을 검색 헤드 클러스터에 배포합니다. 4. 사용자 지정 설정을 검토하고 검색 헤드 클러스터 주 배포 노드로 마이그레이션하여 클러스터 구성원에 복제합니다. 5. 이전 ES 검색 헤드를 종료합니다.
설정 마이그레이션에 대한 자세한 내용은 Splunk Enterprise 분산 검색 매뉴얼에서 독립형 검색 헤드에서 검색 헤드 클러스 터로 마이그레이션을 참조하십시오.
Splunk Enterprise Security 배포 마이그레이션을 계획하는 데 도움이 필요하면 Splunk 프로페셔널 서비스 팀에 문의하십 시오.
28
