# Monitoring Splunk Enterprise overview

Splunk Enterprise provides two types of deployment monitoring:

- Monitoring Console (search-based monitoring).
- Proactive Splunk component monitoring (REST-based monitoring).

Splunk Enterprise는 두 가지 유형의 배포 모니터링을 제공합니다.

- 모니터링 콘솔 (검색 기반 모니터링).
- 능동적 인 Splunk 구성 요소 모니터링 (REST 기반 모니터링).

## Monitoring Console

The Monitoring Console is a search-based monitoring tool that lets you view detailed information about the topology and performance of your Splunk Enterprise deployment. The Monitoring Console provides pre-built dashboards that give you visibility into many areas of your deployment, including search and indexing performance, resource usage, license usage, and more. You can use the Monitoring console to track the status of all types of deployment topologies, from single-instance (standalone) deployments to complex multi-site indexer clusters.

For more information, see About the Monitoring Console.

- Monitoring Console은 검색 기반 모니터링 도구로, Splunk Enterprise 배포의 토폴로지 및 성능에 대한 자세한 정보를 볼 수 있습니다.
- Monitoring Console은 사전 구축 된 대시 보드를 제공하여 검색 및 인덱싱 성능, 리소스 사용, 라이센스 사용 등을 비롯하여 배포의 여러 영역에 대한 가시성을 제공합니다.
- 모니터링 콘솔을 사용하여 단일 인스턴스 (독립 실행 형) 배포에서 복잡한 다중 사이트 인덱서 클러스터에 이르기까지 모든 유형의 배포 토폴로지 상태를 추적 할 수 있습니다.

자세한 내용은 Monitoring Console 정보를 참조하십시오.

## Proactive Splunk component monitoring

Proactive Splunk component monitoring is a REST-based monitoring tool that lets you view the health of Splunk Enterprise features from the output of a REST API endpoint. Individual features report their health status through a tree structure that provides a continuous, high-level view of the health of your deployment. You can access feature health status information using the splunkd health report in Splunk Web, or access feature health status information from the /server/health/splunkd endpoint.

For more information, see About proactive Splunk component monitoring.

- Proactive Splunk 구성 요소 모니터링은 REST 기반 Endpoint의 출력에서 Splunk Enterprise 기능의 상태를 볼 수있는 REST 기반 모니터링 도구입니다.
- 개별 기능은 트리 구조를 통해 상태를 보고하여 배포 상태를 지속적으로 높은 수준으로 볼 수 있습니다.
- Splunk Web의 splunkd 상태 보고서를 사용하여 기능 상태 정보에 액세스하거나 /server/health/splunkd 끝점에서 기능 상태 상태 정보에 액세스 할 수 있습니다.

자세한 내용은 능동적 인 Splunk 구성 요소 모니터링 정보를 참조하십시오.