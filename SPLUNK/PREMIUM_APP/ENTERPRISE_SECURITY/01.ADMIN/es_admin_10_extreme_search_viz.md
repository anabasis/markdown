# Extreme Search Visualization

- <https://splunkbase.splunk.com/app/2855/>

## Overview

Extreme Search Visualization (XSV), is designed as a "helper" app for Scianta Analytics' Extreme Search for Splunk. Co-developed by Scianta Analytics and Splunk Inc., Extreme Search (XS) is now part of the Splunk App for Enterprise Security (ES). XSV provides a robust set of tools to help you create, manage and explore Extreme Search knowledge objects. Additionally, XSV provides valuable documentation about Extreme Search beyond what is included in the app.

The target audience for this release includes: Splunk & Splunk Partner Professional Services Engineers, Splunk SEs, end users working with the Splunk App for Enterprise Security, and analytics professionals who would like to leverage the power of qualitative expression in their solutions.

It is not necessary to use the XSV app or the Splunk App for Enterprise Security to use Extreme Search. Since Extreme Search is implemented as a set of extensions to Splunk's search language, it is available to any Splunk app.

XSV (Extreme Search Visualization)는 Scianta Analytics의 Extunk Search for Splunk 용 "도우미"응용 프로그램으로 설계되었습니다. Scianta Analytics와 Splunk Inc.가 공동 개발 한 Extreme Search (XS)는 이제 Splunk App for Enterprise Security (ES)의 일부입니다. XSV는 Extreme Search 지식 객체를 생성, 관리 및 탐색 할 수있는 강력한 도구 세트를 제공합니다. 또한 XSV는 앱에 포함 된 것 이상의 Extreme Search에 대한 중요한 문서를 제공합니다.

이 릴리스의 대상 고객은 Splunk 및 Splunk 파트너 전문 서비스 엔지니어, Splunk SE, 엔터프라이즈 보안을위한 Splunk 응용 프로그램을 사용하는 최종 사용자 및 솔루션에서 질적 표현의 힘을 활용하고자하는 분석 전문가입니다.

Extreme Search를 사용하기 위해 XSV 앱이나 엔터프라이즈 보안 용 Splunk 앱을 사용할 필요는 없습니다. Extreme Search는 Splunk의 검색 언어에 대한 확장 집합으로 구현되므로 모든 Splunk 응용 프로그램에서 사용할 수 있습니다.

## Introduction

Extreme Search Visualization (XSV), is designed as a "helper" app for Scianta Analytics' Extreme Search for Splunk. Co-developed by Scianta Analytics and Splunk Inc., Extreme Search (XS) is now part of the Splunk App for Enterprise Security (ES). XSV provides a robust set of tools to help you create, manage and explore Extreme Search knowledge objects. Additionally, XSV provides valuable documentation about Extreme Search beyond what is included in the app.

소개
XSV (Extreme Search Visualization)는 Scianta Analytics의 Extunk Search for Splunk 용 "도우미"응용 프로그램으로 설계되었습니다. Scianta Analytics와 Splunk Inc.가 공동 개발 한 Extreme Search (XS)는 이제 Splunk App for Enterprise Security (ES)의 일부입니다. XSV는 Extreme Search 지식 객체를 생성, 관리 및 탐색 할 수있는 강력한 도구 세트를 제공합니다. 또한 XSV는 앱에 포함 된 것 이상의 Extreme Search에 대한 중요한 문서를 제공합니다.

## What is Extreme Search

The Extreme Search Engine is an integrated collection of powerful cognitive-computing-based analytics functions engineered for speed and transportability. The suite provides fast, flexible, and comprehensive statistical reasoning, predictive analytics and conceptual query capabilities in any computing environment. Extreme Search for Splunk is an implementation of the Scianta Extreme Search Engine which is engineered specifically for Big Data using the Splunk platform. The engine is optimized to run multiple, parallel threads on each Splunk indexer and is compiled to operate natively in Linux, Windows or Mac OS. This allows Extreme Search to deliver extraordinary performance, even on gigantic data sets.

