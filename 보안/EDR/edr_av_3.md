# ENDPOINT PROTECTION AND EDR

- Cynet converges endpoint protection, EDR and all other essential security technologies into the first autonomous security platform to provide total environment visibility and protection

What Does EDR Stand For? Endpoint Detection & Response 101
Endpoint Detection and Response (EDR) is a new security category defined by Gartner in 2013. It fills an important gap in protection of endpoints, helping security teams gain visibility into malicious activity on an endpoint, and remotely control endpoints to contain and mitigate attacks.

This article will help you understand the core capabilities of EDR, how it is different from Endpoint Protection Platforms (EPP) and antivirus, and how it can help you secure your organization from the growing threat of endpoint-targeted attacks.

In this article you will learn:

What EDR stands for
Security objectives and core capabilities of EDR technology
Endpoint security terminology and the difference between EDR, EPP, NGAV, SIEM
What are the main features offered by EDR solutions
What Does EDR Stand For?
EDR is a security practice and technology defined by Gartner in 2013. EDR stands for:

Endpoint—an endpoint is a device such as a user workstations, or server
Detection—EDR technology helps detect attacks on endpoint devices and provide security teams with fast access to information that can help investigate the attack
Response—EDR solutions can automatically response to attacks by performing actions at the device level, such as quarantining the endpoint or blocking malicious processes
The primary function of EDR solutions is to alert security teams to malicious activity on endpoints, and enable real-time investigation of the root cause and scope of an attack. EDR has three key mechanisms:

Endpoint data collection—aggregates data on events such as process execution, communication, and user logins.
Detection engine—performs behavioral analysis to establish a baseline of typical endpoint activity, discover anomalies, and determine which anomalies represent malicious activity on the endpoint.
Data recording—provides security teams with real-time data about security incidents on endpoints, which they can use to investigate an incident in real time, contain and mitigate it.
Endpoint Security—Understanding the Terminology
There are several components to a comprehensive endpoint security solution, and the various associated terms are often confused or conflated. Let’s clear up this misunderstanding with precise definitions of the related concepts of EDR, EPP, AV/NGAV, and SIEM.

What is the Difference Between EDR and EPP?
According to Gartner, an Endpoint Protection Platform (EPP) is a security solution designed to detect malicious activity on endpoints, prevent malware attacks, and enable investigation and remediation of dynamic security incidents. This definition includes EDR as an integral part of EPP solutions.

EPP platform functionality can be divided into two broad categories:

Prevention – an EPP goes beyond legacy antivirus (AV). It provides Next-Generation Antivirus (NGAV) technology that can detect malware and exploits even if they don’t match a known file signature. This aspect of EPP focuses on detecting a high percentage of attacks on endpoints and blocking them.
Detection and Response – this part is provided by EDR technology. It focuses on detecting attacks that manage to bypass the endpoint’s defensive measures, taking measures to prevent the attack from spreading, and notifying security analysts.
What is the Difference between EDR and Antivirus?
Many people confuse the capabilities of EDR and antivirus (AV), assuming they only need to use one of them. However, these two technologies complement each other. Antivirus is a preventative tool that relies on signature-based detection, and it doesn’t provide visibility into how attacks play out. AV can catch the malware, but it doesn’t tell you where it came from or how it spread in your network.

EDR, on the other hand, provides a full picture of how an attacker gained access to your system and what they did once inside. EDR can detect malicious activity on an endpoint as a result of zero-day exploits, advanced persistent threats, fileless or malware-free attacks, which don’t leave signatures and can therefore evade legacy AV and even NGAV.

What is the Difference Between EDR and SIEM?
Security Information and Event Management (SIEM) collects log and event data from across your network to help identify behavior patterns, detect threats, and investigate security incidents. It is broader than EDR, which addresses endpoint activity specifically.

In a large organization, EDR will likely be one of the data inputs of a SIEM. The SIEM can combine information on endpoint security incidents coming from the EDR system, with information from other parts of the security environment, such as network monitoring and alerts from other security tools.

