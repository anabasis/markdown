# Requirement

Hardware Recommendations for Apache Hadoop
Hadoop and HBase workloads tend to vary a lot and it takes experience to correctly anticipate the amounts of storage, processing power, and inter-node communication that will be required for different kinds of jobs. This document provides insights on choosing the appropriate hardware components for an optimal balance between performance and both initial as well as the recurring costs.

Hadoop is a software framework that supports large-scale distributed data analysis on commodity servers. Hortonworks is a major contributor to open source initiatives (Apache Hadoop, HDFS, Pig, Hive, HBase, ZooKeeper) and has extensive experience managing production level Hadoop clusters. Hortonworks recommends following the design principles that drive large, hyper-scale deployments. For a Hadoop or HBase cluster, it is critical to accurately predict the size, type, frequency, and latency of analysis jobs to be run. When starting with Hadoop or HBase, begin small and gain experience by measuring actual workloads during a pilot project. This way you can easily scale the pilot environment without making any significant changes to the existing servers, software, deployment strategies, and network connectivity.

Typical Hadoop Cluster
Hadoop and HBase clusters have two types of machines: masters and slaves.
Typical Workload Patterns For Hadoop
Disk space, I/O Bandwidth (required by Hadoop), and computational power (required for the MapReduce processes) are the most important parameters for accurate hardware sizing. Additionally, if you are installing HBase, you also need to analyze your application and its memory requirements, because HBase is a memory intensive component.
Early Deployments
When a team is just starting with Hadoop or HBase, it is usually good to begin small and gain experience by measuring actual workloads during a pilot project. We recommend starting with a relatively small pilot cluster, provisioned for a “ balanced ” workload.
Server Node Hardware Recommendations
You must use the server node hardware recommendations as best practices for selecting the number of nodes, storage options per node (number of disks, size of disks, MTBF, and the replication cost of disk failures), compute power per node (sockets, cores, clock speed), RAM per node, and network capability (number, speed of ports).
Hardware for HBase
HBase uses different types of caches to fill up memory, and as a general rule the more memory HBase has, the better it can cache read requests. Each slave node in an HBase cluster (RegionServer) maintains a number of regions (regions are the chunks of the data in memory). For large clusters, it is important to ensure that the HBase Master and the NameNode run on separate server machines.
Other Issues
In addition to the hardware considerations, you must consider factors such as using erasure coding for storage capacity, weight of the server racks, scalability of the cluster, and others for your Hadoop cluster.
Conclusion
Achieving optimal results from a Hadoop implementation begins with choosing the correct hardware and software stacks. The effort involved in the planning stages can pay off dramatically in terms of the performance and the total cost of ownership (TCO) associated with the environment.

Apache Hadoop에 대한 하드웨어 권장 사항
Hadoop 및 HBase 워크로드는 매우 다양한 경향이 있으며 다양한 작업에 필요한 스토리지, 처리 능력 및 노드 간 통신의 양을 올바르게 예측하려면 경험이 필요합니다. 이 문서는 성능과 초기 비용 및 반복 비용 사이의 최적 균형을 위해 적절한 하드웨어 구성 요소를 선택하는 방법에 대한 통찰력을 제공합니다.

Hadoop은 상용 서버에서 대규모 분산 데이터 분석을 지원하는 소프트웨어 프레임 워크입니다. Hortonworks는 오픈 소스 이니셔티브 (Apache Hadoop, HDFS, Pig, Hive, HBase, ZooKeeper)의 주요 기여자이며 프로덕션 수준의 Hadoop 클러스터 관리 경험이 풍부합니다. Hortonworks는 대규모 대규모 배포를 추진하는 설계 원칙을 따를 것을 권장합니다. Hadoop 또는 HBase 클러스터의 경우 실행할 분석 작업의 크기, 유형, 빈도 및 지연 시간을 정확하게 예측하는 것이 중요합니다. Hadoop 또는 HBase로 시작할 때 파일럿 프로젝트 중에 실제 워크로드를 측정하여 소규모로 시작하여 경험을 쌓으십시오. 이렇게하면 기존 서버, 소프트웨어, 배포 전략 및 네트워크 연결을 크게 변경하지 않고도 파일럿 환경을 쉽게 확장 할 수 있습니다.

