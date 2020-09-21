# Troubleshooting the Splunk SDK for Python

This topic describes how to troubleshoot problems when coding with the Splunk® SDK for Python. It contains the following sections:

- Troubleshooting the .splunkrc file
- Can't programmatically modify access control list (ACL) properties of objects
- Can't use the SDK with an app written in Python 3
- A note about indentation and white space

If you still have questions after reading this topic, see the Questions? sidebar on the right side of this page for additional help.
  
## Troubleshooting the .splunkrc file

You can store your host, port, and login credentials in the .splunkrc file for convenience during development. For more information, see "[Utilities](http://dev.splunk.com/view/python-sdk/SP-CAAAEFC)".

If, after configuring the .splunkrc file, you get this type of error:

```txt
HTTP 401 Unauthorized -- Login failed
```

It probably means the `.splunkrc` file is not set up correctly.

1. Verify you set up the `.splunkrc` file correctly.
2. Be sure to use the value for the admin port (the default is 8089).

If you get this type of error:

```txt
error: no such option: --version
```

You might be using a preview or beta version of the SDK. The format of the `.splunkrc` file has changed between releases and some of the newer fields might not be recognized. You can either update to the latest version of the SDK, or comment out the `app`, `owner`, and `version` fields.
  
## Can't programmatically modify access control list (ACL) properties of objects

The Splunk SDK for Python does not yet provide bindings for modifying the ACLs of objects. To access and modify ACL properties of objects, you can issue raw REST calls using the get and post methods of the SDK's [Service](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Service) object. The Service object can wrap REST API endpoints into a Pythonic interface. For instance, the Service object's post method enables you to perform an HTTP POST request method with a dict of key-value pairs that correspond to ACL properties and values. For more information about managing ACL properties with the Splunk REST API, see [Access control lists for Splunk objects](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTresources#Access_control_lists_for_Splunk_objects).
  
## Can't use the SDK with an app written in Python 3

The Splunk SDK for Python requires Python 2.6.x or later. Python 2.7.x or later is recommended. However, at this time, Python 3.x is not supported.
  
## A note about indentation and white space

Splunk recommends the following best practices around indentation and white space when coding with the Splunk SDK for Python:

- `Use spaces to indent rather than pressing the Tab key` . When you indent code, such as after you've made a statement, use spaces rather than pressing the Tab key. Depending on the editor you use to code, you might be able to set your editor to automatically convert a Tab to spaces.
- `Use four spaces per indentation level` . Use four spaces to indent code. When setting your editor to use spaces rather than Tabs, you might also be able to set the Tab width to four spaces.

For more information about recommended style practices, see "[Code lay-out](http://www.python.org/dev/peps/pep-0008/#code-lay-out)" in the [Style Guide for Python](http://www.python.org/dev/peps/pep-0008/) Code.