SIEM is also responsible for collecting historical data, for example recording endpoint data over several years, allowing analysts to see if this type of attack has happened before.

EDR Features
Some common EDR features include:

Endpoint visibility— allowing security teams to monitor activity at all endpoints, including applications, processes, and communications, from one central interface.
Data collection— build a repository of recorded events for analytics, which can help you understand attacker behaviors and prevent future breaches.
Threat intelligence— understand how incidents occur and how you can avoid or remediate them. EDR can identify Indicators of Compromise (IoCs) and correlate them with threat intelligence to provide information about attacks and threat actors.
Automated alerts and forensics— real-time alerts about endpoint security incidents, with access to additional context and data to allow analysts to investigate the incident in depth.
Trace back to original breach point— compiles data on the potential entry points for an attack, providing more context for analysts beyond the currently-affected endpoint.
Automated response measures on the endpoint— blocking network access on a device, disabling certain processes, or performing other actions to prevent an attack from spreading to other endpoints.
Cynet: EDR and More
EDR solutions only deal with the process behavior that prompts alerts. Organizations can use EDR tools to attend to specific parts of common Tactics, Techniques, and Procedures (TTP) attackers use. However, EDR products are blind to other attack types.

Let’s turn our attention to the example of credential theft. The default process used by attackers involves dumping password hashes from memory using a customized tool or an open source tool. In this example, the attack method includes anomalous behavior, thus an EDR tool should identify these types of attacks. However, an attacker can acquire the same hashes by scraping the network traffic between two hosts, a process that doesn’t include anomalous activity.

A second example involves the attack technique of lateral movement. In this scenario, the attacker may be able to compromise many user account credentials and logs, connected to many hosts in the network. Here, the anomaly is the user activity and not the process behavior. The EDR would thus not identify the attack at all or would see the attack but without sufficient context, and this would trigger false positives. Therefore, process data is important, but organizations cannot rely on it as the only source of their security data.

Another limitation of EDR tools is that they are restricted to endpoints and cannot help mitigate attacks or restore operations at the user or network level.

Cynet 360 holistic cybersecurity solution
Cynet 360 platform is a comprehensive cyber solution that is developed to run in the entire environment of an organization and not only its endpoints. To achieve this Cynet 360 protects all attack surfaces by tracking the three planes; network traffic, process behavior, and user activity. Attackers typically manifest themselves on one or several of these three planes.



Continuous monitoring to detect and stop threats over this triad provides increased threat visibility. Organizations thus have the chance to monitor more stages in the attack’s lifecycle so they can identify and block threats with greater success.

As a subset of these capabilities, Cynet employs EDR technology with the following capabilities:

Advanced endpoint threat detection —complete visibility and predicts how an attacker could operate, based on continuous monitoring of behavioral analysis and endpoints.
Investigation and validation —search and analysis of historic or current incident data on endpoints, validate alerts and investigate threats. This lets you confirm the threat prior to responding, this reduces dwell-time and helps perform faster remediation.
Rapid deployment and response —deploy across thousands of endpoints in just two hours. You can then use it to perform manual or automatic remediation of threats on the endpoints, minimize damage caused by attacks, and disrupt malicious activity.
Cynet 360 threat protection goes beyond attack detection and prevention. Using Cynet organizations can proactively monitor their internal environments, such as endpoints, hosts, files, and network. This can help organizations reduce their attack surface and the potential for multiple attacks. When it comes to active attacks, an organization must work to enclose the capabilities of the attacker to eradicate the presence of the attacker entirely. This includes disabling compromised users, deleting malicious processes and files, isolating infected hosts and blocking traffic controlled by the attacker.

Learn more about the Cynet 360 security platform.

EDR은 무엇을 의미합니까? 엔드 포인트 탐지 및 대응 101
EDR (Endpoint Detection and Response)은 2013 년 Gartner에서 정의한 새로운 보안 범주입니다. 엔드 포인트 보호의 중요한 차이를 메워 보안 팀이 엔드 포인트에서 악의적 인 활동에 대한 가시성을 확보하고 엔드 포인트를 원격으로 제어하여 공격을 차단하고 완화합니다.