일반적인 Hadoop 클러스터
Hadoop 및 HBase 클러스터에는 마스터와 슬레이브의 두 가지 유형의 머신이 있습니다.
Hadoop의 일반적인 워크로드 패턴
디스크 공간, I / O 대역폭 (Hadoop에 필요) 및 계산 능력 (MapReduce 프로세스에 필요)은 정확한 하드웨어 크기 조정을위한 가장 중요한 매개 변수입니다. 또한 HBase를 설치하는 경우 HBase는 메모리 집약적 인 구성 요소이므로 응용 프로그램과 해당 메모리 요구 사항도 분석해야합니다.
초기 배포
팀이 Hadoop 또는 HBase로 막 시작하는 경우 일반적으로 소규모로 시작하여 파일럿 프로젝트 중에 실제 워크로드를 측정하여 경험을 얻는 것이 좋습니다. "균형 잡힌"워크로드를 위해 프로비저닝 된 비교적 작은 파일럿 클러스터로 시작하는 것이 좋습니다.
서버 노드 하드웨어 권장 사항
노드 수, 노드 당 스토리지 옵션 (디스크 수, 디스크 크기, MTBF 및 디스크 오류의 복제 비용), 노드 당 컴퓨팅 성능 (소켓, 코어)을 선택하기위한 모범 사례로 서버 노드 하드웨어 권장 사항을 사용해야합니다. , 클럭 속도), 노드 당 RAM 및 네트워크 기능 (포트 수, 속도).
HBase 용 하드웨어
HBase는 메모리를 채우기 위해 다양한 유형의 캐시를 사용하며 일반적으로 HBase의 메모리가 많을수록 읽기 요청을 더 잘 캐시 할 수 있습니다. HBase 클러스터 (RegionServer)의 각 슬레이브 노드는 여러 영역을 유지합니다 (영역은 메모리에있는 데이터의 청크입니다). 대규모 클러스터의 경우 HBase 마스터와 네임 노드가 별도의 서버 시스템에서 실행되는지 확인하는 것이 중요합니다.
다른 문제
하드웨어 고려 사항 외에도 스토리지 용량에 대한 삭제 코딩 사용, 서버 랙의 무게, 클러스터의 확장 성 및 Hadoop 클러스터에 대한 기타 요소를 고려해야합니다.
결론
Hadoop 구현에서 최적의 결과를 얻으려면 올바른 하드웨어 및 소프트웨어 스택을 선택해야합니다. 계획 단계에 포함 된 노력은 성능 및 환경과 관련된 총 소유 비용 (TCO) 측면에서 상당한 성과를 거둘 수 있습니다.

<https://docs.informatica.com/data-engineering/data-engineering-integration/h2l/big-data-management-10-2-1-performance-tuning-and-sizing-guideli/big-data-management-1021-performance-tuning-and-sizing-guideline/sizing-recommendations/hadoop-cluster-hardware-recommendations.html>

## Hadoop Cluster Hardware Recommendations

The following table lists the minimum and optimal hardware requirements for the Hadoop cluster:

<table>
<tr><td>Hardware</td><td>Sandbox Deployment</td><td>Basic or Standard Deployment</td><td>Advanced Deployment</td></tr>
<tr><td>CPU speed</td><td>2 - 2.5 GHz</td><td>2 - 2.5 GHz</td><td>2.5 - 3.5 GHz</td></tr>
<tr><td>Logical or virtual CPU cores</td><td>16</td><td>24 - 32</td><td>48</td></tr>
<tr><td>Total system memory</td><td>16 GB</td><td>64 GB</td><td>128 GB</td></tr>
<tr><td>Local disk space for yarn.nodemanager.local-dirs1</td><td>256 GB</td><td>500 GB</td><td>2.4 TB</td></tr>
<tr><td>DFS block size</td><td>128 MB</td><td>256 MB</td><td>256 MB</td></tr>
<tr><td>HDFS replication factor</td><td>3</td><td>3</td><td>3</td></tr>
<tr><td>Disk capacity</td><td>32 GB</td><td>256 GB - 1 TB</td><td>1.2 TB</td></tr>
<tr><td>Total number of disks for HDFS</td><td>2</td><td>8</td><td>12</td></tr>
<tr><td>Total HDFS capacity per node</td><td>64 GB</td><td>2 - 8 TB</td><td>At least 14 TB</td></tr>
<tr><td>Number of nodes</td><td>2 +</td><td>4 - 10+</td><td>12 +</td></tr>
<tr><td>Total HDFS capacity on the cluster</td><td>128 GB</td><td>8 - 80 TB</td><td>144 TB</td></tr>
<tr><td>Actual HDFS capacity (with replication)</td><td>43 GB</td><td>2.66 TB</td><td>57.6 TB</td></tr>
<tr><td>/tmp mount point</td><td>20 GB</td><td>20 GB</td><td>30 GB</td></tr>
<tr><td>Installation disk space requirement</td><td>12 GB</td><td>12 GB</td><td>12 GB</td></tr>
<tr><td>Network bandwidth (Ethernet card)</td><td>1 Gbps</td><td>2 Gbps (bonded channel)</td><td>10 Gbps (Ethernet card)</td></tr>
<tr><td colspan=4>1 A property in the yarn-site.xml that contains a list of directories to store localized files. You can find the localized file directory in: ${yarn.nodemanager.local-dirs}/usercache/${user}/appcache/application_${appid}. You can find the work directories of individual containers, container_${contid}, as the subdirectories of the localized file directory.</td></tr>
</table>

## MapR Cluster Recommendation

When you run mappings on the Blaze, Spark, or Hive engine, local cache files are generated under the directory specified in the yarn.nodemanager.local-dirs property in the yarn-site.xml. However, the directory might not contain sufficient disk capacity on a MapR cluster.
To make sure that the directory has sufficient disk capacity, perform the following steps:

1. Create a volume on HDFS.
2. Mount the volume through NFS.
3. Configure the NFS mount location in yarn.nodemanager.local-dirs.

For more information, refer to the MapR documentation.
Sizing Recommendations