익스트림 검색이란 무엇입니까?
익스트림 검색 엔진은 속도와 이동성을 위해 설계된 강력한인지 컴퓨팅 기반 분석 기능의 통합 모음입니다. 이 제품군은 모든 컴퓨팅 환경에서 빠르고 유연하며 종합적인 통계적 추론, 예측 분석 및 개념적 쿼리 기능을 제공합니다. Extunk Search for Splunk는 Splunk 플랫폼을 사용하여 Big Data 용으로 특별히 설계된 Scianta Extreme 검색 엔진을 구현 한 것입니다. 이 엔진은 각 Splunk 인덱서에 여러 개의 병렬 스레드를 실행하도록 최적화되어 있으며 Linux, Windows 또는 Mac OS에서 기본적으로 작동하도록 컴파일되어 있습니다. 이를 통해 Extreme Search는 거대한 데이터 세트에서도 탁월한 성능을 제공합니다.

Extreme Search is, first and foremost, a powerful cognitive computing engine. It is designed to "think" the way you think and to "understand" concept-based queries written in natural language.

익스 트림 검색은 무엇보다도 강력한인지 엔진입니다. 자연 언어로 작성된 개념 기반 쿼리를 생각하고 이해하는 방식을 "생각"하도록 설계되었습니다.

## Converse with your data™

Splunk is arguably the world's most powerful Big Data engine. Extreme Search transforms Splunk into the world's most powerful human-centered, cognitive computing platform for Big Data. Extreme Search understands concepts in the same way you understand them. This allows you to converse with your data the same way you converse with your co-workers, using powerful qualitative concepts in place of arcane quantitative parameters.

귀하의 데이터 ™와 대화하십시오
Splunk은 틀림없이 세계에서 가장 강력한 Big Data 엔진입니다. Extreme Search는 Splunk를 Big Data를위한 세계에서 가장 강력한 인간 중심의인지 적 컴퓨팅 플랫폼으로 변환합니다. 익스 트림 검색은 개념을 이해하는 것과 같은 방식으로 개념을 이해합니다. 이를 통해 비밀스러운 정량적 매개 변수 대신 강력한 질적 개념을 사용하여 동료와 이야기하는 것과 같은 방식으로 데이터와 대화 할 수 있습니다.

Why is this important? Because it allows regular users to gain deep insight from their data simply by asking questions in a way they understand.

왜 이것이 중요한가? 일반 사용자가 이해할 수있는 방식으로 질문하는 것만으로 데이터에서 심층적 인 통찰력을 얻을 수 있기 때문입니다.

## The Value Proposition in Enterprise Security

In addition to supporting qualitative expression in Splunk searches, Extreme Search is capable of periodically refining the meaning of concepts based on a changing environment. Splunk uses this capability to implement Dynamic Thresholding. In the Splunk App for Enterprise Security, this allows the use of semantic terms as thresholds, in place of hard-coded numerical values. For example, a correlation search in ES might produce a notable event if the number of infected hosts is "high". Prior to Extreme Search, that search contained some arbitrary value (200, for example) as a threshold value for a "high number of infected hosts". Is 200 an appropriate "high" threshold value? That depends entirely on the environment.

엔터프라이즈 보안의 가치 제안
익스트림 서치는 Splunk 검색에서 질적 표현을 지원하는 것 외에도 변화하는 환경을 기반으로하는 개념의 의미를 주기적으로 정제 할 수 있습니다. Splunk는이 기능을 사용하여 동적 임계 값을 구현합니다. 엔터프라이즈 보안 용 Splunk 응용 프로그램에서는 하드 코딩 된 숫자 값 대신 임계 값으로 의미 론적 용어를 사용할 수 있습니다. 예를 들어, ES의 상관 관계 검색은 감염된 호스트 수가 "높음"인 경우 주목할만한 이벤트를 생성 할 수 있습니다. Extreme Search 이전에 검색에는 "높은 수의 감염된 호스트"에 대한 임계 값으로 임의의 값 (예 : 200)이 포함되었습니다. 200이 적절한 "높은"임계 값입니까? 그것은 전적으로 환경에 달려 있습니다.

One of the challenges in implementing a complex, powerful Splunk app like ES has been the time it takes to review every correlation search in collaboration with a customer subject matter expert, replacing these arbitrary hard-coded threshold parameters with values that make sense for the organization. With Extreme Search, the correlation search might simply look for values that are "above low". Then a simple search can map the concept "low" to the organization's actual data. This has enabled a very real acceleration of time-to-value when deploying the Splunk App for Enterprise Security. ES can now be customized for a complex enterprise environment in hours instead of weeks.