이 기사는 EDR의 핵심 기능, EEP (Endpoint Protection Platform) 및 안티 바이러스와의 차이점, 엔드 포인트 대상 공격의 위협으로부터 조직을 보호하는 데 도움이되는 방법을 이해하는 데 도움이됩니다.

이 기사에서는 다음을 배웁니다.

EDR의 약자
EDR 기술의 보안 목표 및 핵심 기능
엔드 포인트 보안 용어와 EDR, EPP, NGAV, SIEM의 차이점
EDR 솔루션이 제공하는 주요 기능은 무엇입니까
EDR은 무엇을 의미합니까?
EDR은 2013 년 Gartner에서 정의한 보안 관행 및 기술입니다.

E의 ndpoint - 엔드 포인트는 사용자의 워크 스테이션 또는 서버와 같은 장치이다
D 금 속 탐-EDR 기술은 엔드 포인트 장치에 대한 공격을 탐지하고 공격을 조사하는 데 도움이되는 정보에 빠르게 액세스 할 수있는 보안 팀을 제공하는 데 도움이
R 저 응답 EDR-솔루션 수 악성 프로세스 엔드 포인트 또는 차단 검역 등의 디바이스 레벨에서 수행하는 작업에 의한 공격에 대한 자동 응답
EDR 솔루션의 주요 기능은 보안 팀에 엔드 포인트의 악의적 인 활동을 알리고 근본 원인과 공격 범위를 실시간으로 조사 할 수 있도록하는 것입니다. EDR에는 세 가지 주요 메커니즘이 있습니다.

엔드 포인트 데이터 수집 — 프로세스 실행, 통신 및 사용자 로그인과 같은 이벤트에 대한 데이터를 집계합니다.
탐지 엔진-행동 분석을 수행하여 일반적인 엔드 포인트 활동의 기준을 설정하고 이상을 발견하며 엔드 포인트에서 악의적 인 활동을 나타내는 예외를 결정합니다.
데이터 기록-엔드 포인트의 보안 사고에 대한 실시간 데이터를 보안 팀에 제공합니다.이를 통해 사고를 실시간으로 조사하고 포함하고 완화 할 수 있습니다.
엔드 포인트 보안 — 용어 이해
포괄적 인 엔드 포인트 보안 솔루션에는 여러 가지 구성 요소가 있으며 여러 관련 용어가 혼동되거나 혼동되는 경우가 많습니다. EDR, EPP, AV / NGAV 및 SIEM의 관련 개념을 정확하게 정의하여 이러한 오해를 해결해 봅시다.

EDR과 EPP의 차이점은 무엇입니까?
가트너에 따르면, 엔드 포인트 보호 플랫폼 (EPP)은 엔드 포인트의 악의적 인 활동을 탐지하고 악성 코드 공격을 방지하며 동적 보안 사고를 조사하고 치료할 수 있도록 설계된 보안 솔루션입니다. 이 정의에는 EPP 솔루션의 필수 부분으로 EDR이 포함됩니다.

EPP 플랫폼 기능은 크게 두 가지 범주로 나눌 수 있습니다.

예방 – EPP는 레거시 안티 바이러스 (AV)를 뛰어 넘습니다. 알려진 파일 서명과 일치하지 않더라도 맬웨어 및 악용을 탐지 할 수있는 NGAV (Next-Generation Antivirus) 기술을 제공합니다. EPP의이 측면은 엔드 포인트에 대한 높은 비율의 공격을 탐지하고 차단하는 데 중점을 둡니다.
탐지 및 대응 –이 부분은 EDR 기술로 제공됩니다. 엔드 포인트의 방어 조치를 우회하여 공격이 확산되는 것을 막기위한 조치를 취하고 보안 분석가에게 알리는 공격을 탐지하는 데 중점을 둡니다.
EDR과 바이러스 백신의 차이점은 무엇입니까?
많은 사람들이 EDR과 바이러스 백신 (AV)의 기능을 혼동하는데, 그 중 하나만 사용하면된다고 가정합니다. 그러나이 두 기술은 서로 보완 적입니다. 바이러스 백신은 시그니처 기반 탐지에 의존하는 예방 도구이며 공격의 진행 방식에 대한 가시성을 제공하지 않습니다. AV는 악성 코드를 포착 할 수 있지만 네트워크의 출처 나 확산 방식을 알려주지는 않습니다.

