# Splunk SDK for Python

## Overview

- What's new
- Python 2.7 End-of-Life Statement

## Getting started

- Requirements
- Installation
- Utilities

## Examples

- Command line
- Unit tests
- How to
- Connect to Splunk Enterprise
- Work with saved searches
- Run searches and jobs
- Create modular inputs
- Create custom search commands
- Display search results
- Get data into Splunk Enterprise
- Work with users, roles, and storage passwords

## Troubleshooting

## Python API Reference

--------------------------------------------------------

## Overview(상세내용)

The Splunk® Software Development Kit (SDK) for Python is open source and uses the Apache v2.0 license. If you want to make a code contribution, go to the Open Source page for more information.

Python 용 Splunk® SDK (Software Development Kit)는 오픈 소스이며 Apache v2.0 라이센스를 사용합니다. 코드를 작성하려면 오픈 소스 페이지로 이동하십시오.

This overview tells you more about:

- [What you can do with the Splunk SDK for Python](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#whattodo)
- [The Splunk SDK for Python architecture](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#architecture)

### What you can do with the Splunk SDK for Python

With the Splunk SDK for Python you can write Python applications to programmatically interact with the Splunk engine. The SDK is built on top of the REST API, providing a wrapper over the REST API endpoints. So that means with fewer lines of code, you can write applications that:

Python 용 Splunk SDK를 사용하면 Python 응용 프로그램을 작성하여 프로그래밍 방식으로 Splunk 엔진과 상호 작용할 수 있습니다. SDK는 REST API 맨 위에 빌드되어 REST API 엔드 포인트에 대한 랩퍼를 제공합니다. 따라서 코드의 줄이 적어지면 다음과 같은 응용 프로그램을 작성할 수 있습니다.

- Search your data, run saved searches, and work with search jobs.
- Manage Splunk configurations and objects.
- Integrate search results into your applications.
- Log directly to Splunk.
- Present a custom UI.

- 데이터 검색, 저장된 검색 실행 및 검색 작업.
- Splunk 구성 및 개체를 관리합니다.
- 검색 결과를 응용 프로그램에 통합하십시오.
- Splunk에 직접 로그.
- 사용자 정의 UI를 제시하십시오.

In addition to creating Python applications, you can also integrate Splunk data with other reporting tools. Here's an example of an application that uses a Leftronic Dashboard to show real-time Twitter data that was indexed using the Twitted example in this SDK:

Python 응용 프로그램을 만드는 것 외에도 Splunk 데이터를 다른 보고도구와 통합 할 수 있습니다. 다음은 이 SDK의 Twitted 예제를 사용하여 인덱싱된 실시간 Twitter 데이터를 표시하기 위해 Leftronic 대시보드를 사용하는 응용 프로그램의 예입니다.

![SDK](./images/leftronic_sm.jpg)

For more examples of applications created with Splunk SDKs, see the [Python code examples](https://github.com/splunk/splunk-sdk-python/tree/master/examples) on GitHub.

### The Splunk SDK for Python architecture

This section describes the following parts of the Splunk SDK for Python architecture:

- [Modules](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#modules)
- [The Service class](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#serviceclass)
- [Entities and collections](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#entitiescollections)
- [State caching](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#statecaching)
- [Namespaces](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#namespaces)

### Modules

Each of the modules in the Splunk SDK for Python can be used independently:

Python 용 Splunk SDK의 각 모듈은 독립적으로 사용할 수 있습니다.

- The binding module provides an abstraction layer over raw HTTP.
- The client module provides an abstraction layer over the Splunk REST API.
- The results module provides a Splunk-specific streaming XML reader.
- The data module converts Atom Feed data to a Pythonic format.

- 바인딩 모듈은 원시 HTTP를 통해 추상화 계층을 제공합니다.
- 클라이언트 모듈은 Splunk REST API를 통해 추상화 계층을 제공합니다.
- 결과 모듈은 Splunk 특정 스트리밍 XML 리더를 제공합니다.
- 데이터 모듈은 Atom Feed 데이터를 Pythonic 형식으로 변환합니다.

#### Binding module

The binding module (splunklib.binding) provides a thin abstraction over raw HTTP that:

바인딩 모듈 (splunklib.binding)은 원시 HTTP에 대해 다음과 같은 씬 추상화를 제공합니다.

- Provides access to a low-level HTTP interface and handles certificates for HTTPS access.
- Handles authentication with a host, remembers the session key, and appends the Authorization header to all requests.
- Manages namespaces.
- Constructs URLs from the endpoint fragments in the correct format for the underlying calls to the REST API. For example, the search/jobs endpoint fragment becomes <https://localhost:8089/servicesNS/-/-/search/jobs>.
- Supports custom HTTP request handlers.

- 저수준 HTTP 인터페이스에 대한 액세스를 제공하고 HTTPS 액세스에 대한 인증서를 처리합니다.
- 호스트와의 인증을 처리하고 세션 키를 기억하며 모든 요청에 Authorization 헤더를 추가합니다.
- 네임 스페이스를 관리합니다.
- REST API에 대한 기본 호출의 올바른 형식으로 엔드 포인트 단편에서 URL을 구성합니다. 예를 들어, 검색 / 작업 엔드포인트 조각은 <https://localhost:8089/servicesNS/-/-/search/jobs>가됩니다.
- 사용자 지정 HTTP 요청 처리기를 지원합니다.

The __binding__ module contains the following components:

__binding__ 모듈에는 다음 구성 요소가 포함되어 있습니다.

__Context class__ . This utility class contains the main functionality in the binding layer—the Context class remembers your login information and session key, constructs URLs, and makes HTTP requests.

__Context class__. 이 유틸리티 클래스는 바인딩 계층의 주요 기능을 포함합니다. Context 클래스는 로그인 정보와 세션 키를 기억하고 URL을 구성하며 HTTP 요청을 만듭니다.

__HTTPError exceptions__ . The Splunk SDK for Python exposes HTTP errors as HTTPError exceptions. Any response code that is greater or equal to 400 raises an HTTPError exception that includes the following information:

__HTTPError exceptions__. Python 용 Splunk SDK는 HTTP 오류를 HTTPError 예외로 표시합니다. 400보다 크거나 같은 응답 코드는 다음 정보가 포함된 HTTPError 예외를 발생시킵니다.

- Response code
- Error reason
- Returned headers
- Splunk-supplied error message
- Response body

__Custom HTTP request handlers__ . The Splunk SDK for Python provides a default HTTP request handler, based on the [httplib](https://docs.python.org/2/library/httplib.html) module. The default hander will make an HTTP request to a specified URL when provided with a dictionary that contains the HTTP method, URL, headers, and body. However, you can create and use your own HTTP request handler for features that aren't included in the default handler, such as:

__Custom HTTP request handlers__. Python 용 Splunk SDK는 [httplib] (<https://docs.python.org/2/library/httplib.html>) 모듈을 기반으로 기본 HTTP 요청 처리기를 제공합니다. 기본 핸들러는 HTTP 메소드, URL, 헤더 및 본문이 포함된 사전이 제공되면 지정된 URL에 대한 HTTP 요청을 작성합니다. 그러나 다음과 같이 기본 처리기에 포함되지 않은 기능에 대해 자체 HTTP 요청 처리기를 만들고 사용할 수 있습니다.

- Support for HTTP proxies.
- Server certificate validation.
- Event libraries, such as gevent or Eventlet.
- Additional logging or diagnostic output for debugging.

#### Client module

The client module (splunklib.client) provides an abstraction layer over the REST API, allowing you to access the endpoints in a stateless, Pythonic approach. The client layer sits on top of the binding layer, and uses its HTTP capabilities to access the REST API.

클라이언트 모듈 (splunklib.client)은 REST API를 통해 추상화 계층을 제공하므로 상태 비 저장 Pythonic 접근 방식으로 끝점에 액세스 할 수 있습니다. 클라이언트 계층은 바인딩 계층의 최상위에 위치하며 HTTP 기능을 사용하여 REST API에 액세스합니다.

Most importantly, this module contains the Service class, which is the primary entry point for the client library. Using the Service class you can access many parts of the Splunk REST API.

가장 중요한 점은 이 모듈에는 클라이언트 라이브러리의 기본 진입 점인 Service 클래스가 포함되어 있다는 점입니다. Service 클래스를 사용하면 Splunk REST API의 많은 부분에 액세스 할 수 있습니다.

Although you can use the binding module alone, using the client module has benefits:

바인딩 모듈 만 사용할 수도 있지만 클라이언트 모듈을 사용하면 다음과 같은 이점이 있습니다.

- Consistent access to Splunk resources. For example, whether you want to access apps or users, the code to list, add, remove, and update items is similar.
- Useful abstractions of the REST API. For example, you can easily use the receivers/simple endpoint just by getting the index you want to submit to:

- Splunk 리소스에 대한 일관된 액세스. 예를 들어 앱이나 사용자에 액세스 할 때 항목을 나열, 추가, 제거 및 업데이트하는 코드는 유사합니다.
- REST API의 유용한 추상화. 예를 들어 제출하려는 색인을 얻는 것만으로 수신기 / 단순 종점을 쉽게 사용할 수 있습니다.

```python
index = service.indexes["my_index"]
index.submit("some event", sourcetype="myevent")
The client module contains the following components:
```

- __Service class__ . This utility class inherits from splunklib.binding.Context, and provides functions to access various parts of the Splunk REST API (such as apps, jobs, users, inputs, and so on).
- __Endpoint class__ . This base class provides common functions that are relevant to all Splunk REST API endpoints. The Endpoint class builds the path, and provides methods to execute GET and POST HTTP requests against that path.
- __Entity class__ . This abstract base class is a subclass of Endpoint, and provides functions that implement the Splunk entity protocol such as the ability to read properties of the entity, update properties, read metadata, and so on.
- __Collection class__ . This class is a subclass of Endpoint, and provides functions that implement the Splunk collection protocol such as listing all entries in the collection, getting a specific item, creating new items, and so on.
- __Endpoint extensions__ . These include implementations and extensions that are specific to certain REST endpoints.

- __Service class__ . 이 유틸리티 클래스는 splunklib.binding.Context를 상속하며 Splunk REST API의 다양한 부분 (예 : 앱, 작업, 사용자, 입력 등)에 액세스하는 기능을 제공합니다.
- __Endpoint class__. 이 기본 클래스는 모든 Splunk REST API 끝점과 관련된 공통 함수를 제공합니다. Endpoint 클래스는 경로를 작성하고 해당 경로에 대해 GET 및 POST HTTP 요청을 실행하는 메소드를 제공합니다.
- __Entity class__ . 이 추상 기본 클래스는 Endpoint의 하위 클래스이며 엔터티의 속성을 읽거나 속성을 업데이트하거나 메타 데이터를 읽는 등의 기능을 Splunk 엔터티 프로토콜을 구현하는 기능을 제공합니다.
- __Collection class__ . 이 클래스는 Endpoint의 하위 클래스이며 컬렉션의 모든 항목 나열, 특정 항목 가져 오기, 새 항목 만들기 등과 같은 Splunk 컬렉션 프로토콜을 구현하는 함수를 제공합니다.
- __Endpoint extensions__ . 여기에는 특정 REST 엔드 포인트와 관련된 구현 및 확장이 포함됩니다.

#### Results module

The results module (splunklib.results) provides a Splunk-specific streaming XML reader. The results module abstracts over the details of the Splunk XML responses and provides a Pythonic way to access the stream of data.

결과 모듈 (splunklib.results)은 Splunk 특정 스트리밍 XML 판독기를 제공합니다. 결과 모듈은 Splunk XML 응답의 세부 사항을 추상화하고 데이터 스트림에 액세스하는 Pythonic 방식을 제공합니다.

The results module contains several classes, including the ResultsReader class, which can use with the search/export endpoint for streaming output as results become available.

결과 모듈에는 ResultsReader 클래스를 비롯한 여러 클래스가 포함되어 있습니다. 결과 클래스를 사용할 수있게되면 스트리밍 출력을 위해 검색 / 내보내기 끝점과 함께 사용할 수 있습니다.

#### Data module

The data module (splunklib.data) converts Splunk's Atom Feed response into a Pythonic structure (a dictionary or list), and provides a utility to navigate dictionaries using dot syntax.

데이터 모듈 (splunklib.data)은 Splunk의 Atom Feed 응답을 파이썬 구조 (사전 또는 목록)로 변환하고 도트 구문을 사용하여 사전을 탐색 할 수있는 유틸리티를 제공합니다.

### The Service class

The Service class is the primary entry point for the client library. Construct an instance of the Service class and provide any arguments that are required to connect to an available Splunk server. Once the Service instance is created, call the login method and provide login credentials. Once you have an authenticated Service instance, you can use it to navigate, enumerate, and operate on a wide variety of Splunk resources.

Service 클래스는 클라이언트 라이브러리의 기본 진입 점입니다. Service 클래스의 인스턴스를 생성하고 사용 가능한 Splunk 서버에 연결하는 데 필요한 인수를 제공합니다. Service 인스턴스가 생성되면 login 메소드를 호출하고 로그인 자격 증명을 제공하십시오. 인증 된 Service 인스턴스가 있으면이를 사용하여 다양한 Splunk 리소스를 탐색, 열거 및 작동 할 수 있습니다.

### Entities and collections

The Splunk REST API has over 160 endpoints (resources) that provide access to almost every feature of Splunk. The Splunk SDK for Python API exposes many of these resources as collections of entities, where an entity is a resource that has properties, actions, and descriptive metadata. Examples of entities are: jobs, indexes, apps, and configuration stanzas.

Splunk REST API는 거의 모든 Splunk 기능에 대한 액세스를 제공하는 160 종점 (자원)을 가지고 있습니다. Python API 용 Splunk SDK는 많은 리소스를 엔터티 컬렉션으로 제공합니다. 여기서 엔터티는 속성, 동작 및 설명 메타 데이터가있는 리소스입니다. 엔티티의 예로는 작업, 색인, 응용 프로그램 및 구성 스탠자가 있습니다.

This pattern provides a consistent approach to interacting with entities and collections of entities. Collections use a common mechanism to create and remove entities. Entities use a common mechanism to retrieve and update property values, and access entity metadata. Once you're familiar with this pattern, you'll have a reasonable understanding of how the SDK and underlying REST API work.

이 패턴은 엔터티 및 엔터티 컬렉션과의 일관된 접근 방식을 제공합니다. 컬렉션은 엔티티를 만들고 제거하는 공통 메커니즘을 사용합니다. 엔터티는 공통 메커니즘을 사용하여 속성 값을 검색 및 업데이트하고 엔터티 메타 데이터에 액세스합니다. 이 패턴에 익숙해지면 SDK와 기본 REST API가 어떻게 작동하는지 알 수 있습니다.

The Splunk SDK for Python contains the base classes Entity and Collection, both of which derive from the common base class Endpoint. Note that Service is not an Entity, but is a container that provides access to all features associated with a Splunk instance.

Python 용 Splunk SDK에는 공통 기본 클래스 Endpoint에서 파생 된 기본 클래스 인 Entity 및 Collection이 포함되어 있습니다. Service는 Entity가 아니라 Splunk 인스턴스와 관련된 모든 기능에 대한 액세스를 제공하는 컨테이너입니다.

The class hierarchy for the Splunk SDK for Python library is as follows:

Python 라이브러리 용 Splunk SDK의 클래스 계층 구조는 다음과 같습니다.

```properties
Service
Endpoint
    Entity
    Collection
```

### State caching

The client module enables state caching for Entity objects. When you instantiate an Entity object, a state record (a dictionary of key-value pairs) is read and copied from the server, creating a local snapshot of its properties. The state record dictionary contains the following keys:

클라이언트 모듈은 Entity 객체에 대한 상태 캐싱을 활성화합니다. Entity 객체를 인스턴스화하면 상태 레코드 (키 - 값 쌍 사전)가 읽고 서버에서 복사되어 해당 속성의 로컬 스냅 샷이 만들어집니다. 상태 레코드 사전에는 다음 키가 포함됩니다.

- title: The entity name, which is the title of the Splunk REST endpoint that this resource corresponds to (for example, alert or savedsearch).
- links: The URI of the resource, relative to the management port of a Splunk instance.
- access: A dictionary containing the permissions for accessing the resource. (For more about access control lists (ACLs), see Accessing Splunk resources).
- fields: A dictionary containing values that indicate which resource fields are wildcards, required, and optional.
- content: The full dictionary of properties and values of a resource, including access and fields.

- title :이 자원이 해당하는 Splunk REST 엔드 포인트의 제목 인 엔티티 이름입니다 (예 : alert 또는 savedsearch).
- links : 자원의 URI로, Splunk 인스턴스의 관리 포트를 기준으로합니다.
- 액세스 : 리소스에 액세스 할 수있는 권한이 포함된 사전입니다. ACL (액세스 제어 목록)에 대한 자세한 내용은 Splunk 리소스 액세스를 참조하십시오.
- 필드 : 와일드 카드, 필수 및 선택 사항 인 자원 필드를 나타내는 값이 포함된 사전입니다.
- content : 액세스 및 필드를 포함하여 자원의 속성 및 값의 전체 사전입니다.

Calls to the Entity object return values from the local cache rather than from the server. The cached state record in the Entity object can be accessed using a variety of properties, including:

Entity 개체를 호출하면 서버가 아닌 로컬 캐시에서 값을 반환합니다. Entity 개체의 캐시 된 상태 레코드는 다음과 같은 다양한 속성을 사용하여 액세스 할 수 있습니다.

- Entity.state returns a snapshot of the state record from the server.
- Entity.content returns the value of the content key of the local state record.
- Entity.access returns the value of the access key of the local state record.
- Entity.fields returns the value of the fields key of the local state record.

- Entity.state는 서버에서 상태 레코드의 스냅 샷을 반환합니다.
- Entity.content는 로컬 상태 레코드의 내용 키 값을 반환합니다.
- Entity.access는 로컬 상태 레코드의 액세스 키 값을 반환합니다.
- Entity.fields는 로컬 상태 레코드의 fields 키 값을 반환합니다.

This interface is designed to give you complete control of when round-trips are issued to the server, and to enable multiple updates to be made at a time. Use these methods to retrieve and update state record values on the local cache and server:

이 인터페이스는 왕복이 서버에 발행되는시기를 완벽하게 제어하고 한 번에 여러 개의 업데이트가 가능하도록 설계되었습니다. 로컬 캐시 및 서버에서 상태 레코드 값을 검색하고 업데이트하려면 다음 방법을 사용하십시오.

- Entity.refresh retrieves the current state record from the server and updates the local, cached copy. (If a local state record does not exist, the read method is called instead.)
- Entity.read returns the current state record from the server, but does not replace the values in the local, cached copy. Use this method to implement your own caching system.
- Entity.update updates the properties on the server with the values you provide, but this method does not update your local, cached copy (you must do that explicitly).

- Entity.refresh는 서버에서 현재 상태 레코드를 검색하고 캐시 된 로컬 복사본을 업데이트합니다. 로컬 상태 레코드가 없으면 대신 read 메서드가 호출됩니다.
- Entity.read는 서버에서 현재 상태 레코드를 반환하지만 캐시 된 로컬 복사본의 값을 대체하지 않습니다. 자신의 캐싱 시스템을 구현하려면이 방법을 사용하십시오.
- Entity.update는 사용자가 제공 한 값으로 서버의 등록 정보를 업데이트하지만이 방법은 로컬 캐시 된 복사본을 업데이트하지 않습니다 (명시 적으로 수행해야합니다).

> Note that refreshing the local state cache is always explicit and always requires a call to Entity.refresh. When you call Entity.update and then retrieve local values, you will still see the cached values because they have not been updated with new values from the server. To update the local copy, you must call refresh after update. For example :
> 로컬 상태 캐시 새로 고침은 항상 명시 적이며 항상 Entity.refresh를 호출해야합니다. Entity.update를 호출 한 다음 로컬 값을 검색 할 때 캐시 된 값은 서버의 새 값으로 업데이트되지 않았으므로 계속 표시됩니다. 로컬 복사본을 업데이트하려면 업데이트 후 새로 고침을 호출해야합니다. 예 :

```python
entity.update(attr=value).refresh()
```

### Namespaces

To account for permissions to view apps, system files, and other entity resources by users throughout a Splunk installation, Splunk provides access to entity resources based on a namespace. This is similar to the app/user context that is used by the Splunk REST API when accessing resources using endpoints.

Splunk 설치 전반에 걸쳐 사용자가 응용 프로그램, 시스템 파일 및 기타 엔티티 자원을 볼 수있는 권한을 설명하기 위해 Splunk는 네임 스페이스를 기반으로 엔티티 자원에 대한 액세스를 제공합니다. 이는 엔드 포인트를 사용하여 자원에 액세스 할 때 Splunk REST API에서 사용하는 app / user 컨텍스트와 유사합니다.

The namespace is defined by:

네임 스페이스는 다음에 의해 정의됩니다.

- An owner, which is the Splunk username, such as "admin". A value of "nobody" means no specific user. The "-" wildcard means all users.
- An app, which is the app context for this resource (such as "search"). The "-" wildcard means all apps.
- A sharing mode, which indicates how the resource is shared. The sharing mode can be :
  "user": The resource is private to a specific user, as specified by owner.
  "app": The resource is shared through an app, as specified by app. The owner is "nobody", meaning no specific user.
  "global": The resource is globally shared to all apps. The owner is "nobody", meaning no specific user.
  "system": The resource is a system resource (owner is "nobody", app is "system").

- owner, "admin"과 같은 Splunk 사용자 이름입니다. "nobody"값은 특정 사용자를 의미하지 않습니다. "-"와일드 카드는 모든 사용자를 의미합니다.
- 이 리소스의 앱 컨텍스트 인 앱입니다 (예 : '검색'). "-"와일드 카드는 모든 앱을 의미합니다.
- 자원 공유을 나타내는 공유 모드. 공유 모드는 다음과 같습니다.
  "user": 소유자가 지정한대로 특정 사용자의 비공개 리소스입니다.
  "app": 리소스는 앱에서 지정한대로 앱을 통해 공유됩니다. 소유자는 특정 사용자가 없음을 의미하는 "nobody"입니다.
  "global": 리소스는 모든 앱에 전체적으로 공유됩니다. 소유자는 특정 사용자가 없음을 의미하는 "아무도 없음"입니다.
  "system": 리소스는 시스템 리소스입니다 (소유자는 "nobody"이고 app은 "system"입니다).

In general, when you specify a namespace you can specify any combination of owner, app, and sharing the SDK library will reconcile the values, overriding them as appropriate. If a namespace is not explicitly specified, the current user is used for owner and the default app is used for app.

일반적으로 네임 스페이스를 지정할 때 소유자, 응용 프로그램 및 공유의 모든 조합을 지정할 수 있으므로 SDK 라이브러리가 적절하게 값을 재정 의하여 값을 조정합니다. 네임 스페이스를 명시 적으로 지정하지 않으면 현재 사용자가 소유자 용으로 사용되고 기본 앱이 앱에 사용됩니다.

Here are some example combinations of owner, app, sharing:

다음은 소유자, 앱, 공유의 몇 가지 조합입니다.

- List all of the saved searches for a specific user named Kramer: kramer, -, user
- Create an index to be used within the Search app: nobody, search, app

- Kramer라는 특정 사용자에 대해 저장된 검색 결과를 모두 나열하십시오 : kramer, -, user
- 검색 앱에서 사용할 색인 생성 : nobody, search, app

## Python 2.7 End-of-Life Statement

Splunk is preparing for the Python 2.7 end of life on January 1, 2020. We know how much the Splunk platform relies on Python 2.7, as do other Splunk products, third-party applications, and integrations. We are exploring multiple solutions to adapt to this change in a way that is architecturally sound and secure.

Splunk은 2020 년 1 월 1 일에 Python 2.7의 단종을 준비 중입니다. 다른 Splunk 제품, 타사 응용 프로그램 및 통합과 마찬가지로 Splunk 플랫폼이 Python 2.7에 의존하는지 알 수 있습니다. 우리는 구조적으로 건전하고 안전한 방식으로 이러한 변화에 적응할 수있는 여러 솔루션을 모색하고 있습니다.

Splunk is committed to:

Splunk는 다음을 위해 최선을 다하고 있습니다.

- Ensuring the stability of Splunk software installations and the Splunk platform.
- Ensuring the security of Splunk software installations and the Splunk platform.
- Providing sufficient, but not unlimited, time for our customers and developer community to transition.

- Splunk 소프트웨어 설치 및 Splunk 플랫폼의 안정성 보장.
- Splunk 소프트웨어 설치 및 Splunk 플랫폼의 보안 보장.
- 고객 및 개발자 커뮤니티가 전환 할 수있는 충분한 시간을 제공하지만 무제한은 아닙니다.

We are looking for interested Splunk software users and developers to help us exceed your expectations by:

우리는 Splunk 소프트웨어 사용자와 개발자가 다음과 같은 방법으로 귀하의 기대를 뛰어 넘는 데 도움이 될만한 제품을 찾고 있습니다.

- Helping Splunk understand concerns and the implications of this change on your business.
- Being part of a customer feedback group for this change.
- Testing and providing feedback on potential solutions.

- Splunk가 귀하의 비즈니스에 대한 이러한 변화의 시사점과 의미를 이해하도록 도와줍니다.
- 이러한 변화에 대한 고객 피드백 그룹에 속합니다.
- 잠재적 인 솔루션에 대한 피드백을 테스트하고 제공합니다.

Anyone interested in working with us to shape this next chapter for the Splunk platform should email python27-eol@splunk.com.

Splunk 플랫폼에 대한이 다음 장을 만들기 위해 우리와 함께 일하는 데 관심이있는 사람은 python27-eol@splunk.com으로 전자 메일을 보내야합니다.