ES와 같이 복잡하고 강력한 Splunk 앱을 구현할 때의 과제 중 하나는 고객 주제 전문가와 협력하여 모든 상관 관계 검색을 검토하고 이러한 임의의 하드 코딩 된 임계 값 매개 변수를 조직에 적합한 값으로 대체하는 데 걸리는 시간이었습니다 . Extreme Search를 사용하면 상관 관계 검색에서 "낮음"보다 높은 값을 찾을 수 있습니다. 그런 다음 간단한 검색을 통해 개념을 "낮음"으로 조직의 실제 데이터에 매핑 할 수 있습니다. 이를 통해 Splunk App for Enterprise Security를 ​​배포 할 때 가치 실현 시간이 매우 단축되었습니다. ES는 복잡한 엔터프라이즈 환경을 위해 몇 주가 아닌 몇 시간 만에 사용자 정의 할 수 있습니다.

Extreme Search also provides a very high level of granularity in setting thresholds. The notion of a "high number of login attempts" is unlikely to be the same on a Monday morning at 9AM as it would be on a Sunday at 2AM. Extreme Search can classify the context for number of login attempts based on any secondary control parameter: time of day; day of week; network type; product category; employee role, etc.. This ability to support Highly Granular, User-Defined, Dynamic Thresholding is one of the important reasons Extreme Search is now included with the Splunk App for Enterprise Security.

익스 트림 검색은 또한 임계 값 설정에서 매우 세분화 된 수준을 제공합니다. "높은 로그인 시도 횟수"의 개념은 월요일 오전 2시에 일요일과 같이 오전 9시에 동일하지 않을 수 있습니다. Extreme Search는 보조 제어 매개 변수 인 시간대에 따라 로그인 시도 횟수를 컨텍스트로 분류 할 수 있습니다. 요일; 네트워크 유형; 제품 카테고리; 직원 역할 등이 포함됩니다. 고도로 세밀하고 사용자 정의 된 동적 임계 값을 지원하는이 기능은 엔터프라이즈 보안을위한 Splunk 응용 프로그램에 익스트림 검색이 포함 된 중요한 이유 중 하나입니다.

Command Categories

Extreme Search commands can be divided into three operational categories:

- Conceptual Search
  Qualitative, concept-based data exploration.
- Statistical Reasoning
  Powerful regression and correlation tools for data analysis.
- Predictive Analytics
  State-of-the-art machine learning tools.

The Command Reference documentation page of this app describes each command in each of these three categories.

명령 카테고리
익스 트림 검색 명령은 세 가지 운영 카테고리로 나눌 수 있습니다.

개념 검색
정 성적, 개념 기반 데이터 탐색.

통계적 추론
데이터 분석을위한 강력한 회귀 분석 및 상관 분석 도구.

예측 분석
최첨단 기계 학습 도구.
이 응용 프로그램의 Command Reference 문서 페이지는이 세 가지 범주의 각 명령을 설명합니다.

### Conceptual Search

#### Terminology

To understand Extreme Search's concept-based search, it is useful to understand some terminology used in the product and its documentation:

개념 검색
술어
Extreme Search의 개념 기반 검색을 이해하려면 제품 및 설명서에 사용 된 몇 가지 용어를 이해하는 것이 유용합니다.

#### Concept

An idea represented by a descriptive semantic term. In Extreme Search, these terms are usually user defined as part of a Context. Tall, Short, Fast and Slow are Semantic Terms used to describe Concepts.

개념
설명 적 의미 론적 용어로 표현되는 아이디어. 익스 트림 검색에서 이러한 용어는 대개 컨텍스트의 일부로 사용자 정의됩니다. 크고, 짧고, 빠르며 천천히 개념을 설명하는 데 사용되는 시맨틱 용어입니다.

#### Context

A collection of terms that form a conceptually coherent view of a knowledge domain. Height might be a Context comprised of the Terms Tall and Short. Speed might be a Context comprised of the Terms Fast, Typical and Slow.

문맥
지식 도메인의 개념적 일관된 관점을 형성하는 용어 집합. 높이는 용어 Tall과 Short로 구성된 컨텍스트 일 ​​수 있습니다. 속도는 빠른, 표준 및 느린 조건으로 구성된 컨텍스트 일 ​​수 있습니다.

#### Semantic Term

