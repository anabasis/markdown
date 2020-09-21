# 시작 및 확장

## About the Falcon Platform

### What is Falcon

팔콘은 무엇입니까

CrowdStrike Falcon is a SaaS-based, next generation endpoint protection solution that provides advanced detection, prevention, monitoring and search capabilities, allowing analysts to defend against sophisticated threats and adversaries. Falcon offers remote visibility across endpoints throughout an environment, enabling instant access to the "who, what, when, where, and how" of an attack. We collect and analyze more than 80 billion endpoint events each day from millions of sensors deployed across 176 countries. Falcon can help you protect your endpoints, whether you have just a few or hundreds of thousands.
CrowdStrike Falcon은 고급 탐지, 예방, 모니터링 및 검색 기능을 제공하는 SaaS 기반 차세대 엔드 포인트 보호 솔루션으로, 분석가가 정교한 위협 및 공격으로부터 방어 할 수 있도록합니다. Falcon은 환경 전체의 엔드 포인트에 대한 원격 가시성을 제공하여 공격의 "누가, 무엇을, 언제, 어디서, 어떻게"에 즉시 액세스 할 수 있습니다. 176 개국에 설치된 수백만 개의 센서에서 매일 800 억 개 이상의 엔드 포인트 이벤트를 수집하고 분석합니다. Falcon은 단지 수십만에 달하는 엔드 포인트를 보호하여 엔드 포인트를 보호 할 수 있습니다.

### How does Falcon work

팔콘은 어떻게 작동합니까?

Falcon consists of two components: the sensor and the cloud.
팔콘은 센서와 클라우드의 두 가지 구성 요소로 구성됩니다.

First, a lightweight sensor is deployed to every endpoint where it gathers appropriate system events from each host and takes proactive detection and prevention actions. The Falcon sensor detects and defends against attacks occurring on disk and in memory. The platform continuously watches for suspicious processes, events, and activities, wherever they may reside. Falcon also provides advanced prevention capabilities like custom allowing and blocking (also called "whitelisting" or "blacklisting"), malware blocking, exploit blocking, and IOA-based prevention (Indicators of Attack).

먼저 경량 센서가 모든 엔드 포인트에 배치되어 각 호스트에서 적절한 시스템 이벤트를 수집하고 사전에 감지 및 예방 조치를 취합니다. 팔콘 센서는 디스크와 메모리에서 발생하는 공격을 탐지하고 방어합니다. 이 플랫폼은 의심스러운 프로세스, 이벤트 및 활동이 어디에 있든지 지속적으로 감시합니다. Falcon은 또한 사용자 지정 허용 및 차단 ( "화이트리스트"또는 "블랙리스트"라고도 함), 맬웨어 차단, 악용 차단 및 IOA 기반 예방 (공격 표시기)과 같은 고급 예방 기능을 제공합니다.

Data gathered by the sensor is then transmitted continuously from the sensor to CrowdStrike’s Advanced Threat Intelligence Cloud, where CrowdStrike analyzes and draws links between events across the entire Falcon sensor community. These behavioral patterns are detected in real time via CrowdStrike’s Threat Graph data model, allowing analysts to detect new attacks, whether the attacks use malware or not.
센서가 수집 한 데이터는 센서에서 CrowdStrike의 고급 위협 인텔리전스 클라우드로 지속적으로 전송됩니다. 여기서 CrowdStrike는 전체 Falcon 센서 커뮤니티의 이벤트 간을 분석하고 연결합니다. 이러한 행동 패턴은 CrowdStrike의 Threat Graph 데이터 모델을 통해 실시간으로 탐지되므로 공격자가 맬웨어를 사용하는지 여부에 관계없이 분석가가 새로운 공격을 탐지 할 수 있습니다.

CrowdStrike provides you a suite of powerful investigation, prevention, detection, and sensor monitoring tools in the Falcon web interface—your command center for everything to do with Falcon. See the Falcon Console User Guide for an app-by-app walkthrough.
CrowdStrike는 Falcon 웹 인터페이스 (Falcon과 관련된 모든 명령 센터)에서 강력한 조사, 예방, 탐지 및 센서 모니터링 도구를 제공합니다. 앱별 연습 은 Falcon Console 사용 설명서 를 참조하십시오 .

### What detection capabilities does Falcon have

