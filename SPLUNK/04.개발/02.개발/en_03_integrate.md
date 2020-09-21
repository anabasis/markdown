# Integrate with the Splunk platform

- Splunk REST API
- Splunk Software Development Kits (SDKs)
- Developer tools

## Splunk REST API

Splunk provides a fully-documented and supported REST API with over 200 endpoints. Developers can programmatically index, search, and visualize data in Splunk from any application.

> Learn more about the [Splunk REST API](http://dev.splunk.com/restapi)

### The Splunk REST API

An Application Programming Interface (API) defines interfaces to a programming library or framework for accessing functionality provided by the library or framework. The Splunk Enterprise REST API provides methods for accessing every feature in our product. Your program talks to Splunk Enterprise using HTTP or HTTPS, the same protocols that your web browser uses to interact with web pages, and follows the principles of [Representational State Transfer](http://en.wikipedia.org/wiki/Representational_state_transfer) (REST).

To learn how to use the Splunk Enterprise REST API, read the following manuals:

- [REST API Reference Manual](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTprolog)
- [REST API User Manual](http://docs.splunk.com/Documentation/Splunk/latest/RESTUM/RESTusing)
- [REST API Tutorials Manual](http://docs.splunk.com/Documentation/Splunk/latest/RESTTUT/RESTconfigurations)

## Splunk Software Development Kits (SDKs)

The Splunk SDKs are built as a layer on top of the Splunk REST API. These SDKs include documentation, code samples, resources, and tools to make it faster and more efficient to program against the Splunk REST API using constructs and syntax familiar to developers experienced with Java, Python, JavaScript, and C#. In just a few lines of code, developers can easily manage HTTP access, authentication, and namespaces.

### Developers can use the Splunk SDKs to

- Run real-time searches and retrieve Splunk data from line-of-business systems such as Customer Service applications.
- Integrate data and visualizations (charts, tables) from Splunk into BI tools and reporting dashboards.
- Build mobile applications with real-time KPI dashboards and alerts powered by Splunk.
- Log directly to Splunk from remote devices and applications via TCP, UDP, and HTTP.
- Build customer-facing dashboards in applications powered by user-specific data in Splunk.
- Manage a Splunk instance, including adding and removing users as well as creating data inputs from an application outside of Splunk.
- Programmatically extract data from Splunk for long-term data warehousing.

> Learn more about the [Splunk SDKs](http://dev.splunk.com/sdks)

### Overview of the Splunk SDKs

The Splunk® SDKs are written on top of the Splunk REST APIs. The intent is to give you broad coverage of the REST API in a language-specific fashion to ease your access to the Splunk engine.

Currently, Splunk has SDKs for these languages:

- [Python](http://dev.splunk.com/python)
- [Java](http://dev.splunk.com/java)
- [JavaScript](http://dev.splunk.com/javascript)
- [C#](http://dev.splunk.com/csharp)

#### What you can do

Here are a few things that our customers are doing. We want to hear what you are doing or want to do. Ping us at Splunk Dev Info and tell us all about it.

- Integrate with third-party reporting tools and portals
- Log directly to Splunk
- Integrate Splunk search results into your application
- Extract data for archiving
- Build a UI on the web stack of your choice
- ...and so much more

#### REST API coverage by Splunk SDKs

The most basic way to programmatically access Splunk's resources is by using the REpresentational State Transfer (REST) model to make HTTP requests. Splunk provides a REST API that lets you interact with a Splunk instance and do most everything that you can using Splunk Web—including authentication, creating and running searches, managing search jobs, creating and managing indexes and inputs, and configuring Splunk. All of these things can be done with HTTP GET, POST, and DELETE operations using Splunk's REST API.

However, we want to make it even easier for you to develop Splunk applications using common programming languages, so we've created software development kits (SDKs) to help out. We've got Splunk SDKs for Python, Java, and JavaScript. But first, here's some background on how the SDKs relate to the REST API.

#### A little about the Splunk REST API

Each of Splunk's resources (apps, users, searches, jobs, indexes, inputs, and others) has a corresponding REST endpoint that indicates the resource's category (for example, the operation for streaming search results is GET search/jobs/export). To use the REST API to interact with Splunk's resources, you send a request to the management port of a Splunk server (which is port 8089 by default). The request requires admin access and is over HTTPS, using the URI of the REST endpoint. You can use any web browser, command-line tool, REST client, scripting language, or programming language that supports making HTTP calls. Curl and Wget are common tools. By default, responses are returned in Atom Syndication Format (an Atom Feed) with entries containing information about the Splunk resource.

The URI for the request includes the location of the Splunk server splunkd, the user/app context, and a REST endpoint that corresponds to the resource category. You also need to provide login credentials for Splunk and any additional parameters for the request.

The Atom Feed response contains the following containers:

- \<feed\> is a top-level element containing meta data that pertains to the entire response, such as the time of the request or the total number of results.
- \<entry\> corresponds to each individual result, containing meta data that pertains to one result and its content.
- \<content\> includes the key-value pairs that make up each result.

So what does that look like in practice? Here's an example of an HTTP POST request that creates a new index with a given name ("myindexname"):

- The username:password credentials are admin:pass.
- The location of splunkd is <https://localhost:8089/servicesNS>.
- The user/app context is admin/search.
- The endpoint is data/indexes.

You put it all together at the command line with curl and you get this:

```bash
curl -k -u admin:pass https://localhost:8089/servicesNS/admin/search/data/indexes \
     -d name=myindexname
```

To see what the Atom Feed response looks like, see the example under the POST data/indexes endpoint.

This is just a taste of the Splunk REST API. The endpoints are fully documented in the REST API Reference, along with information about how to use them.

#### What the Splunk SDKs do for you

Although you can use the REST API directly, you can also use the Splunk SDKs to interact with Splunk. Essentially, these SDKs are wrappers around the REST API that do a lot of the work for you, such as:

- Handling HTTP access. The SDKs provide HTTP access and handle the certificates for HTTPS.
- Authenticating. When you log in using a username and password, Splunk returns a session key. The SDKs automatically remember and append this session key to subsequent requests.
- Managing namespaces. A namespace is the user/app context for accessing a resource, which is specified by a Splunk username, a Splunk app (such as the default Search app), and a sharing mode. The SDKs send requests based on the namespace that was used for logging in, or you can specify a namespace to access a specific resource. For example, you can list all apps, or only the apps that a specific user has access to.
- Simplifying access to REST endpoints. The SDKs provide access to the REST API in the native style of different programming languages. For example, here's how to set a Splunk app's description using the REST API:

```bash
curl -k -u admin:yourpassword https://localhost:8089/services/apps/local/myApp \
    -d description="My Killer App"
```

Here's how you'd do this with a Java setter method:

```bash
app.setDescription("My Killer App");
````

To show you how the other languages are used, here\'s a Python example that submits an event to an index:

```bash
index = service.indexes["my_index"]
index.submit("some event", source="www", sourcetype="web_event")
```

For comparison, here's the same example using curl to access the REST API:

```bash
curl -k -u admin:yourpassword "https://localhost:8089/services/receivers/simple?source=www&sourcetype=web_event&index=my_index" \
    -d "some event"
```

- `Building the correct URL for an endpoint`. The SDKs build out the complete REST URLs in the correct format, with the namespace and any additional parameters you specify.
- `Displaying simplified output for searches`. The REST API returns search results (events) in XML, JSON, or CSV—but in a raw format. The SDKs provide results readers (helper classes for Python and Java, a code example for JavaScript) that parse these events and return them in a simplified structure with clear key-value pairs.

#### Do you still need to know the REST API

While the Splunk SDKs provide a layer over the Splunk REST API, you still should become familiar with the REST endpoints and understand how to navigate them. The abstraction layer can be thin depending on which SDK language you are using, and not every feature has a corresponding class or method in each SDK.

In general, you interact with the REST API by getting and setting the parameters that are available for each endpoint. Similarly, the SDKs use key-value pairs to set parameters when there isn't a specific SDK API to do the job. That's when being familiar with the REST API helps—in these cases when you want to get or set parameters that are not defined by the SDK, you need to know which parameters are allowed for that resource, being careful to specify the case-sensitive name, and provide a value in the correct format.

## Developer tools

Splunk empowers developers to optimize their productivity and write great code using any technology platform, language, framework or approach.

Splunk offers tooling support for:

- Visual Studio: .NET developers can use the [Splunk extension for Visual Studio](http://dev.splunk.com/view/splunk-extension-vs/SP-CAAAEXT) to build applications that use and extend Splunk. The extension simplifies creating projects using the Splunk SDK for C#. Additionally, the Splunk extension for Visual Studio includes support for creating modular inputs to extend Splunk and allow it to talk to additional data sources such as other internal systems or public APIs like Google, Facebook and Twitter.
- Eclipse IDE: The [Splunk plug-in for Eclipse](http://dev.splunk.com/view/splunk-plugin-eclipse/SP-CAAAEQP) contains a project template for building a new Splunk SDK for Java application. This is ideal for building an application that searches against Splunk data or does automation. The project template includes snippets for performing common SDK tasks, as well as infrastructure for wiring up the application to log application data directly to Splunk utilizing popular log frameworks like Apache log4j.

Splunk also provides logging libraries that allow you to log activity from your .NET, Java, or JavaScript application directly back to Splunk Enterprise:

- [Splunk logging for .NET](http://dev.splunk.com/view/splunk-loglib-dotnet/SP-CAAAEX4)
- [Splunk logging for Java](http://dev.splunk.com/view/splunk-logging-java/SP-CAAAE2K)
- [Splunk logging for JavaScript](http://dev.splunk.com/view/splunk-logging-javascript/SP-CAAAE6U)

The Splunk SDK for Java has built-in support for IntelliJ for easy integration. SpringSource introduced the [Spring Integration Extension for Splunk](https://github.com/SpringSource/spring-integration-extensions/tree/master/spring-integration-splunk), making it easier for developers using the popular Java framework Spring to log directly to Splunk from their applications.