A linguistic representation of a Concept. The semantic term Tall might be used to represent the Concept of people of large physical stature as a part of the Context Height.

For an introduction to conceptual search, click on "Learn More About Conceptual Search" on the "Overview" page in the XSV app, or select "Intro to Conceptual Search" from the XSV menu.

시맨틱 용어
개념의 언어 표현. 시맨틱 용어 인 Tall은 Context Height의 일부로서 큰 신체적 인 사람의 개념을 나타 내기 위해 사용될 수 있습니다.

개념 검색에 대한 소개를 보려면 XSV 앱의 '개요'페이지에서 '개념 검색에 대해 자세히 알아보기'를 클릭하거나 XSV 메뉴에서 '개념 소개 소개'를 선택하십시오.

### Statistical Reasoning

Scianta Extreme Search includes powerful statistical reasoning functions that facilitate analysis of very large data sets quickly and intuitively within Splunk. Extreme Search supports the following categories of Statistical Reasoning functions:

- Linear Regression
- Non-linear Regression
- Auto Regression
- Correlation

Learn more about Statistical Reasoning in Extreme Search in the Command Reference in the XSV app.

통계적 추론
Scianta Extreme Search에는 매우 큰 데이터 세트를 Splunk에서 빠르고 직관적으로 분석 할 수있는 강력한 통계적 추론 기능이 포함되어 있습니다. 익스트림 검색은 다음과 같은 통계 추론 기능 범주를 지원합니다.

선형 회귀
비선형 회귀 분석
자동 회귀 분석
상관 관계
Extreme Search의 통계 추론에 대한 자세한 내용은 XSV 응용 프로그램의 Command Reference를 참조하십시오.

### Predictive Analytics

The Scianta Extreme Search engine is part of the Scianta Analytics Cognitive Computing Suite. Our Cognitive Computing Suite delivers industry-leading, proprietary cognitive modeling, machine learning and predictive analytics to the world of Big Data. Extreme Search for Splunk includes concept-based predictive analytics functions that run natively, within Splunk.

Learn more about Predictive Analytics in Extreme Search for Splunk in the Command Reference in the XSV app.

예측 분석
Scianta Extreme 검색 엔진은 Scianta Analytics Cognitive Computing Suite의 일부입니다. Cognitive Computing Suite는 업계 최고의 독점적 인인지 모델링, 기계 학습 및 예측 분석을 빅 데이터 세계에 제공합니다. Splunk의 Extreme Search에는 기본적으로 Splunk 내에서 실행되는 개념 기반 예측 분석 기능이 포함됩니다.

XSV 앱의 Command Reference에서 Splunk를위한 Extreme Search의 Predictive Analytics에 대해 더 자세히 알아보십시오.

## Using Extreme Search

We encourage you to review the Extreme Search Command Reference. Taken together, these commands deliver to Splunk users the power of Scianta Analytics' cognitive-computing-based conceptual search, statistical reasoning and predictive analytics technology natively, within the Splunk platform. As a query processor, the components can also be combined and used as a data filter to collect, filter, and rank information based on the qualitative semantics associated with each data element. Because Extreme Search is tightly integrated with Splunk, results are delivered alongside the results of native Splunk search commands.

익스트림 검색 사용
Extreme Search Command Reference를 검토하는 것이 좋습니다. 이러한 명령은 Splunk 사용자에게 Scianta Analytics의 인식 컴퓨팅 기반 개념 검색, 통계적 추론 및 예측 분석 기술의 힘을 Splunk 플랫폼에서 제공합니다. 쿼리 프로세서로서 구성 요소를 결합하고 데이터 필터로 사용하여 각 데이터 요소와 관련된 정 성적 의미론을 기반으로 정보를 수집, 필터링 및 순위를 매길 수 있습니다. Extreme Search는 Splunk와 긴밀하게 통합되어 있기 때문에 결과는 기본 Splunk 검색 명령의 결과와 함께 제공됩니다.

Extreme Search is implemented as a set of extensions to Splunk's Search Processing Language (SPL). All Extreme Search functions may be entered as commands directly within the Splunk Search bar, or within scheduled searches and reports. Results are displayed within the Splunk web interface in the same manner as any other Splunk search. It is not necessary to use a specific Splunk app or an external interface to take advantage of conceptual search. Some Scianta Analytics cognitive computing suites, such as Scianta Analytics Extreme Vigilance™ , execute Extreme Search functions and display results through their own user interfaces. The Splunk App for Enterprise Security has integrated Extreme Search qualitative expression within many ES searches, reports and dashboards. Please see the documentation for these systems to learn how they work with Extreme Search for Splunk. Documentation on the use of Extreme Search within the Splunk App for Enterprise Security is [available online here](http://docs.splunk.com/Documentation/ES/3.3.1/User/ExtremeSearch).