팔콘에는 어떤 탐지 기능이 있습니까?

For known threats, Falcon provides cloud-based antivirus (Cloud AV) and Indicators of Compromise (IOC) detection capabilities. For unknown and zero-day threats, Falcon applies IOA detection, using machine learning techniques to build predictive models that can detect never-before-seen malicious activities with high accuracy. Driven by CrowdStrike’s Threat Graph data model, this IOA analysis recognizes behavioral patterns to detect new attacks.

알려진 위협의 경우 Falcon은 클라우드 기반 바이러스 백신 (Cloud AV) 및 IOC (Indicator of Compromise) 탐지 기능을 제공합니다. 알려지지 않은 제로 데이 위협에 대해 Falcon은 머신 러닝 기술을 사용하여 IOA 탐지를 적용하여 예측할 수없는 악성 활동을 정확하게 탐지 할 수있는 예측 모델을 구축합니다. CrowdStrike의 Threat Graph 데이터 모델을 기반으로하는이 IOA 분석은 행동 패턴을 인식하여 새로운 공격을 탐지합니다.

### What is the Falcon Sensor

팔콘 센서 란 무엇입니까?

The sensor takes only minutes to deploy to your endpoints, and analysts monitor and manage the environment via the Falcon web interface, a powerful web portal. With Falcon, there are no controllers to be installed, configured, updated or maintained. There is no on-premise equipment. Falcon is a 100% cloud-based solution, offering Security as a Service to users.
센서는 엔드 포인트에 배포하는 데 몇 분 밖에 걸리지 않으며 분석가는 강력한 웹 포털 인 Falcon 웹 인터페이스를 통해 환경을 모니터링하고 관리합니다. Falcon을 사용하면 설치, 구성, 업데이트 또는 유지 관리 할 컨트롤러가 없습니다. 온 프레미스 장비가 없습니다. Falcon은 100 % 클라우드 기반 솔루션으로 사용자에게 보안 서비스를 제공합니다.

### Does the sensor integrate with SIEM technology?

센서가 SIEM 기술과 통합됩니까?

For Windows, Mac, and Linux hosts, CrowdStrike provides the Falcon SIEM Connector which allows you to send detections and audit events to your SIEM. The Falcon SIEM Connector integrates with HP ArcSight, IBM Q-Radar and Splunk. Additionally, CrowdStrike offers the Streaming API to enable integration with third-party SIEMs. For more information, see the SIEM Connector Feature Guide and Streaming API Event Dictionary.
Windows, Mac 및 Linux 호스트의 경우 CrowdStrike는 탐지 및 감사 이벤트를 SIEM에 보낼 수있는 팔콘 SIEM 커넥터를 제공합니다. Falcon SIEM 커넥터는 HP ArcSight, IBM Q-Radar 및 Splunk와 통합됩니다. 또한 CrowdStrike는 타사 SIEM과 통합 할 수 있도록 스트리밍 API를 제공합니다. 자세한 정보는 SIEM Connector 기능 안내서 및 스트리밍 API 이벤트 사전을 참조하십시오 .

### What operating systems does Falcon support?

팔콘은 어떤 운영 체제를 지원합니까?

For the most up to date list of supported operating systems, see the Deployment Guides:
지원되는 운영 체제의 최신 목록은 배포 안내서를 참조하십시오.

Windows
Mac
Linux
Mobile: iOS and Android

### What data does Falcon send to the cloud?

Falcon은 어떤 데이터를 클라우드로 전송합니까?

The Falcon platform is designed to maximize visibility into real-time and historical endpoint security events by gathering the event data necessary to identify, understand, and respond to attacks — but nothing more. The amount of data that a sensor transmits to the cloud varies depending on each host’s activity.
Falcon 플랫폼은 공격을 식별, 이해 및 대응하는 데 필요한 이벤트 데이터를 수집하여 실시간 및 과거 엔드 포인트 보안 이벤트에 대한 가시성을 극대화하도록 설계되었습니다. 센서가 클라우드로 전송하는 데이터의 양은 각 호스트의 활동에 따라 다릅니다.

