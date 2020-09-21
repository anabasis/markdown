# How to create custom search commands using Splunk SDK for Python

A custom search command is a Python script that reads input from stdin and writes output to stdout. Input comes in as CSV (with an optional header), and is in general meant to be read using Python's stdlib csv module (using csv.reader or csv.DictReader). Output is also expected to be in CSV, and is likewise meant to be used with csv.writer or csv.DictWriter.

There are two subtypes of custom search commands:

- A streaming custom search command is one to which data is streamed. You can think of it as applying a "function"/"transformation" to each event and then writing out the result of that operation. It is a kind of mapper. An example of such a command might be a command that adds a field to each event.
- A non-streaming custom search command expects to have all the data before it operates on it. As such, it is usually "reducing" the data into the output by applying some sort of summary transformation on it. An example of a non-streaming command is the stats command, which will collect all the data before it can calculate the statistics.

Neither of these cases precludes having previews of the data, and you can enable or disable preview functionality in the configuration.

Until now, building your own custom search commands from scratch for Splunk Enterprise using Python has not been easy. The learning curve is steep, plus configuring commands properly and conforming to the search command style guide is difficult. There is also no built-in logging configuration mechanism.

To simplify the creation of custom search commands, the Splunk SDK for Python includes the splunklib.searchcommands module, which simplifies the creation of custom search commands and adds logging functionality. In addition, the Splunk SDK for Python includes several templates that you can use to easily build custom search commands with just a few modifications.

This topic contains the following sections:

- Module structure
  - EventingCommand class
  - GeneratingCommand class
  - ReportingCommand class
  - StreamingCommand class
- Custom search command starter example
- Custom search command basic example
- Custom search command 'shape' example
- Custom search command template
- Control access to a custom search command
  - What you can edit in Splunk Web
  - What you can edit in .conf files

## Module structure

The core of the search commands module consists of four classes, each of which represents the first three custom search command types:

- [EventingCommand](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.EventingCommand): Applies a transformation to search results as they travel through the events pipeline. Examples of generating commands include sort_, dedup_, and cluster_.
- [GeneratingCommand](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.GeneratingCommand): Generates event records based on command arguments. Examples of generating commands include search (at the beginning of the pipeline), inputcsv, input lookup, and metadata.
- [ReportingCommand](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.ReportingCommand) : Processes search results and generates a reporting data structure. Examples of reporting commands include stats, top, and timechart.
- [StreamingCommand](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.StreamingCommand) : Applies a transformation to search results as they travel through the processing pipeline. Examples of streaming commands include search, eval, and where.

In addition you can use the following helper classes:

- The [Configuration class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.Configuratgion) negates the need for you to manually configure the [commands.conf](http://docs.splunk.com/Documentation/Splunk/latest/Admin/Commandsconf) file by enabling you to dynamically configure commands in your code.
- The [Option class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.Option) eliminates the need for you to parse and validate search command arguments manually by enabling you to simply include them as a required or optional property.

Finally, logging has been standardized with the searchcommands.logging module and framework error handling code.

### EventingCommand class

The [EventingCommand class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.EventingCommand) is the base class for dataset processing commands that filter results arriving at a search head from one or more search peers as they travel through the events pipeline.
Eventing commands typically filter, group, order, and/or or augment event records. Examples of eventing commands from the built-in Splunk Enterprise command set include [sort_](http://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Sort), [dedup_](http://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Dedup), and [cluster_](http://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Cluster). Each execution of an eventing command should produce a set of event records that is independently usable by downstream processors.

### GeneratingCommand class

The [GeneratingCommand class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.GeneratingCommand) generates events based on command arguments.
Implement a generate method as a generator function that yields `dict` or `list(dict)` instances. Generating commands receive no input, and must be the first command on a pipeline. By default, Splunk Enterprise runs your command locally on a search head.
@Configuration()
You can change the default behavior by specifying that the events should be streamed, as follows:

```python
@Configuration(streaming=True)
```

Splunk Enterprise runs the command locally (by default) or remotely on one or more indexers (if you've specified streaming). If you only want Splunk Enterprise to run the command locally on a search head, and not remotely on indexers, specify the following:

```python
@Configuration(streaming=True, local=True)
```

Be sure to tell Splunk Enterprise if your generating command produces event records in time order:

```python
@Configuration(generates_timeorder=True)
```

### ReportingCommand class

The [ReportingCommand class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.ReportingCommand) processes search results and generates a reporting data structure. Reporting search commands run as either reduce or map/reduce operations.
You can implement a `map` method as a generator function that iterates over a set of event records and yields `dict` or `list(dict)` instances. Configure the `map` operation using a `Configuration` decorator on the `ReportingCommand.map` method, and configure it as described in `StreamingCommand`.

### StreamingCommand class

The [StreamingCommand class](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/searchcommands.html#splunklib.searchcommands.StreamingCommand) applies a transformation to search results as they travel through the processing pipeline.

Implement a `stream` method as a `generator` function that iterates over a set of event records and yields `dict` or `list(dict)` instances.
Streaming commands typically filter, sort, modify, or combine search results. By default, Splunk Enterprise runs a streaming command locally on a search head or remotely on one or more indexers. If you only want Splunk Enterprise to run the streaming command locally on a search head, and not remotely on indexers, specify the following:

```python
@Configuration(local=True)
```

Be sure to tell Splunk Enterprise if your streaming command modifies the time order of event records:

```python
@Configuration(overrides_timeorder=True)
```

### Custom search command starter example

You can use the SDK's simple starter example to start building your own custom search command. The example consists of a custom Splunk Enterprise app, custom_search, that provides a single custom search command, usercount.

> Note: In addition to this starter example, be sure to read the section, "[Custom search command template](http://dev.splunk.com/view/python-sdk/SP-CAAAEU2#template)."

The purpose of the app is to provide an example of how to define a custom search command, how to configure it, and what the input and output should look like in order to work with Splunk Enterprise. The custom search command runs a Python script, `WinAD.py`, to collect Active Directory information. This sample Python script is available from Microsoft.
To get the complete custom search command, go to our GitHub repository: [Splunk GitHub Python SDK custom search command](http://github.com/splunk/splunk-sdk-python/tree/master/examples/custom_search).

### Add the Python script

Add this script, `WinAD.py`, to an appropriate apps directory, `$SPLUNK_HOME/etc/apps/<app_name>/bin/`:

```python
import win32com.client
strComputer = "."
objWMIService = win32com.client.Dispatch("WbemScripting.SWbemLocator")
objSWbemServices = objWMIService.ConnectServer(strComputer,"root\cimv2")
colItems = objSWbemServices.ExecQuery("Select * from Win32_NTDomain")
for objItem in colItems:
    print "Caption: ", objItem.Caption
    print "Client Site Name: ", objItem.ClientSiteName
    print "Creation Class Name: ", objItem.CreationClassName
    print "Dc Site Name: ", objItem.DcSiteName
    print "Description: ", objItem.Description
    print "Dns Forest Name: ", objItem.DnsForestName
    print "Domain Controller Address: ", objItem.DomainControllerAddress
    print "Domain Controller Address Type: ", objItem.DomainControllerAddressType
    print "Domain Controller Name: ", objItem.DomainControllerName
    print "Domain Guid: ", objItem.DomainGuid
    print "Domain Name: ", objItem.DomainName
    print "DS Directory Service Flag: ", objItem.DSDirectoryServiceFlag
    print "DS Dns Controller Flag: ", objItem.DSDnsControllerFlag
    print "DS Dns Domain Flag: ", objItem.DSDnsDomainFlag
    print "DS Dns Forest Flag: ", objItem.DSDnsForestFlag
    print "DS Global Catalog Flag: ", objItem.DSGlobalCatalogFlag
    print "DS Kerberos Distribution Center Flag: ",    objItem.DSKerberosDistributionCenterFlag
    print "DS Primary Domain Controller Flag: ", objItem.DSPrimaryDomainControllerFlag
    print "DS Time Service Flag: ", objItem.DSTimeServiceFlag
    print "DS Writable Flag: ", objItem.DSWritableFlag
    print "Install Date: ", objItem.InstallDate
    print "Name: ", objItem.Name
    print "Name Format: ", objItem.NameFormat
    print "Primary Owner Contact: ", objItem.PrimaryOwnerContact
    print "Primary Owner Name: ", objItem.PrimaryOwnerName
    z = objItem.Roles
    if z is None:
        a = 1
    else:
        for x in z:
            print "Roles: ", x
            print "Status: ", objItem.Status
```

### Edit configuration files

Edit these configuration files in the app's local directory, `$SPLUNK_HOME/etc/app/<app_name>/local/`.

In `commands.conf`, add this stanza:

```properties
[WinAD]
filename = WinAD.py
```

In `authorize.conf`, add these two stanzas:

```properties
[capability::run_script_WinAD]

[role_admin]
run_script_WinAD= enabled
```

Restart Splunk Enterprise.

### Run the command in Splunk Web

In the apps manager, modify the sharing for the search script so that it has Global Permissions.
Restart Splunk Enterprise.
Now you can run the command from the search bar. Because it is an event-generating command, it should start with a leading pipe ("|").

## Custom search command basic example

The SDK contains an example app, custom_search_commands, which includes three command types:

- `simulate.py`: A generating command that generates a sequence of events drawn from a set of event records stored in a CSV file using repeated random sampling.
- `sum.py`: A reporting command that adds all the numbers in a set of fields.
- `countmatches.py`: A streaming command that counts the number of non-overlapping matches to a regular expression in a set of fields.

The example app's directory, `searchcommands_app`, is contained in the SDK's `example` directory. Inside `searchcommands_app` is a `bin` directory and a `default` directory. To install the example:

1. Copy the splunk-sdk-python/examples/searchcommands_app directory to `$SPLUNK_HOME/etc/apps/`.
2. Inside the copied searchcommands_app directory, open the bin directory.
3. Inside the bin directory, create a new directory called splunklib.
4. Copy the splunk-sdk-python/splunklib/searchcommands directory to `$SPLUNK_HOME/etc/apps/searchcommands_app/bin/splunklib/`.
5. Restart Splunk Enterprise.

The `$SPLUNK_HOME/etc/apps/searchcommands_app/` directory's contents should now have the following hierarchy:

```bash
├── bin
│   ├── splunklib
│   │   └── searchcommands ....... splunklib.searchcommands module
│   ├── simulate.py .............. SimulateCommand implementation
│   ├── sum.py ................... SumCommand implementation
│   └── countmatches.py .......... CountMatchesCommand implementation
└── default
   ├── data
   │   └── ui
   │       └── nav
   │           └── default.xml ..
   ├── app.conf ................. Used by Splunk Enterprise to maintain app state
   ├── commands.conf ............ Search command configuration
   ├── logging.conf ............. Python logging configuration in ConfigParser format
   └── logging.debug.conf ....... Logs to app root directory as well as the Splunk Enterprise log file
```

## Custom search command 'shape' example

This following is a new command called shape that categorizes events based on their line count (tall or short) and line length (thin, wide, and very_wide) and whether or not they are indented.

### Add the Python script

Add this script, `shape.py`, to an appropriate apps directory, `$SPLUNK_HOME/etc/apps/<app_name>/bin/`:

```python
  import splunk.Intersplunk
  def getShape(text):
       description = []
       linecount = text.count("\n") + 1
       if linecount > 10:
           description.append("tall")
       elif linecount > 1:
           description.append("short")
       avglinelen = len(text) / linecount
       if avglinelen > 500:
           description.append("very_wide")
       elif avglinelen > 200:
           description.append("wide")
       elif avglinelen < 80:
           description.append("thin")
       if text.find("\n ") >= 0 or text.find("\n\t") >= 0:
           description.append("indented")
       if len(description) == 0:
           return "normal"
       return "_".join(description)
  # get the previous search results
  results,unused1,unused2 = splunk.Intersplunk.getOrganizedResults()
  # for each results, add a 'shape' attribute, calculated from the raw event text
  for result in results:
       result["shape"] = getShape(result["_raw"])
  # output results
  splunk.Intersplunk.outputResults(results)
```

### Edit configuration files

Edit these configuration files in the app's local directory, `$SPLUNK_HOME/etc/app/<app_name>/local/`.

In `commands.conf`, add this stanza:

```properties
[shape]
filename = shape.py
```

In authorize.conf, add these two stanzas:

```properties
[capability::run_script_shape]

[role_admin]
run_script_shape= enabled
```

Restart Splunk Enterprise.

## Custom search command template

The SDK contains a template for a custom search command app, searchcommands_template, which contains all three command types, each in their own file. You can delete any you don't need.

- `generate.py` : A generating command template.
- `report.py` : A reporting command template.
- `stream.py` : A streaming command template.

To create an app based on the template:

1. Make a copy of the template directory and place it somewhere convenient.
2. Rename the directory from searchcommands_template to what you want your app_id to be.
3. Delete any files within the bin directory that you don't need (generate.py, report.py and/or stream.py).
4. Rename the file(s) that remain in the bin directory to the names of the classes they (will) contain.
5. In the default directory, open the app.conf file. Now, replace the following tokens with your own values:
    - %(app_label)
    - %(app_description)
    - %(app_author)
    - %(app_version)
    - %(app_id)
6. Within each of the following files, replace the %(command.title()) label with the name of the generating, reporting, or streaming command class:
    - Within the bin directory, whichever of the following files remains: generate.py, report.py, stream.py
    - Within the default directory, the logging.conf file
7. Within the default directory, open the commands.conf file. Replace %(command.lower()) with the lowercase version of the name of the command. This must be identical to the name of the python script that contains the command class name.

To install your new custom search command, copy the directory to `$SPLUNK_HOME/etc/apps/`, and then restart Splunk Enterprise.

## Control access to a custom search command

By default, all roles have read-access to `commands.conf`, but only admins have write access. This means that all roles can run the commands listed in `commands.conf`, unless the access controls are explicitly changed for an individual command. If you want to restrict the usage of the command to certain roles or users, modify its access controls in Splunk Web or edit `default.meta.conf`.

### What you can edit in Splunk Web

You can use the Search commands management page to disable a search command that you don't want to run in an app:

1. Navigate to `Settings > Advanced search > Search commands`.
  This brings you to the table of search commands, which includes the following information: the command's name, the filename of the script that defines the command, the owner of the script, the app it belongs to, its sharing restrictions, and whether or not it is enabled.
    > Note: This table only lists the search commands that were written in Python.
2. Under the Status column for the search command, click Disable.

Splunk Enterprise will display a message banner saying that the command was disabled in the app.

You can also use the Search commands management page to change the role's access controls for a command:

1. Under the Sharing column for the search command, click Permissions.
  This opens the Permissions view for the search command. Use this page to specify:
    - If this command should appear in the current app or all apps.
    - Which roles are have read and write access to this command.
2. Don't forget to save your changes!

### What you can edit in .conf files

You can also change the access controls for a command using the `$SPLUNK_HOME/etc/apps/<app_name>/metadata/default.meta` file. For more information, see the [default.meta.conf](http://docs.splunk.com/Documentation/Splunk/latest/Admin/Defaultmetaconf) reference in the Admin Manual.

The following example shows the default access for commands.conf and the input command, which you cannot run unless you are an admin.

```properties
[commands]
access = read : [ * ], write : [ admin ]
export = system

[commands/input]
access = read : [ admin ], write : [ admin ]
```

There is also an access control restriction on the search script files themselves. These controls are defined in the [searchscripts] stanza. By default, the files are visible to all roles and apps, but only admins can edit them:

```python
[searchscripts]
access = read : [ * ], write : [ admin ]
export = system
```

Use the export = system attribute to make files available to all apps in the system. In the examples above, access to commands.conf and [searchscripts] are global. If the global export under [searchscripts] was not present, the script configurations (commands.conf) would be visible in all apps, but the script files themselves would not be.

> Note: Custom commands in apps that do not have a UI should also be exported to the system, since there is no way to run the command in a local context.

The [searchscripts] stanza also defines access for lookup scripts. Here are some example configurations.

```properties
[searchscripts/user_agents]
access = read : [ * ], write : [ admin, power ]
export = system

[searchscripts/user_agents.py]
access = read : [ * ], write : [ admin, power ]
export = system
```