Extreme Search는 Splunk의 SPL (Search Processing Language)에 대한 확장 집합으로 구현됩니다. 모든 익스트림 검색 기능은 Splunk Search 바에서 또는 예약 검색 및 보고서 내에서 직접 명령으로 입력 할 수 있습니다. 결과는 다른 Splunk 검색과 동일한 방식으로 Splunk 웹 인터페이스에 표시됩니다. 개념 검색을 이용하려면 특정 Splunk 앱이나 외부 인터페이스를 사용할 필요가 없습니다. Scianta Analytics Extreme Vigilance ™와 같은 일부 Scianta Analytics인지 컴퓨팅 제품군은 Extreme Search 기능을 실행하고 자체 사용자 인터페이스를 통해 결과를 표시합니다. Splunk App for Enterprise Security는 많은 ES 검색, 보고서 및 대시 보드 내에서 Extreme Search 질적 표현을 통합했습니다. Splunk 용 익스트림 서치 (Extreme Search for Splunk)에서 이들 시스템이 어떻게 작동하는지 알아 보려면이 시스템의 설명서를 참조하십시오. 엔터프라이즈 보안을위한 Splunk 응용 프로그램 내에서 익스트림 검색 사용에 관한 문서는 여기에서 온라인으로 볼 수 있습니다.

### Extreme Search Architectural Hierarchy

Knowledge objects in Extreme Search follow this organizational hierarchy:

익스트림 검색 아키텍처 계층 구조
익스트림 검색의 지식 객체는 다음과 같은 조직 계층 구조를 따릅니다.

#### App

Any Splunk App may contain Extreme Search knowledge objects.

앱
모든 Splunk 앱에는 익스트림 검색 지식 객체가 포함될 수 있습니다.

#### Container

An App may contain one or more XS Containers. A Container is a special kind of CSV-formatted lookup file designed to contain one or more Contexts. Containers are generally transparent to the XS knowledge object hierarchy. They can best be thought of as a collection of Contexts.

컨테이너
앱은 하나 이상의 XS 컨테이너를 포함 할 수 있습니다. 컨테이너는 하나 이상의 컨텍스트를 포함하도록 설계된 특별한 종류의 CSV 형식 조회 파일입니다. 컨테이너는 일반적으로 XS 지식 객체 계층에 투명합니다. 그들은 컨텍스트의 콜렉션으로 가장 잘 생각할 수 있습니다.

#### Context(컨텍스트)

Containers contain one or more Contexts. A Context is a semantically coherent set of Concepts. All of the Concepts assembled into a particular Context must apply to the same knowledge domain. The Concepts Tall and Short may share a Context. Tall and Fast may not.

문맥
컨테이너는 하나 이상의 컨텍스트를 포함합니다. 컨텍스트는 의미 론적으로 일관된 개념 집합입니다. 특정 컨텍스트로 모아진 모든 개념은 동일한 지식 도메인에 적용되어야합니다. Concepts Tall과 Short는 컨텍스트를 공유 할 수 있습니다. 키 크고 빠를 수도 없습니다.

#### Context Class

Contexts may be classified based upon the value of a secondary field. If a Context contains Concepts associated with Network Latency, you might define a Context Class for each network_type and a Default Class that applies to all network_types. The Classes would be named based upon the potential values of the network_type field. The value of the network_type field can then be used to select the appropriate Context to be used in your search.

컨텍스트 클래스
컨텍스트는 2 차 필드의 값에 따라 분류 될 수 있습니다. 컨텍스트에 네트워크 지연과 관련된 개념이 포함되어 있으면 각 network_type에 대한 컨텍스트 클래스와 모든 network_type에 적용되는 기본 클래스를 정의 할 수 있습니다. 클래스는 network_type 필드의 잠재적 인 값을 기반으로 명명됩니다. 그런 다음 network_type 필드의 값을 사용하여 검색에 사용할 적절한 컨텍스트를 선택할 수 있습니다.