반면 EDR은 공격자가 시스템에 액세스 한 방법과 내부에서 한 번 수행 한 작업에 대한 전체 그림을 제공합니다. EDR은 제로 데이 익스플로잇, 고급 영구 위협, 파일없는 또는 맬웨어없는 공격의 결과로 엔드 포인트에서 악의적 인 활동을 탐지 할 수 있으며, 이는 서명을 남기지 않으므로 레거시 AV 및 NGAV를 피할 수 있습니다.

EDR과 SIEM의 차이점은 무엇입니까?
SIEM (Security Information and Event Management)은 네트워크에서 로그 및 이벤트 데이터를 수집하여 동작 패턴을 식별하고 위협을 탐지하며 보안 사고를 조사합니다. 엔드 포인트 활동을 구체적으로 다루는 EDR보다 광범위합니다.

대규모 조직에서 EDR은 SIEM의 데이터 입력 중 하나 일 수 있습니다. SIEM은 EDR 시스템에서 발생하는 엔드 포인트 보안 사고에 대한 정보를 네트워크 모니터링 및 다른 보안 도구의 경보와 같은 보안 환경의 다른 부분에있는 정보와 결합 할 수 있습니다.

SIEM은 또한 수년에 걸쳐 엔드 포인트 데이터를 기록하는 것과 같이 기록 데이터를 수집하여 분석가가 이러한 유형의 공격이 이전에 발생했는지 확인할 수 있도록합니다.

EDR 기능
일반적인 EDR 기능은 다음과 같습니다.

엔드 포인트 가시성 — 보안 팀은 하나의 중앙 인터페이스에서 응용 프로그램, 프로세스 및 통신을 포함한 모든 엔드 포인트의 활동을 모니터링 할 수 있습니다.
데이터 수집 — 분석을 위해 기록 된 이벤트의 리포지토리를 구축하여 공격자의 행동을 이해하고 향후 위반을 방지 할 수 있습니다.
위협 인텔리전스 — 사고가 발생하는 방식과이를 방지하거나 개선 할 수있는 방법을 이해합니다. EDR은 침해 지표 (IoC)를 식별하고이를 위협 인텔리전스와 연관시켜 공격 및 위협 행위자에 대한 정보를 제공 할 수 있습니다.
자동 경보 및 법의학 — 분석가가 사건을 심층적으로 조사 할 수 있도록 추가 컨텍스트 및 데이터에 액세스 할 수있는 엔드 포인트 보안 사건에 대한 실시간 경보.
원래의 침해 지점으로 추적 — 공격의 잠재적 진입 점에 대한 데이터를 컴파일하여 현재 영향을받는 엔드 포인트를 넘어 분석가에게 더 많은 컨텍스트를 제공합니다.
엔드 포인트에서의 자동화 된 대응 조치- 장치의 네트워크 액세스를 차단하거나 특정 프로세스를 비활성화하거나 다른 작업을 수행하여 공격이 다른 엔드 포인트로 확산되는 것을 방지합니다.
Cynet : EDR 등
EDR 솔루션은 경고를 표시하는 프로세스 동작 만 처리합니다. 조직은 EDR 도구를 사용하여 공격자가 사용하는 일반적인 전술, 기법 및 절차 (TTP)의 특정 부분에 참여할 수 있습니다. 그러나 EDR 제품은 다른 공격 유형에 대해 눈을 멀게합니다.