This default set of system events is focused on process execution and is continually monitored for suspicious activity. When such activity is detected, additional data collection activities are initiated to better understand the situation and enable a timely response to the event. The specific data collected changes as CrowdStrike advances capabilities and in response to changes in the threat landscape.
이 기본 시스템 이벤트 세트는 프로세스 실행에 중점을두고 의심스러운 활동에 대해 지속적으로 모니터링됩니다. 이러한 활동이 감지되면 상황을보다 잘 이해하고 이벤트에 적시에 응답 할 수 있도록 추가 데이터 수집 활동이 시작됩니다. 특정 데이터는 CrowdStrike가 기능을 발전시키고 위협 환경의 변화에 ​​대응하여 변경 사항을 수집했습니다.

### How does Falcon safeguard customer data?

Falcon은 고객 데이터를 어떻게 보호합니까?

CrowdStrike uses an SSL/TLS-encrypted tunnel to send data between the sensor and the cloud.
CrowdStrike는 SSL / TLS 암호화 터널을 사용하여 센서와 클라우드간에 데이터를 보냅니다.

Additionally, CrowdStrike uses certificate pinning on the sensor side. This means that a sensor will only communicate with cloud endpoints that have a known certificate. CrowdStrike also provides you the ability to allow our cloud endpoints in your firewalls to ensure that your Falcon sensors only communicate with CrowdStrike.
또한 CrowdStrike는 센서 측에서 인증서 고정을 사용합니다. 이는 센서가 알려진 인증서가있는 클라우드 엔드 포인트와 만 통신 함을 의미합니다. 또한 CrowdStrike는 방화벽에서 클라우드 엔드 포인트가 Falcon 센서가 CrowdStrike 와만 통신하도록 보장 할 수있는 기능을 제공합니다.

Next, every customer is assigned a unique customer ID. Because CrowdStrike tags customer data with a unique customer ID, any query or exchange of data will be limited to the scope of a specific customer ID, which further secures data.

다음으로 모든 고객에게 고유 한 고객 ID가 할당됩니다. CrowdStrike는 고유 한 고객 ID로 고객 데이터에 태그를 지정하므로 모든 쿼리 또는 데이터 교환은 특정 고객 ID의 범위로 제한되어 데이터를 더욱 안전하게 보호합니다.

Once data is in the CrowdStrike cloud, all data, including backups, are encrypted with industry-standard AES256 encryption.
데이터가 CrowdStrike 클라우드에 있으면 백업을 포함한 모든 데이터는 업계 표준 AES256 암호화로 암호화됩니다.

CrowdStrike also limits employee access to customer data to individuals with a business need. This includes Customer Support and Falcon Overwatch. Moreover, direct access to underlying systems is limited only to engineers with a business need. Access is protected by encrypted VPN and multi-factor authentication.

CrowdStrike는 또한 비즈니스 요구가있는 개인에 대한 직원의 고객 데이터 액세스를 제한합니다. 여기에는 고객 지원 및 팔콘 오버 워치가 포함됩니다. 또한 기본 시스템에 대한 직접 액세스는 비즈니스 요구가있는 엔지니어로만 제한됩니다. 암호화 된 VPN 및 다단계 인증으로 액세스를 보호합니다.

## Getting Falcon up and running

This high-level walkthrough will help guide you a basic Falcon implementation for Windows, Mac, and Linux endpoints, from installing your first sensor to scaling up to your whole environment. For information about iOS and Android endpoint protection, see the Falcon for Mobile Deployment Guide.
이 고급 연습은 첫 번째 센서 설치에서 전체 환경에 이르기까지 Windows, Mac 및 Linux 엔드 포인트에 대한 기본 Falcon 구현을 안내합니다. iOS 및 Android 엔드 포인트 보호에 대한 정보는 Falcon for Mobile Deployment Guide를 참조하십시오 .

### Before You Begin

Have two devices:

A test device running Windows. You'll install the Falcon sensor on this device. For general use, Falcon also supports Mac and Linux devices.

A management device with Google Chrome. This device is used to access the Falcon console.

Set up your Falcon account, including two-factor authentication (2FA), using the link in your activation email.


Download and Install the Sensor
The Falcon sensor is a lightweight agent that you install on each device. When a device has a Falcon sensor installed, we call that device a host. Each sensor detects and prevents malicious activity on a host, according to the policies that you’ll configure later. You use the Falcon console to manage your hosts.

In this example process, download and manually install the Falcon sensor on your test device.