#### Concept(컨셉)

A Concept is a descriptive semantic term. It is represented as a two-dimensional array of points. The X axis corresponds to the value of the field in question. The Y axis represents the membership of that value in the Concept, stored as a value from 0 to 1.

개념
개념은 기술적으로 의미있는 용어입니다. 점의 2 차원 배열로 표현됩니다. X 축은 해당 필드의 값에 해당합니다. Y 축은 0에서 1 사이의 값으로 저장된 개념의 해당 값의 구성원을 나타냅니다.

![Extreme Search Knowlegde Object Hierarchy](.\images\e19849d6-4cdc-11e5-bb9f-02e61222c923.png)

The XSV Context Explorer dashboard allows you to visually navigate Extreme Search knowledge objects using this hierarchy.

XSV Context Explorer 대시 보드를 사용하면이 계층을 사용하여 Extreme Search 지식 객체를 시각적으로 탐색 할 수 있습니다.

![Concept Explorer](.\images\a91513d6-4d78-11e5-88a0-063854888a19.jpeg)

Extreme Search, as packaged with the Splunk App for Enterprise Security, is a Splunk Supporting Add-on called "Splunk_SA_ExtremeSearch" at etc/apps/Splunk_SA_ExtremeSearch. If installed as a stand-alone app, you'll find it at etc/apps/xtreme.
Documentation and Help

익스트림 서치 (Extreme Search)는 엔터프라이즈 보안을위한 스플 런크 앱과 함께 패키지로되어 있으며, Splunk_SA_ExtremeSearch라는 부가 기능은 etc / apps / Splunk_SA_ExtremeSearch에있다. 독립 실행 형 응용 프로그램으로 설치 한 경우 etc / apps / xtreme에서 찾을 수 있습니다.
문서 및 도움말

NOTE: XSV requires Extreme Search version 6.0.6 or later, which is included in ES version 3.3.1 and later. Note that it is not necessary to use the Extreme Search Visualization application or the Splunk App for Enterprise Security to use Extreme Search. Since Extreme Search is implemented as a set of extensions to Splunk's search language, once it is installed, it is available to any Splunk app.

참고 : XSV에는 ES 버전 3.3.1 이상에 포함 된 Extreme Search 버전 6.0.6 이상이 필요합니다. Extreme Search Visualization 응용 프로그램이나 Enterprise Security 용 Splunk 응용 프로그램을 사용하여 Extreme Search를 사용할 필요는 없습니다. Extreme Search는 Splunk의 검색 언어에 대한 확장 집합으로 구현되므로 일단 설치하면 모든 Splunk 응용 프로그램에서 사용할 수 있습니다.

Detailed information about each Extreme Search XS and XSV command is provided within your Splunk environment by Splunk's interactive help system. In addition, the Extreme Search Visualization application view includes Help pages like this one within the application view. Some important Help pages include:

각 Extreme Search XS 및 XSV 명령에 대한 자세한 정보는 Splunk의 대화식 도움말 시스템을 통해 Splunk 환경에서 제공됩니다. 또한 익스트림 검색 시각화 응용 프로그램보기에는 응용 프로그램보기에서 이와 같은 도움말 페이지가 포함됩니다. 몇 가지 중요한 도움말 페이지는 다음과 같습니다.

```txt
Introduction to Conceptual Search
Hedges and Synonyms
Using Extreme Search Visualization
Instant Anomaly Detection
Extreme Search Command Reference
Scianta Online Docs: Extreme Search Visualization
Splunk Online Docs: Extreme Search
```

```txt
개념 검색 소개
헤지스와 동의어
극단적 인 검색 시각화 사용
즉시 이상 탐지
익스 트림 검색 명령 참조
Scianta Online Docs : 극단적 인 검색 시각화
Splunk 온라인 문서 : 익스트림 검색
```

VERSION: 20151104.1
Custom application development for Splunk by Concanon LLC.
Some portions of Extreme Search for Splunk are patented under US Patent 9,087,090.
Copyright 2015, 2016 Scianta Analytics LLC. All Rights Reserved.

버전 : 20151104.1
Concanon LLC의 Splunk 사용자 정의 응용 프로그램 개발
Extron Search for Splunk의 일부는 미국 특허 9,087,090에 따라 특허를 받았습니다.
저작권 2015, 2016 Scianta Analytics LLC. 판권 소유.