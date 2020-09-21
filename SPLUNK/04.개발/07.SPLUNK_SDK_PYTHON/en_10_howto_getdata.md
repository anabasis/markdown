# How to get data into Splunk using the Splunk SDK for Python

Getting data into Splunk® involves taking data from inputs, and then indexing that data by transforming it into individual events that contain searchable fields. Here's a brief overview of how it all works.

## Data inputs

A data input is a source of incoming event data. Splunk can index data from the following types of inputs:

- _Files and directories_ — the contents of files and directories of files. You can upload a file for one-time indexing (a oneshot input), monitor for new data, or monitor for file system changes (events are generated when the directory undergoes a change). Files and directories can be included using whitelists, and excluded using blacklists.
- _Network events_ —data that is received over network Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) ports, such as data that is sent from a Splunk forwarder from a remote computer. TCP inputs are separated into raw (unprocessed) and cooked (processed) inputs, with SSL as an option for either type.
- _Windows data_ —data from Windows computers, which includes:
  - Windows event log data
  - Windows Registry data
  - Windows Management Instrumentation (WMI) data
  - Active Directory data
  - Performance monitoring (perfmon) data
- _Other data sources_ —data from custom apps, FIFO queues, scripts that get data from APIs, and other remote data interfaces and message queues.

Your data inputs and their configurations are saved in the [inputs.conf](http://docs.splunk.com/Documentation/Splunk/latest/admin/Inputsconf) configuration file.

## Indexes

The index stores compressed, raw event data. When receiving data from your inputs, Splunk parses the data into events and then indexes them, as follows:

- During parsing, Splunk extracts default fields, configures character-set encoding, identifies line termination, identifies timestamps (creating them if they aren't there), masks sensitive or private data, and can apply custom metadata. Parsing can be done by heavy forwarders. Universal forwarders do minimal parsing.
- During indexing, Splunk breaks events into segments, builds the index data structures, and writes the raw data and index files to disk.

Splunk can usually determine the data type and handle the data accordingly. But when setting up new inputs, you might consider sending data to a test index first to make sure everything is configured the way you want. You can delete the indexed data (clean the index) and start over as needed. Event processing rules are set in the props.conf configuration file, which you'll need to modify directly if you want to reconfigure how events are processed.

Each index is stored as a collection of database directories (also known as buckets) in the file system, located in `$SPLUNK_HOME/var/lib/splunk`. Buckets are organized by age:

- _Hot_ buckets are searchable, actively being written to, one per index. Hot buckets roll to warm at a certain size or when splunkd is restarted, then a new hot bucket is created.
- _Warm_ buckets are searchable. Oldest warm buckets roll to cold when the number of warm buckets reaches a number limit.
- _Cold_ buckets are searchable. After a set period of time, cold buckets roll to frozen.
- _Frozen_ buckets are not searchable. These buckets are archived or deleted.

You can configure aspects such as the path configuration for your buckets. For example, keep the hot and warm buckets on a local computer for quick access, and put the cold and frozen buckets on a separate disks for long-term storage. You can also set the storage size.

By default, data is stored in the main index, but you can add more indexes for different data inputs. You might want multiple indexes to:

- _Control user access_ . Users can search only in indexes they are allowed to by their assigned role.
- _Accommodate varying retention policies_ . Set a different archive or retention policy by index.
- _Speed searches in certain situations_ . Create dedicated indexes for each data source, search just in the index you want.

## The input and index APIs

The classes for working with getting data into Splunk are:

- The [splunklib.client.Inputs](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Inputs) class for the collection of inputs.
- The [splunklib.client.Input](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Input) class for an individual input.
- The [splunklib.client.Indexes](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Indexes) class for the collection of indexes.
- The [splunklib.client.Index](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Index) class for an individual index.

Access these classes through an instance of the [splunklib.client.Service](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Service) class. Retrieve a collection, and from there you can access individual items in the collection and create new ones. For example, here's a simplified program for getting a collection of inputs and creating a new one:

```python
# Connect to Splunk
service = client.connect(...)

# Get the collection of inputs
myinputs = service.inputs

# Create an input
mynewinput = myinputs.create(<i>name</i>, <i>inputkind</i>, <i>optional_arguments</i>)
```

## Code examples

This section provides examples of how to use the index and input APIs, assuming you first connect to a Splunk instance:

- To list data inputs
- To create a new data input
- To view and modify the properties of a data input
- To list indexes
- To create a new index
- To add data directly to an index
- To view and modify the properties of an index
- To clean events from an index

Here are the available parameters for inputs and indexes:

- Input kinds
- Collection parameters
- Input parameters
- Index parameters

### To list data inputs

This example shows how to use the [splunklib.client.Inputs](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Inputs) class to retrieve the collection of data inputs that have been configured for Splunk and list them. For a list of available parameters to use when retrieving a collection, see [Collection parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE6#getcollparams).
> Note  To be able to list inputs, the user's role must include those capabilites. For a list of available capabilities, see Capabilities.

```python
# Get the collection of data inputs
inputs = service.inputs

# List the inputs and kind
for item in inputs:
    print "%s (%s)" % (item.name, item.kind)
```

### To create a new data input

This example shows how to create a data input. You'll need to provide an input kind and a name. The values for these depend on the kind of input you are creating:

<table>
<tr><td>Input</td><td>kind parameter</td><td>name parameter</td></tr>
<tr><td>Active Directory</td><td>"ad"</td><td>The name of the configuration for a specific domain controller.</td></tr>
<tr><td>Monitor</td><td>"monitor"</td><td>The file or directory path to monitor.</td></tr>
<tr><td>Script</td><td>"script"</td><td>The name of the script.</td></tr>
<tr><td>TCP cooked</td><td>"splunktcp"</td><td>The port number of the input.</td></tr>
<tr><td>TCP raw</td><td>"tcp"</td><td>The port number of the input.</td></tr>
<tr><td>UDP</td><td>"udp"</td><td>The port number of the input.</td></tr>
<tr><td>Windows event log</td><td>"win-event-log-collections"</td><td>The name of the collection.</td></tr>
<tr><td>Windows Perfmon</td><td>"win-perfmon"</td><td>The name of the collection.</td></tr>
<tr><td>Windows Registry</td><td>"registry"</td><td>The name of the configuration stanza.</td></tr>
<tr><td>WMI</td><td>"win-wmi-collections"</td><td>The name of the collection.</td></tr>
</table>

> Note: Oneshot inputs are created differently (see [To add data directly to an index](http://dev.splunk.com/view/python-sdk/SP-CAAAEE6#add2index)).
This example shows how to create a tcp data input.
> Note: To be able to create and modify inputs, the user's role must include those capabilites. For a list of available capabilities, see [Capabilities](http://dev.splunk.com/view/python-sdk/SP-CAAAEJ6#capabilities).

```python
# Get the collection of data inputs
inputs = service.inputs

# Create a new TCP data input
tcpinput = inputs.create("9999", "tcp", host="sdk-test")
print "New TCP input:", tcpinput.name
```

### To view and modify the properties of a data input

This example continues from the previous example―it displays the properties of the new tcp input using the [splunklib.client.Input](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Input) class then modifies a few properties. To make these changes to the server, call the [update](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Entity.update) method, then call the [refresh](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Entity.refresh) method to update your local object (allow time for the update between server and client to show up). For more about the properties you can set for different types of data inputs, see [Input parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE6#inputparams).

> Note: To be able to create and modify inputs, the user's role must include those capabilites. For a list of available capabilities, see Capabilities.

```python
# Continue the previous example with the new tcpinput

# Retrieve the new TCP data input
tcpinput = service.inputs["9999"]

# Print the input's current properties
print tcpinput.name
print "connection_host:", tcpinput["connection_host"]
print "disabled:", tcpinput["disabled"]
print "group:", tcpinput["group"]
print "host:", tcpinput["host"]
print "index:", tcpinput["index"]

# Modify a property
kwargs = {"host": "sdk-test-mod"}

# Update changes on the server
tcpinput.update(**kwargs).refresh()

# Display the new property and value
print "\nhost:", tcpinput["host"]
```

### To list indexes

This example shows how to use the [splunklib.client.Indexes](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Indexes) class to retrieve and list the indexes that have been configured for Splunk, along with the number of events contained in each. For a list of available parameters to use when retrieving a collection, see [Collection parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE6#getcollparams).

```python
# Get the collection of indexes
indexes = service.indexes

# List the indexes and their event counts
for index in indexes:
    count = index["totalEventCount"]
    print "%s (events: %s)" % (index.name, count)
```

### To create a new index

When you create an index, all you need to provide is a name. You can also specify additional properties at the same time by providing a dictionary of key-value pairs for the properties (the possible parameters are summarized in [Index parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE6#indexparams)). Or, modify properties after you have created the index.

> Note: To be able to create and modify an index, the user's role must include those capabilites. For a list of available capabilities, see [Capabilities](http://dev.splunk.com/view/python-sdk/SP-CAAAEJ6#capabilities).

This example shows how to create a new index.

> Note: If you are using a version of Splunk earlier than 5.0, you can't delete indexes using the SDK or the REST API—something to be aware of before creating lots of test indexes.

```python
# Create a new index
mynewindex = service.indexes.create("test_index")
```

### To add data directly to an index

There are different ways to add data directly to an index, without configuring a data input. First, retrieve an index using the [splunklib.client.Index](http://dev.splunk.com/view/python-sdk/SP-CAAAEJ6#capabilities) class, and then use one of the following methods:

- Use the [upload](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Index.upload) method to upload a single file as an event stream for one-time indexing, which corresponds to a oneshot data input. You'll need to specify the file and path to upload, too.
- Use the [submit](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Index.submit) method to send an event over HTTP. You'll need to provide the event as a string, and specify values to apply to the event (host, source, and sourcetype).
- Use the [attach](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Index.attach) method to send events over a writeable socket. You can also specify the values to apply to these events (host, source, and sourcetype).

Here is an example of uploading a single file:

```python
# Create a oneshot input

# Retrieve the index for the data
myindex = service.indexes["test_index"]

# Create a variable with the path and filename
uploadme = "/Applications/Splunk/README-splunk.txt"

# Upload and index the file
myindex.upload(uploadme);
```

Here is an example of submitting an event over HTTP:

```python
# Send an event over HTTP

# Retrieve the index for the data
myindex = service.indexes["test_index"]

# Submit an event over HTTP
myindex.submit("This is my HTTP event", sourcetype="access_combined.log", host="local")
```

Here is an example of sending event to an open socket:

```python
# Send an event over an open socket

# Retrieve the index for the data
myindex = service.indexes["test_index"]

# Open a socket
mysocket = myindex.attach()

# Send events to it
mysocket.send("This is my socket event\r\n")

# Close the socket
mysocket.close()
```

### To view and modify the properties of an index

This example shows how to view the properties of the index created in the previous example and modify its properties. For more about the available properties for indexes, see [Index parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEJ2#indexparams).
Note: To be able to create and modify an index, the user's role must include those capabilites. For a list of available capabilities, see [Capabilities](http://dev.splunk.com/view/python-sdk/SP-CAAAEJ6#capabilities).

```python
# Retrieve the new index and display some properties
mynewindex = service.indexes["test_index"]

print "coldPath:", mynewindex["coldPath"]
print "currentDBSizeMB:", mynewindex["currentDBSizeMB"]
print "frozenTimePeriodInSecs:", mynewindex["frozenTimePeriodInSecs"]
print "homePath:", mynewindex["homePath"]
print "maxDataSize:", mynewindex["maxDataSize"]
print "maxHotBuckets:", mynewindex["maxHotBuckets"]
print "maxMemMB:", mynewindex["maxMemMB"]
print "maxTime:", mynewindex["maxTime"]
print "maxTotalDataSizeMB:", mynewindex["maxTotalDataSizeMB"]
print "totalEventCount:", mynewindex["totalEventCount"]

# Modify a couple of properties
kwargs = {"maxHotBuckets": 4,
        "maxTotalDataSizeMB": 100000}

# Update the server and refresh locally
mynewindex.update(**kwargs).refresh()
```

### To clean events from an index

This example shows how to clean an index using the [Index.clean](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Index.clean) method, which removes the events from it. Note that this method can block up to the timeout period.

```python
# Retrieve the index
myindex = service.indexes["test_index"]

# Display the current size
print "Current DB size:", myindex["currentDBSizeMB"], "MB"

# Clean all events from the index and display its size again
timeout = 60
myindex.clean(timeout)
print "Current DB size:", myindex["currentDBSizeMB"], "MB"
```

### Input kinds

The Splunk SDK for Python takes a kind parameter to several methods to specify the type of input, for example when you create a data input. Here are the values for kind:

<table>
<tr><td>Input kind</td><td>Description</td></tr>
<tr><td>ad</td><td>Active Directory</td></tr>
<tr><td>monitor</td><td>Files and directories</td></tr>
<tr><td>registry</td><td>Windows Registry</td></tr>
<tr><td>script</td><td>Scripts</td></tr>
<tr><td>splunktcp</td><td>TCP, processed</td></tr>
<tr><td>tcp</td><td>TCP, unprocessed</td></tr>
<tr><td>udp</td><td>UDP</td></tr>
<tr><td>win-event-log-collections</td><td>Windows event log</td></tr>
<tr><td>win-perfmon</td><td>Performance monitoring</td></tr>
<tr><td>win-wmi-collections</td><td>WMI</td></tr>
</table>

### Collection parameters

By default, all entities are returned when you retrieve a collection. But by using the parameters below, you can also specify the number of entities to return and how to sort them. These parameters are available whenever you retrieve a collection:

<table>
<tr><td>Parameter</td><td>Description</td></tr>
<tr><td>count</td><td>A number that indicates the maximum number of entities to return.</td></tr>
<tr><td>offset</td><td>A number that specifies the index of the first entity to return.</td></tr>
<tr><td>search</td><td>A string that specifies a search expression to filter the response with, matching field values against the search expression. For example, "search=foo" matches any object that has "foo" as a substring in a field, and "search=field_name%3Dfield_value" restricts the match to a single field.</td></tr>
<tr><td>sort_dir</td><td>An enum value that specifies how to sort entities. Valid values are "asc" (ascending order) and "desc" (descending order).</td></tr>
<tr><td>sort_key</td><td>A string that specifies the field to sort by.</td></tr>
<tr><td>sort_mode</td><td>An enum value that specifies how to sort entities. Valid values are "auto", "alpha" (alphabetically), "alpha_case" (alphabetically, case sensitive), or "num" (numerically).</td></tr>
</table>

### Input parameters

The properties that are available for each type of data input corresponds to the parameters for the following REST API endpoints:

- **Active Directory** : [POST data/inputs/ad](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fad) endpoint
- **files and directories (monitor)** : [POST data/inputs/monitor](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fmonitor) endpoint
- **files and directories (oneshot)** : [POST data/inputs/oneshot](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Foneshot) endpoint
- **scripts** : [POST data/inputs/script](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fscript) endpoint
- **TCP, cooked** : [POST data/inputs/tcp/cooked](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Ftcp.2Fcooked) endpoint
- **TCP, raw** : [POST data/inputs/tcp/raw](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Ftcp.2Fraw) endpoint
- **TCP, SSL** : [POST data/inputs/tcp/ssl](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Ftcp.2Fssl) endpoint
- **UDP** : [POST data/inputs/udp](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fudp) endpoint
- **Windows event log** : [POST data/inputs/win-event-log-collections](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fwin-event-log-collections) endpoint
- **Windows performance monitor** : [POST data/inputs/win-perfmon](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fwin-perfmon) endpoint
- **Windows Registry** : [POST data/inputs/registry](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fregistry) endpoint
- **WMI** : [POST data/inputs/win-wmi-collections](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTinput#data.2Finputs.2Fwin-wmi-collections) endpoint

This table summarizes the available parameters for different types of inputs:

<table>
<tr><td>Parameter</td><td>Description</td><td>Input type</td></tr>
<tr><td>_rcvbuf</td><td>Read only. A number that specifies the size of the socket receive buffer, in bytes. This parameter is valid for UDP inputs, but is deprecated elsewhere.</td><td>UPD</td></tr>
<tr><td>_TCP_ROUTING</td><td>Read only. A string that contains a list of TCP forwarding groups, as specified in the outputs.conf configuration file.</td><td>monitor</td></tr>
<tr><td>baseline</td><td>A Boolean that specifies whether to establish a baseline value for the registry keys (1 means yes, 0 means no).</td><td>Windows Registry</td></tr>
<tr><td>blacklist</td><td>A string that specifies a regular expression (regex) for a file path. File paths that match this expression are not indexed.</td><td>monitor</td></tr>
<tr><td>Bytes Indexed</td><td>Read only. A number that indicates the total number of bytes that were read and sent to the pipeline for indexing from a oneshot input. This total includes the uncompressed byte count from a source file that is compressed on disk.</td><td>oneshot</td></tr>
<tr><td>check-index</td><td>A Boolean that indicates whether to check the "index" value to ensure that it is the name of a valid index.</td><td>monitor</td></tr>
<tr><td>check-path</td><td>A Boolean that indicates whether to check the "name" value to ensure that it exists.</td><td>monitor</td></tr>
<tr><td>cipherSuite</td><td>Read only. A string that contains a list of acceptable ciphers to use in SSL.</td><td>TCP ssl</td></tr>
<tr><td>classes</td><td>A string that contains a valid WMI class name.</td><td>WMI</td></tr>
<tr><td>connection_host</td><td>An enum that sets the host for the remote server that is sending data. Valid values are "ip" (uses the IP address), "dns" (uses the reverse DNS entry for the IP address), and "none" (uses the host as specified in the inputs.conf configuration file, which is typically the Splunk system hostname).</td><td>TCP cooked, TCP raw, UPD</td></tr>
<tr><td>connection</td><td>Read only. A string that contains the IP address and port of the source connecting to this input port.</td><td>TCP cooked, TCP raw</td></tr>
<tr><td>counters</td><td>A string that specifies a set of counters to monitor. An asterisk ("*") is equivalent to all counters.</td><td>Windows perfmon</td></tr>
<tr><td>crc-salt</td><td>A string that is used to force Splunk to index files that have a matching cyclic redundancy check (CRC).</td><td>monitor</td></tr>
<tr><td>disabled</td><td>A Boolean that indicates whether a given item (monitoring stanza, input, script, monitor input, or collection) has been disabled.</td><td>AD, monitor, script, TCP cooked, TCP raw, TCP SSL, Windows Registry, WMI</td></tr>
<tr><td>eai:attributes</td><td>Read only. A string that contains the metadata for this input.</td><td>monitor, oneshot, script, TCP cooked, TCP raw, UDP</td></tr>
<tr><td>endtime</td><td>Read only. A string that contains the time when the script stopped running.</td><td>script</td></tr>
<tr><td>fields</td><td>A string that specifies properties (fields) to gather from the given WMI class.</td><td>WMI</td></tr>
<tr><td>filecount</td><td>Read only. A number that indicates how many files are being monitored.</td><td>monitor</td></tr>
<tr><td>followTail</td><td>A Boolean that indicates whether files that are seen for the first time are read from the end.</td><td>monitor</td></tr>
<tr><td>group</td><td>Read only. A string that contains the OS group of commands. A value of "listenerports" is used for listening ports.</td><td>script, TCP cooked, TCP raw, UDP
<tr><td>hive</td><td>A string that specifies the registry hive under which to monitor for changes.</td><td>Windows Registry</td></tr>
<tr><td>host_regex</td><td>A string that specifies a regular expression (regex) to use for extracting a "host" field from the path. If the path matches this regular expression, the captured value is used to populate the "host" field for events from this data input. The regular expression must have one capture group.</td><td>monitor, oneshot</td></tr>
<tr><td>host_segment</td><td>A string that contains the specified slash-separate segment of the file path as the value of the "host" field.</td><td>monitor, oneshot</td></tr>
<tr><td>host</td><td>A string that specifies the host from which the indexer gets event data. This parameter corresponds to the "host" field.</td><td>oneshot, monitor, script, TCP cooked, TCP raw, UPD</td></tr>
<tr><td>hosts</td><td>A string that contains a comma-separated list of additional hosts to use for monitoring. The first host should be set with the "lookup_host" parameter, and any additional hosts should be set using this parameter.</td><td>Windows event log</td></tr>
<tr><td>ignore-older-than</td><td>A string that specifies a time value indicating the rolling time window. When the modification time of a file being monitored falls outside of this rolling time window, the file is no longer monitored.</td><td>monitor</td></tr>
<tr><td>index</td><td>A string that specifies the index that stores events from this input.</td><td>AD, monitor. oneshot, script, TCP raw, UPD, Windows event log, Windows perfmon, Windows Registry, WMI</td></tr>
<tr><td>instances</td><td>A string that contains a set of counter instances to monitor. An asterisk ("*") is equivalent to all instances.</td><td>Windows perfmon, WMI</td></tr>
<tr><td>interval</td><td>A string that contains the number of seconds or a cron schedule that specifies the interval at which to run a script, poll performance counters, or query WMI providers.</td><td>script, Windows perfmon, WMI</td></tr>
<tr><td>logs</td><td>A string that contains a comma-separated list of event log names from which to gather event data.</td><td>Windows event log</td></tr>
<tr><td>lookup_host</td><td>A string that specifies the host from which to gather event data.</td><td>Windows event log, WMI</td></tr>
<tr><td>monitorSubnode</td><td>A Boolean that indicates whether the Windows Registry input monitors all sub-nodes under a given hive.</td><td>Windows Registry</td></tr>
<tr><td>monitorSubtree</td><td>A Boolean that indicates whether to monitor the subtrees of a given directory tree path (1 means yes, 0 means no).</td><td>AD</td></tr>
<tr><td>name</td><td>A string that specifies the name of the input based on the type:
Active Directory: The name of the configuration for a specific domain controller.
Monitor: The file or directory path to monitor.
Oneshot: The path to the file to index.
Script: The name of the script.
TCP cooked: The port number of the input.
TCP raw: The port number of the input.
UDP: The port number of the input.
Windows event log: The name of the collection.
Windows Perfmon: The name of the collection.
Windows Registry: The name of the configuration stanza.
WMI: The name of the collection.</td><td>AD, monitor, oneshot, script, TCP cooked, TCP raw, TCP SSL, UPD, Windows event log, Windows perfmon, Windows Registry, WMI</td></tr>
<tr><td>no_appending_timestamp</td><td>A Boolean that indicates whether to prevent Splunk from prepending a timestamp and hostname to incoming events.</td><td>UPD</td></tr>
<tr><td>no_priority_stripping</td><td>A Boolean that indicates whether to prevent Splunk from removing the "priority" field from incoming syslog events.</td><td>UPD</td></tr>
<tr><td>object</td><td>A string that specifies a valid performance monitor object (for example, "Process", "Server", or "PhysicalDisk").</td><td>Windows perfmon</td></tr>
<tr><td>passAuth</td><td>A string that contains a username specifying the user to run the script under. Splunk generates an authorization token for the user and passes it to the script.</td><td>script</td></tr>
<tr><td>password</td><td>A string that contains the certificate password.</td><td>TCP SSL
<tr><td>proc</td><td>A string that specifies a regular expression (regex). Changes are only collected for process names that match this expression.</td><td>Windows Registry</td></tr>
<tr><td>queue</td><td>An enum that specifies where to deposit the events that are read by the input processor. Valid values are "parsingQueue" (apply values from the props.conf configuration file and other parsing rules to your data) and "indexQueue" (send your data directly into the index).</td><td>TCP raw, UPD</td></tr>
<tr><td>rawTcpDoneTimeout</td><td>A number that specifies, in seconds, the timeout value for adding a Done key. If a connection over the port specified by name remains idle after receiving data for the specified number of seconds, it adds a Done key, implying the last event has been completely received.</td><td>TCP raw</td></tr>
<tr><td>recursive</td><td>A Boolean that indicates whether to monitor any subdirectories within the data input.</td><td>monitor</td></tr>
<tr><td>rename-source</td><td>A string that specifies a name for the "source" field for events from this data input. The same source should not be used for multiple data inputs.</td><td>monitor, oneshot, script</td></tr>
<tr><td>requireClientCert</td><td>A Boolean that indicates whether a client is required to authenticate.</td><td>TCP SSL</td></tr>
<tr><td>restrictToHost</td><td>A string that specifies a host to which incoming connections on this port are restricted.</td><td>TCP cooked, TCP raw, UPD</td></tr>
<tr><td>rootCA</td><td>A string that specifies the path to the root certificate authority file.</td><td>TCP SSL</td></tr>
<tr><td>script</td><td>A string that specifies the path to the script to restart. This path must match an existing scripted input that has already been configured.</td><td>script</td></tr>
<tr><td>server</td><td>A string that specifies a comma-separated list of additional servers to gather data from. Use this parameter when you need to gather data from more than one server.</td><td>WMI</td></tr>
<tr><td>serverCert</td><td>A string that specifies a full path to the server certificate.</td><td>TCP SSL</td></tr>
<tr><td>servername</td><td>Read only. A string that specifies the server name of the source connecting to this port.</td><td>TCP cooked, TCP raw</td></tr>
<tr><td>Size</td><td>Read only. A number that specifies the size of the source file, in bytes.</td><td>oneshot</td></tr>
<tr><td>source</td><td>A string that specifies the source for this input, which corresponds to the "source" field. The same source should not be used for multiple data inputs.</td><td>script, TCP raw, UPD, Windows perfmon</td></tr>
<tr><td>Sources Indexed</td><td>Read only. Indicates the number of sources read from a file in a compressed format, such as TAR or ZIP. A value of 0 means the source file was not compressed.</td><td>oneshot</td></tr>
<tr><td>sourcetype</td><td>A string that specifies the source type for events from this input, which corresponds to the "sourcetype" field. The source type of an event is the format of the data input from which it originates, such as access_combined or cisco_syslog. The source type also determines how Splunk formats your data.</td><td>monitor, oneshot, script, TCP raw, UPD, Windows perfmon</td></tr>
<tr><td>Spool Time</td><td>Read only. A string that specifies the time the request was made to read the source file.</td><td>oneshot</td></tr>
<tr><td>SSL</td><td>A Boolean that indicates whether SSL is configured.</td><td>TCP cooked, TCP raw</td></tr>
<tr><td>startingNode</td><td>A string that specifies where in the Active Directory tree to start monitoring. If a value is not specified, Splunk attempts to start monitoring at the root of the directory tree.</td><td>AD</td></tr>
<tr><td>starttime</td><td>Read only. A string that specifies the time when the script was run.</td><td>script</td></tr>
<tr><td>targetDc</td><td>A string that specifies a fully-qualified domain name of a valid, network-accessible domain controller. If a value is not specified, Splunk will obtain the domain controller from the local computer.</td><td>AD</td></tr>
<tr><td>time-before-close</td><td>A number that specifies the minimum number of seconds to keep a file open when Splunk reaches the end of a file that is being read. After this period has elapsed, the file is checked again for more data.</td><td>monitor</td></tr>
<tr><td>type</td><td>A string that specifies a regular expression (regex) for the types of registry events to monitor.</td><td>Windows Registry</td></tr>
<tr><td>whitelist</td><td>A string that specifies a regular expression (regex) for a file path. Only those file paths that match this expression are indexed.</td><td>monitor</td></tr>
</table>

### Index parameters

The parameters you can use for working with indexes correspond to the parameters for the [data/indexes](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTindex) endpoint in the REST API.
The following parameters are available for indexes:

<table>
<tr><td>Parameter</td><td>Description</td></tr>
<tr><td>assureUTF8</td><td>A Boolean that indicates whether all data retrieved from the index is in proper UTF8 encoding. When true, indexing performance is reduced. This setting is global, not per index.</td></tr>
<tr><td>blockSignatureDatabase</td><td>A string that specifies the name of the index that stores block signatures of events. This setting is global, not per index.</td></tr>
<tr><td>blockSignSize</td><td>A number that indicates how many events make up a block for block signatures. A value of 0 means block signing has been disabled for this index.</td></tr>
<tr><td>bloomfilterTotalSizeKB</td><td>A number that indicates the total size of all bloom filter files, in KB.</td></tr>
<tr><td>bucketRebuildMemoryHint</td><td>A string that contains a suggestion for the Splunk bucket rebuild process for the size of the time-series (tsidx) file to make.</td></tr>
<tr><td>coldPath</td><td>A string that contains the file path to the cold databases for the index.</td></tr>
<tr><td>coldPath_expanded</td><td>A string that contains an absolute path to the cold databases for the index.</td></tr>
<tr><td>coldToFrozenDir</td><td>A string that contains the destination path for the frozen archive. Use as an alternative to the "coldToFrozenScript" parameter. The "coldToFrozenDir" parameter takes precedence over "coldToFrozenScript" if both are specified.</td></tr>
<tr><td>coldToFrozenScript</td><td>A string that contains the destination path to the archiving script. If your script requires a program to run it (for example, python), specify the program followed by the path. The script must be in $SPLUNK_HOME/bin or one of its subdirectories.</td></tr>
<tr><td>compressRawdata</td><td>This parameter is ignored.</td></tr>
<tr><td>currentDBSizeMB</td><td>A number that indicates the total size of data stored in the index, in MB. This total includes data in the home, cold, and thawed paths.</td></tr>
<tr><td>defaultDatabase</td><td>A string that contains the index destination, which is used when index destination information is not available in the input data.</td></tr>
<tr><td>disabled</td><td>A Boolean that indicates whether the index has been disabled.</td></tr>
<tr><td>eai:acl</td><td>A string that contains the access control list for this input.</td></tr>
<tr><td>eai:attributes</td><td>A string that contains the metadata for this input.</td></tr>
<tr><td>enableOnlineBucketRepair</td><td>A Boolean that indicates whether to enable asynchronous online fsck bucket repair, which runs in a concurrent process with Splunk. When enabled, you do not have to wait until buckets are repaired to start Splunk. However, you might observe a slight performance degradation.</td></tr>
<tr><td>enableRealtimeSearch</td><td>A Boolean that indicates whether real-time search is enabled. This setting is global, not per index.</td></tr>
<tr><td>frozenTimePeriodInSecs</td><td>A number that indicates how many seconds after which indexed data rolls to frozen.</td></tr>
<tr><td>homePath</td><td>A string that contains a file path to the hot and warm buckets for the index.</td></tr>
<tr><td>homePath_expanded</td><td>A string that contains an absolute file path to the hot and warm buckets for the index.</td></tr>
<tr><td>indexThreads</td><td>A number that indicates how many threads are used for indexing. This setting is global, not per index.</td></tr>
<tr><td>isInternal</td><td>A Boolean that indicates whether the index in internal.</td></tr>
<tr><td>lastInitTime</td><td>A string that contains the last time the index processor was successfully initialized. This setting is global, not per index.</td></tr>
<tr><td>maxBloomBackfillBucketAge</td><td>A string that indicates the age of the bucket. If a warm or cold bucket is older than this time, Splunk does not create (or re-create) its bloom filter. The valid format is number followed by a time unit ("s", "m", "h", or "d"), for example "5d".</td></tr>
<tr><td>maxConcurrentOptimizes</td><td>A number that indicates how many concurrent optimize processes can run against a hot bucket.</td></tr>
<tr><td>maxDataSize</td><td>A string that indicates the maximum size for a hot bucket to reach before a roll to warm is triggered. The valid format is a number in MB, "auto" (Splunk auto-tunes this value, setting the size to 750 MB), or "auto_high_volume" (for high-volume indexes such as the main index, setting the size to 10 GB on 64-bit, and 1 GB on 32-bit systems).</td></tr>
<tr><td>maxHotBuckets</td><td>A number that indicates the maximum number of hot buckets that can exist per index. When this value is exceeded, Splunk rolls the least recently used (LRU) hot bucket to warm. Both normal hot buckets and quarantined hot buckets count towards this total. This setting operates independently of "maxHotIdleSecs", which can also cause hot buckets to roll.</td></tr>
<tr><td>maxHotIdleSecs</td><td>A number that indicates the maximum life, in seconds, of a hot bucket. When this value is exceeded, Splunk rolls the hot bucket to warm. This setting operates independently of "maxHotBuckets", which can also cause hot buckets to roll. A value of 0 turns off the idle check.</td></tr>
<tr><td>maxHotSpanSecs</td><td>A number that indicates the upper bound, in seconds, of the target maximum timespan of hot and warm buckets. If this value is set too small, you can get an explosion of hot and warm buckets in the file system.</td></tr>
<tr><td>maxMemMB</td><td>A number that indicates the amount of memory, in MB, that is allocated for indexing.</td></tr>
<tr><td>maxMetaEntries</td><td>A number that indicates the maximum number of unique lines in .data files in a bucket, which may help to reduce memory consumption. When set to 0, this parameter is ignored. When this value is exceeded, a hot bucket is rolled to prevent further increase.</td></tr>
<tr><td>maxRunningProcessGroups</td><td>A number that indicates the maximum number of processes that the indexer creates at a time. This setting is global, not per index.</td></tr>
<tr><td>maxTime</td><td>A string that contains the UNIX timestamp of the newest event time in the index.</td></tr>
<tr><td>maxTimeUnreplicatedNoAcks</td><td>A number that specifies the upper limit, in seconds, on how long an event can remain in a raw slice. This value applies only when replication is enabled for this index.</td></tr>
<tr><td>maxTimeUnreplicatedWithAcks</td><td>A number that specifies the upper limit, in seconds, on how long events can remain unacknowledged in a raw slice. This value applies only when acks are enabled on forwarders and replication is enabled (with clustering).</td></tr>
<tr><td>maxTotalDataSizeMB</td><td>A number that indicates the maximum size of an index, in MB. If an index grows larger than the maximum size, the oldest data is frozen.</td></tr>
<tr><td>maxWarmDBCount</td><td>A number that indicates the maximum number of warm buckets. If this number is exceeded, the warm buckets with the lowest value for their latest times are moved to cold.</td></tr>
<tr><td>memPoolMB</td><td>A number that indicates how much memory is given to the indexer memory pool. This setting is global, not per index.</td></tr>
<tr><td>minRawFileSyncSecs</td><td>A string that indicates how frequently splunkd forces a file system sync while compressing journal slices. This value can be either an integer or "disable". If set to 0, splunkd forces a file system sync after every slice has finished compressing. If set to "disable", syncing is disabled and uncompressed slices are removed as soon as compression is complete. Some file systems are very inefficient at performing sync operations, so only enable this if you are sure it is needed. During this interval, uncompressed slices are left on disk even after they are compressed, then splunkd forces a file system sync of the compressed journal and removes the accumulated uncompressed files.</td></tr>
<tr><td>minTime</td><td>A string that contains the UNIX timestamp of the oldest event time in the index.</td></tr>
<tr><td>name</td><td>A string that contains the name of the index.</td></tr>
<tr><td>numBloomfilters</td><td>A number that indicates how many bloom filters are created for this index.</td></tr>
<tr><td>numHotBuckets</td><td>A number that indicates how many hot buckets are created for this index.</td></tr>
<tr><td>numWarmBuckets</td><td>A number that indicates how many warm buckets are created for this index.</td></tr>
<tr><td>partialServiceMetaPeriod</td><td>A number that indicates how often to sync metadata, in seconds, but only for records where the sync can be done efficiently in place, without requiring a full re-write of the metadata file. Records that require a full re-write are synced at the frequency specified by "serviceMetaPeriod". When set to 0 or a value greater than "serviceMetaPeriod", metadata is not partially synced, but is synced at the frequency specified by "serviceMetaPeriod".</td></tr>
<tr><td>quarantineFutureSecs</td><td>A number that indicates a time, in seconds. Events with a timestamp of this value newer than "now" are dropped into a quarantine bucket. This is a mechanism to prevent main hot buckets from being polluted with fringe events.</td></tr>
<tr><td>quarantinePastSecs</td><td>A number that indicates a time, in seconds. Events with timestamp of this value older than "now" are dropped into a quarantine bucket. This is a mechanism to prevent the main hot buckets from being polluted with fringe events.</td></tr>
<tr><td>rawChunkSizeBytes</td><td>A number that indicates the target uncompressed size, in bytes, for individual raw slice in the raw data journal of the index. If set to 0, "rawChunkSizeBytes" is set to the default value. Note that this value specifies a target chunk size. The actual chunk size may be slightly larger by an amount proportional to an individual event size.</td></tr>
<tr><td>repFactor</td><td>A string that contains the replication factor, which is a non-negative number or "auto". This value only applies to Splunk clustering slaves.</td></tr>
<tr><td>rotatePeriodInSecs</td><td>A number that indicates how frequently, in seconds, to check whether a new hot bucket needs to be created, and how frequently to check if there are any warm or cold buckets that should be rolled or frozen.</td></tr>
<tr><td>serviceMetaPeriod</td><td>A number that indicates how frequently metadata is synced to disk, in seconds.</td></tr>
<tr><td>summarize</td><td>A Boolean that indicates whether to omit certain index details to provide a faster response. This parameter is only used when retrieving the index collection.</td></tr>
<tr><td>suppressBannerList</td><td>A string that contains a list of indexes to suppress "index missing" warning banner messages for. This setting is global, not per index.</td></tr>
<tr><td>sync</td><td>A number that indicates how many events can trigger the indexer to sync events. This setting is global, not per index.</td></tr>
<tr><td>syncMeta</td><td>A Boolean that indicates whether to call a sync operation before the file descriptor is closed on metadata file updates.</td></tr>
<tr><td>thawedPath</td><td>A string that contains the file path to the thawed (resurrected) databases for the index.</td></tr>
<tr><td>thawedPath_expanded</td><td>A string that contains the absolute file path to the thawed (resurrected) databases for the index.</td></tr>
<tr><td>throttleCheckPeriod</td><td>A number that indicates how frequently Splunk checks for index throttling condition, in seconds.</td></tr>
<tr><td>totalEventCount</td><td>A number that indicates the total number of events in the index.</td></tr>
</table>