자격 증명 도난의 예에주의를 기울 이겠습니다. 공격자가 사용하는 기본 프로세스에는 사용자 지정 도구 또는 오픈 소스 도구를 사용하여 메모리에서 암호 해시를 덤프하는 작업이 포함됩니다. 이 예에서 공격 방법에는 비정상적인 동작이 포함되므로 EDR 도구는 이러한 유형의 공격을 식별해야합니다. 그러나 공격자는 비정상적인 활동을 포함하지 않는 프로세스 인 두 호스트 사이의 네트워크 트래픽을 스크랩하여 동일한 해시를 얻을 수 있습니다.

두 번째 예는 측면 운동의 공격 기술과 관련이 있습니다. 이 시나리오에서 공격자는 네트워크의 많은 호스트에 연결된 많은 사용자 계정 자격 증명 및 로그를 손상시킬 수 있습니다. 여기서 예외는 프로세스 동작이 아니라 사용자 활동입니다. 따라서 EDR은 공격을 전혀 식별하지 못하거나 공격을 볼 수 있지만 컨텍스트가 충분하지 않으면 오 탐지를 유발할 수 있습니다. 따라서 프로세스 데이터는 중요하지만 조직은 보안 데이터의 유일한 소스로이 데이터에 의존 할 수 없습니다.

EDR 도구의 다른 제한 사항은 엔드 포인트로 제한되며 사용자 또는 네트워크 수준에서 공격을 완화하거나 작업을 복원 할 수 없다는 것입니다.

Cynet 360 전체적인 사이버 보안 솔루션
Cynet 360 플랫폼은 엔드 포인트뿐만 아니라 조직의 전체 환경에서 실행되도록 개발 된 포괄적 인 사이버 솔루션입니다. 이 Cynet 360은 3 개의 평면을 추적하여 모든 공격 표면을 보호합니다. 네트워크 트래픽, 프로세스 동작 및 사용자 활동. 공격자는 일반적으로이 세 가지 비행기 중 하나 이상에 나타납니다.



이 트라이어드에서 위협을 탐지하고 중지하기위한 지속적인 모니터링으로 위협 가시성을 높입니다. 따라서 조직은 공격 수명주기에서 더 많은 단계를 모니터링하여 위협을보다 성공적으로 식별하고 차단할 수 있습니다.

Cynet은 이러한 기능의 하위 집합으로 다음 기능과 함께 EDR 기술을 사용합니다.

고급 엔드 포인트 위협 탐지 -행동 분석 및 엔드 포인트의 지속적인 모니터링을 기반으로 완벽한 가시성을 확보하고 공격자가 어떻게 작동 할 수 있는지 예측합니다.
조사 및 검증 — 엔드 포인트에서 과거 또는 현재 사건 데이터를 검색 및 분석하고 경보를 검증하며 위협을 조사합니다. 이를 통해 대응 전에 위협을 확인할 수 있으므로 체류 시간이 단축되고보다 신속한 치료가 가능합니다.
신속한 구축 및 대응 — 단 2 시간 만에 수천 개의 엔드 포인트에 구축 할 수 있습니다. 그런 다음이를 사용하여 엔드 포인트에서 위협에 대한 수동 또는 자동 치료를 수행하고 공격으로 인한 피해를 최소화하며 악의적 인 활동을 방해 할 수 있습니다.
Cynet 360 위협 보호는 공격 탐지 및 예방 그 이상입니다. Cynet 조직을 사용하면 엔드 포인트, 호스트, 파일 및 네트워크와 같은 내부 환경을 사전에 모니터링 할 수 있습니다. 이를 통해 조직은 공격 범위와 여러 공격 가능성을 줄일 수 있습니다. 적극적인 공격과 관련하여 조직은 공격자의 존재를 완전히 근절하기 위해 공격자의 기능을 동봉해야합니다. 여기에는 손상된 사용자 비활성화, 악성 프로세스 및 파일 삭제, 감염된 호스트 격리 및 공격자가 제어하는 ​​트래픽 차단이 포함됩니다.

Cynet 360 보안 플랫폼에 대해 자세히 알아보십시오 .