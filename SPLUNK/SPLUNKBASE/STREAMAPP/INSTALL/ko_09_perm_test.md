Splunk Stream 테스트 환경
이 페이지에서는 Splunk Stream 하드웨어 성능 테스트에 사용되는 다양한 테스트 환경을 설명합니다.

인 Splunk 스트림 성능 테스트 결과는 CPU 사용량 및 메모리 사용량을 보여 splunkd및 streamfwdHTTP와와 SSL없이 모두 작업의 범위 TCP / UDP 트래픽. 하드웨어 성능 테스트는 다음 Splunk Stream 기능에서 실행됩니다.

Splunk_TA_stream( streamfwd바이너리 포함 ) UF (Universal Forwarder)에서 실행됩니다.
streamfwdHTTP Event Collector (HEC)를 통해 인덱서로 데이터를 보내는 Independent Stream Forwarder ( 바이너리).
흐름 수집기.
Splunk_TA_stream (UF) 테스트 환경
Splunk_TA_stream(UF) 테스트는 최대 1Gbps의 워크로드로 실행되었습니다. HEC는 더 높은 대역폭 트래픽에 권장됩니다.

하드웨어 테스트
CentOS 6.7 (64 비트).
듀얼 Intel Xeon E5-2650 CPU (2.0Ghz 코어 16 개, 총 32 개 코어).
164GB RAM.
streamfwd.conf 구성
[streamfwd]
ipAddr = 0.0.0.0
logConfig = streamfwdlog.conf
포트 = 8889
processingThreads = 4
streamfwdcapture.0.interface = eth0
dedicatedCaptureMode = 0
스트림 구성
유니버설 포워더는 기본 스트림 캡처 구성으로 실행됩니다.

HEC (Independent Stream Forwarder) 테스트 환경
모든 독립 Stream Forwarder 테스트 환경은 동일한 하드웨어 구성을 사용합니다. 테스트 설정의 유일한 차이점은 활성화 된 스트림 목록입니다.

하드웨어 테스트
독립 streamfwd테스트는 다음 서버에서 실행됩니다.

CentOS 6.7 (64 비트).
듀얼 Intel Xeon E5-2698 v3 CPU (2.3Ghz 코어 16 개, 총 32 개 코어).
64GB RAM.
streamfwd.conf 구성
[streamfwd]
ipAddr = 0.0.0.0
processingThreads = 4
dedicatedCaptureMode = 1
streamfwdcapture.0.interface = 0000 : 05 : 00.0
streamfwdcapture.1.interface = 0000 : 05 : 00.1
스트림 구성
streamfwdHEC ( Independent Stream Forwarder ) 테스트는 네 가지 스트림 구성에서 성능을 측정합니다. 이러한 구성 streamfwd은 인덱서로 전송되는 트래픽의 양과 streamfwd이벤트를 추출하기 위해 패킷을 검사하는 정도 를 결정합니다.

구성	인덱서로 전달 된 이벤트	패킷 검사 수준
기본 구성	골재	깊은
HTTP 원시 이벤트	원시 이벤트	깊은
TCP / UDP 원시 이벤트	원시 이벤트	얕은
TCP / UDP 집계	골재	얕은
}

기본 구성
Splunk_ *로 시작하는 모든 스트림은 활성화되고 원시 이벤트를 전달하는 다른 모든 스트림은 비활성화됩니다. Splunk_ * 스트림은 다양한 스트림에서 이벤트 집합을 생성하므로 사용자는 다양한 원시 이벤트 전달을 켤 때 Stream에서 사용할 인덱서 용량을 추정 할 수 있습니다.

HTTP 원시 이벤트
이 구성에서는 http 원시 이벤트 만 활성화됩니다. 그러나 HTTP는 레벨 7 프로토콜이므로 관심있는 HTTP 이벤트를 생성하려면 패킷 전체에서 상태를 유지해야합니다.

TCP / UDP 원시 이벤트
이 구성에서는 tcp 및 udp 원시 이벤트 만 활성화됩니다. 이것은 네트워크 스택의 레벨 4보다 높지 않으므로 더 심층 분석을 수행 할 필요가 없지만 가져 오는 모든 원시 패킷에 대한 정보를 보냅니다.

