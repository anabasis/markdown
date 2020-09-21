# How to connect to Splunk Enterprise using Splunk SDK for Python

To start a Splunk Enterprise session, the first thing your program must do is connect to Splunk Enterprise by sending login credentials to the splunkd server. Splunk returns an authentication token, which is then automatically included in subsequent calls for the rest of your session. By default, the token is valid for one hour, but is refreshed every time you make a call to splunkd.

> Tip: To help prevent coding errors, Splunk recommends the following best practices around indentation and white space when coding with the Splunk SDK for Python:
> - Use spaces to indent rather than pressing the Tab key. Most editors can be configured to insert spaces automatically when you press the Tab key instead of inserting an ASCII Tab character. If you can't configure your editor in this way, just use spaces instead of pressing Tab.
> - Use four spaces per indentation level. When setting your editor to use spaces rather than Tab characters, you should also be able to set the Tab width to four spaces.

The basic steps to connect to Splunk Enterprise are as follows:

1. Import the splunklib.client module. This module contains the Service class, which is the primary entry point to the Splunk client library and provides access to most of Splunk's resources.
2. Create a Service instance by using the connect function, which logs in and returns the new object.

Here's the example code for connecting to Splunk Enterprise. The credentials for logging in to the splunkd server are hard-coded, so replace them with your own. This example also prints your locally-installed Splunk Enterprise apps to the console to verify you connected successfully.

Before you run this example, be sure to start the Splunk Enterprise server if it isn't running already.
import splunklib.client as client

```properties
HOST = "localhost"
PORT = 8089
USERNAME = "admin"
PASSWORD = "yourpassword"

# Create a Service instance and log in
service = client.connect(
    host=HOST,
    port=PORT,
    username=USERNAME,
    password=PASSWORD)

# Print installed apps to the console to verify login
for app in service.apps:
    print app.name
```