1. DOWNLOAD THE SENSOR INSTALLER
Use your management device to download the sensor installer:

In the Falcon console, go to Hosts > Sensor Downloads.

If you're installing the sensor on a Mac or Linux device, copy your customer ID checksum. This ID is required when the installer runs. If you're installing the sensor on a Windows device, the customer ID checksum will automatically populate.

Download the installer file for your test device’s platform.

Transfer the installer to your test device using a USB drive or another file transfer method.

2. RUN THE SENSOR INSTALLER
On your test device, run the sensor installer using an account with administrative privileges.

Open the sensor installer and follow the prompts.

If you're installing the sensor on a Mac or Linux device, enter your customer ID checksum when prompted. If you don't have your customer ID checksum, it can be found on the Hosts > Sensor Downloads page.

Confirm the sensor is running using this command at your host’s command line interface: sc query csagent

3. SCALE UP
Learn about deploying at scale, using tools like SCCM or JAMF, configuring images for cloning, and more from our full deployment guides:

Windows
Mac
Linux

Set Up a Host Group
Groups are collections of hosts in your organization. Using groups, you can control endpoint protection  and sensor upgrades for each of your hosts. For example, you might create separate groups for servers, general users' devices, and your executives' devices.

In this example, create a group and assign your host by platform.

top
1. CREATE A HOST GROUP
In the Falcon console, go to Hosts > Groups.

Click Add New Group in the upper-right corner.

Enter a name and an optional description.

Select Dynamic as your group type. This means the group automatically adds new hosts when they match the group's assignment rule.

top
2. PUT A HOST IN THE GROUP
In your host group’s details:

Click Edit near Assignment rule.

In the OS Version column of the filter bar, select your host’s operating system. When you do, the host is added to the list of Hosts for this group.

Click Save in the upper-right corner.

top
3. SCALE UP
Host groups are essential when your environment has dozens (to hundreds of thousands) of hosts. Read Host and Host Group Management for information about:

Assigning hosts to dynamic groups using other attributes, such as their Organizational Unit (OU) in Active Directory

Assigning hosts to static groups by manually selecting them

Use host groups to keep your hosts running up-to-date sensor versions. Read Sensor Update Policies for more information.

top
Review the Default Prevention Policy
Prevention policies are sets of rules that control how Falcon responds to potentially malicious activity identified by your sensors.

1. NAVIGATE TO THE DEFAULT POLICY
When you created your group, Falcon automatically assigned it to use the Default Policy, which is detection only. Review the default policy using the Falcon console:

Go to Configuration > Prevention Policies.

Click Default Policy.

You can examine the controls in the Default Policy to understand its settings. Later, you can create your own policies to be as cautious or as aggressive as your environment requires.

2. SCALE UP
When you have many groups, you want more fine-tuned control over the detections and preventions triggered on your hosts. This introductory guide shows you how to start small with Falcon, but Falcon can detect and prevent much more sophisticated attacks on all the endpoints in your environment. Read Detection and Prevention Policies for more information on configuring prevention policies and custom detection and prevention settings:

File Exclusions

Prevention Hashes

Custom IOA Rules

top
Watch the Sensor Detect an Event
Falcon sensors detect malicious activity, respond according to your policies, and report the activity to the CrowdStrike Cloud. You can see information on this malicious activity in the Falcon console.

1. RUN A SIMULATED ATTACK
To see an example of what a detection looks like, run a simulated but harmless attack on your host:

Open a command prompt.

Run this command: choice /M crowdstrike_sample_detection

2. VIEW THE DETECTION
Return to the Falcon console on your management device to see that the Falcon sensor detected this attack.

Go to Activity > Detections on your management device.

Click the line item for the detection you triggered.

Review a summary of the event and investigate the sequence of events on your host that led to the attack.

3. SCALE UP
Read Detections Monitoring for more about understanding the detections and preventions in your environment and learn how to set up custom alerts to receive emails about your detections.

top
More Info
Visit the Support Portal to submit questions and find more info.

Sign up for our alert system to receive critical updates. We'll notify you by email or SMS to inform you of new product releases, upcoming features, and status updates on our cloud services.

Watch videos, read data sheets, and view webinars in our Resource Center.

Learn about adversaries that may threaten your organization or industry from our Intelligence profiles.