TCP / UDP 집계
이 구성에서는 TCP 및 UDP 프로토콜의 각 소스 IP 주소 (src_ip)에 대해 전송되는 바이트 수를 계산합니다. 집계는 30 초마다 계산됩니다. 이것은 네트워크 스택의 레벨 4보다 높지 않으므로 심층 분석이 필요하지 않습니다.

흐름 수집기 테스트 환경
하드웨어 테스트
NetFlow 수집기 테스트는 다음 서버에서 실행됩니다.

 CentOS 6.7 (64 비트).
 듀얼 Intel Xeon E5-2698 v3 CPU (2.3Ghz 코어 16 개, 총 32 개 코어).
 64GB RAM
streamfwd.conf 구성
[streamfwd]
ipAddr = 0.0.0.0
processingThreads = 4
dedicatedCaptureMode = 0
httpRequestSenderThreads = 4
httpRequestSenderConnections = 40
netflowReceiver.0.port = 9996
netflowReceiver.0.protocol = udp
netflowReceiver.0.decoder = netflow
netflowReceiver.0.ip = 172.18.1.4
netflowReceiver.0.decodingThreads = 32
Flow collector 테스트 결과 및 방법론 은이 설명서의 Flow collector 테스트 결과 를 참조하십시오 .

Splunk_TA_stream (UF) 테스트 결과-기본 구성
이 페이지는 streamfwd다양한 워크로드에 대해 UF (유니버설 포워더)에서 실행 되는 Splunk_TA_stream ( 바이너리 포함)에 대한 성능 테스트 결과를 보여줍니다 . 자세한 테스트 환경 및 구성 정보 는이 설명서의 Splunk Stream 테스트 환경 을 참조하십시오 .

HTTP 100K 응답 테스트 결과
통계	8Mbps	64Mbps	256Mbps	512Mbps	1Gbps	2Gbps	3Gbps	4Gbps
CPU % (splunkd)	2	4	4	삼	삼	삼	4	4
메모리 MB (splunkd)	130	131	130	130	163	162	162	161
CPU % (streamfwd)	7	15	32	55	93	183	266	414
메모리 MB (streamfwd)	163	174	223	234	308	325	376	5270
이벤트 / 초	41	217	884	1758 년	3435	6852	10277	14318
드롭률 %	0	0	0	0	0	0	0	5
HTTP 25K 응답 테스트 결과
통계	8Mbps	64Mbps	256Mbps	512Mbps	1Gbps	2Gbps	3Gbps	4Gbps	
CPU % (splunkd)	4	4	4	4	4	4	삼	5
메모리 MB (splunkd)	126	126	125	125	162	162	162	162
CPU % (streamfwd)	8	23	64	118	202	382	364	318
메모리 MB (streamfwd)	165	176	219	229	311	428	5987	6126
이벤트 / 초	113	845	3470	6910	13449	26909	20043	12627
드롭률 %	0	0	0	0	0	0	17	17	
HTTPS 100K 응답 테스트 결과
통계	8Mbps	64Mbps	256Mbps	512Mbps	1Gbps	2Gbps	3Gbps	4Gbps
CPU % (splunkd)	2	4	4	삼	4	29	34	46
메모리 MB (splunkd)	128	129	129	128	167	164	164	167
CPU % (streamfwd)	7	26	78	144	249	387	412	429
메모리 MB (streamfwd)	247	298	327	349	468	1504 년	1871 년	2114
이벤트 / 초	21	158	431	841	1603 년	1938 년	1892 년	2235
드롭률 %	0	0	0	0	0	6	15	28
HTTPS 25K 응답 테스트 결과
통계	8Mbps	64Mbps	256Mbps	512Mbps	1Gbps	2Gbps	3Gbps	4Gbps
CPU % (splunkd)	2	삼	2	삼	삼	36	37	36
메모리 MB (splunkd)	128	165	165	164	162	161	162	161
CPU % (streamfwd)	7	26	79	149	271	406	441	455
메모리 MB (streamfwd)	247	360	387	412	452	1553 년	1808 년	2119
이벤트 / 초	22	162	427	831	1597 년	1927 년	1971 년	2294 년
드롭률 %	0	0	0	0	0	7	12	23

