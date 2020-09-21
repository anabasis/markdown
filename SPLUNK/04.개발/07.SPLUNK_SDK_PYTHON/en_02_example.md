# Examples

## Examples(소제목)

OK, now that you've got the Splunk® SDK for Python installed, it's time to start playing with it.

### Start with the ABC example

The Splunk SDK for Python consists of different modules that can interact with Splunkd (the service used for accessing, processing, indexing, and searching your data). The two modules that do most of the work are:

- The binding module (splunklib.binding), which provides a thin abstraction over raw HTTP. It also handles authentication, remembers the session key, and appends the Authorization header to all requests.
- The client module (splunklib.client), which builds on the binding module and provides an abstraction layer over the REST API, allowing you to access the endpoints.

So while we don't have an actual "Hello World" example, the ABC example (/splunk-sdk-python/examples/abc) in the Splunk SDK for Python provides some basic code examples to show how making calls using the REST API directly differs from using the Splunk SDK for Python. Each example retrieves a list of the installed Splunk apps, but the code gets progressively simpler as you go from using the REST API directly (a.py), to the binding layer (b.py), and then to the client layer (c.py). Before you run these examples, be sure to update the files with your own login credentials, which are hard coded and don't use the .splunkrc convenience file.
For more about the architecture of the Splunk SDK for Python, see [The Splunk SDK for Python architecture](http://dev.splunk.com/view/python-sdk/SP-CAAAEBB#architecture).

### Dig into the SDK examples

The Splunk SDK for Python has a lot more examples for you to try out. Go to the /splunk-sdk-python/examples directory, and you'll find a collection of [command-line examples](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK) that cover the basic tasks, such as starting a Splunk session and logging in, running search queries and saved searches, working with indexes and inputs, and so on.
If you haven't found an example of how to use a specific API, check out the [unit tests](http://dev.splunk.com/view/python-sdk/SP-CAAAEDH).

## Command line examples in the Splunk SDK for Python

Examples are located in the /splunk-sdk-python/examples directory. To run the examples at the command line, use the Python interpreter and include any arguments that are required by the example.

```bash
python examplename.py --username="admin" --password="yourpassword"
```

If you saved your login credentials in the .splunkrc file, you can omit those arguments:

```bash
python examplename.py
```

To get help for an example, use the --help argument with an example:

```bash
python examplename.py --help
```

### Run examples

Here are some different command-line examples to show how to use the SDK examples. Make sure Splunk Enterprise is running, and then open a command prompt in the /splunk-sdk-python/examples directory.

- [Run a search and display formatted results](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#searchresults)
- [Run a simple oneshot search](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#oneshot)
- [Work with search jobs](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#job)
- [Display Splunk system info](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#info)
- [List your data inputs](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#inputs)
- [Upload a data input file](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#upload)
- [Work with data indexes](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#index)
- [Display events as they are indexed](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#stail)
- [Generate sample events for testing](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#genevents)
- [Work with Splunk configuration files](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#conf)
- [List Splunk server logging categories](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#loggers)
- [Run GET commands for Splunk REST API endpoints](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#spurl)
- [Run Splunk's interactive Python interpreter](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#spcmd)
- [List saved event types](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#eventtypes)
- [List fired alerts](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#firedalerts)
- [Explore the REST API](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#explorer)
- [Export indexed events to a file](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#export)
- [Work with saved searches](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK#saved_search)

### Run a search and display formatted results

The search.py example runs a search and returns the results, using parameters to customize your searches. Running this example creates a search job that is deleted once the results are returned. Use --help to list the available options. For a deeper description of what these parameters mean, look for them in the Requests table under POST search/jobs in the Splunk REST API documentation.

Here's how to search everything, return results in CSV format, and display progress:

```bash
python search.py "search *" --output_mode=csv --verbose=verbose
```

This example searches everything after a specified time, requires the "desc" field, and returns the first 10 results in JSON format:

```bash
python search.py "search * | head 10" --earliest_time="2011-03-12T17:15:00.000-07:00" --rf="desc" --output_mode=json
```

Use the Results.py example to format your search results. This example removes the XML tags and condenses the output, so the results are easier to read than XML. To use this example, pipe a search.py command to a results.py command:

```bash
python search.py "search * | head 10" | python results.py
```

### Run a simple oneshot search

The oneshot.py example is a simpler search example that just runs a search and returns results in the same call (this example hard-codes --exec_mode=oneshot). This example also reformats the XML results automatically. Here's how to search everything and return the first 10 results:

```bash
python oneshot.py "search * | head 10"
```

### Work with search jobs

The job.py example works with search jobs and perform different actions on them. For example, here's how to list your current search jobs:

```bash
python job.py list
```

Here's how to display the properties of a specific job—you specify either the index value or search ID:

```bash
python job.py list @4              # Print the fourth job in the queue
python job.py list 1354642929.43   # Print a job specified by a search ID
```

### Display Splunk system info

The info.py example takes no arguments and simply prints system information about your Splunk instance to the console:

```bash
python info.py
```

### List your data inputs

The inputs.py example enumerates the data inputs that have been set up for your Splunk Enterprise instance, and the properties of each:

```bash
python inputs.py
```

### Upload a data input file

The upload.py example adds a data input file. This command uploads the sampledata.zip file (from the Splunk Tutorial) to the "test_index" index:

```bash
python upload.py /Users/myusername/Downloads/sampledata.zip --index="test_index"
```

### Work with data indexes

The index.py example works with the indexes that store your Splunk data. When you run the index.py example with the list argument alone, it lists all indexes along with the number of events in each:

```bash
python index.py list
```

You can also specify an action (create, clean, enable, disable, reload, update, list) to perform on a specific index. This shows how to list properties for the "test" index:

```bash
python index.py list test
```

This shows how to clean the summary and test indexes:

```bash
python index.py clean summary test
```

### Display events as they are indexed

The stail.py example prints events to the console as they are indexed (the "tail" of a real-time search). For example, this command prints incoming events to the "twitter" index:

```bash
python stail.py "search index=twitter"
```

### Generate sample events for testing

The genevents.py example is a simple event generator that writes 50,000 short time-stamped events to a specified index. You can also specify the way the data is received: over an HTTP connection (stream, the default), over a TCP connection (TCP), or over individually-constructed HTTP connections per event (submit).

For example, this adds events to the "main" index over a TCP connection:

```bash
python genevents.py main tcp
```

Use genevents.py for testing when you need a bunch of events. For example, you can use genevents.py with the stail.py example to display events as they are received in a "test" index. Open two command-prompt windows. In one, enter:

```bash
python stail.py "search index=test"
```

Then, in the other window, enter:

```bash
python genevents.py test
```

### Work with Splunk configuration files

The conf.py example lets you work with Splunk configuration (.conf) files. This command lists the .conf files in $SPLUNK_HOME/etc/system and $SPLUNK_HOME/etc/users (depending on your user account permissions):

```bash
python conf.py list
```

To display the content of a specific .conf file, such as inputs.conf:

```bash
python conf.py list inputs
```

### List Splunk server logging categories

The loggers.py example lists the Splunk server logging categories and their current logging level:

```bash
python loggers.py
```

### Run GET commands for Splunk REST API endpoints

The spurl.py example runs a GET command for any endpoint in the Splunk REST API, and returns the Atom Feed response. These examples use two different endpoints:

```bash
python spurl.py /services/data/indexes
python spurl.py /services/saved/searches
```

### Run Splunk's interactive Python interpreter

The spcmd.py example starts an interactive Python interpreter for the Splunk SDK for Python. This interpreter is similar to using the regular Python interpreter, but this Splunk version automatically logs in and connects to your Splunk instance (taking your login credentials from the .splunkrc file):

```bash
python spcmd.py
```

Once the interpreter is running, you can enter single Python commands. For example, next you could run a simple search:

```python
print service.jobs.oneshot("search * | head 10")
```

Or, list the Splunk apps that are installed:

```python
for app in service.apps: print app.name
```

To quit the interpreter press Ctrl+D, or enter:

```python
quit()
```

### List saved event types

The event_types.py example simply lists your saved event types, which are saved searches that do not include a pipe operator or a subsearch.

This lists all of your saved event types:

```bash
python event_types.py
```

### List fired alerts

The fired_alerts.py example lists a summary of the alerts that were fired on the server.
This lists all of your fired alerts:

```bash
python fired_alerts.py
```

### Explore the REST API

The explorer.py example, which is located in the /splunk-sdk-python/examples/explorer directory, lets you interact with all of the endpoints in the Splunk Enterprise REST API from a web page interface. You can select an endpoint, set parameters, and submit the request. If successful, the web page displays the Atom Feed response to the REST API call.
To run this example and launch the explorer.html page in a web browser, open a command prompt in the /splunk-sdk-python/examples/explorer directory and enter:

```bash
python explorer.py
```

### Export indexed events to a file

The export.py example, which is located in the /splunk-sdk-python/examples/export directory, takes events from an index and saves them to a file, export.out, in the same directory. You can export events in XML, CSV, or JSON format.

This exports the main index:

```bash
python export.py --index=main
```

### Work with saved searches

The SDK includes two examples for working with saved searches:

- The saved_searches.py example, which is located in the /splunk-sdk-python/examples directory, simply lists your saved searches.
- The saved_search.py example, which is located in the /splunk-sdk-python/examples/saved_search directory, lets you list your saved searches, but also lets you view properties for a specific saved search and delete a saved search.

From the /splunk-sdk-python/examples directory, this lists all of your saved searches in an easy-to-read format:

```bash
python saved_searches.py
```

This also lists saved searches, but in the Atom Feed format:

```bash
python saved_search/saved_search.py list-all
```

You can also list the details of one specific search (run one of the previous commands to retrieve names):

```bash
python saved_search/saved_search.py list --name="Name of a saved search"
```

This deletes a saved search:

```bash
python saved_search/saved_search.py --operation="delete" --name="Name of a saved search"
```

## Unit tests

A great place to look for examples of how to use the Splunk® SDK for Python is in the unit tests. These are the same tests that we used to validate the core SDK library.

The test suite uses Python's standard library and the built-in unittest library.

> Note: If you're using Python 2.7, you're all set. However, if you are using Python 2.6, you'll also need to install the unittest2 library to get the additional features that were added to Python 2.7 (just run pip install unittest2 or easy_install unittest2).

To run the unit tests, you'll first need to install several test apps in Splunk Enterprise:

1. Open a command prompt in the splunk-sdk-python directory.
2. Enter the following, and then press Enter:
    ```bash
    python setup.py build dist
    ```
    This command creates a new build directory in the splunk-sdk-python directory with three test Splunk apps inside: github_forks.spl, random_numbers.spl, and searchcommands_app.spl.
3. Install each app into your Splunk server. For instructions, see the "[If you are not connected to the internet](http://docs.splunk.com/Documentation/Splunk/latest/Admin/Wheretogetmoreapps#If_you_are_not_connected_to_the_internet)" section of "[Where to get more apps and add-ons](http://docs.splunk.com/Documentation/Splunk/latest/Admin/Wheretogetmoreapps#If_you_are_not_connected_to_the_internet)".

After you've installed the three apps, you're ready to run the tests. Open a command prompt in the splunk-sdk-python directory and enter the following:

```bash
python setup.py test
```

> Note: You can safely ignore "Test requires sdk-app-collection. Skipping." messages.

You can also run individual test files, which are located in /splunk-sdk-python/tests. For example, to run the apps test, open a command prompt in the /splunk-sdk-python/tests subdirectory and enter:

```bash
python test_app.py
```

### Code coverage

Coverage.py is an excellent tool for measuring code coverage of Python programs.
To install it, use Easy Install:

```bash
easy_install coverage
```

Or use pip:

```bash
pip install coverage
```

To generate a report of the code coverage of the unit test suite, open a command prompt in the /splunk-sdk-python directory and enter:

```bash
python setup.py coverage
```

This command runs the entire test suite and writes an HTML coverage report to the /splunk-sdk-python/coverage_report directory.
For more information about Coverage.py, see the author's website(<http://nedbatchelder.com/code/coverage/>).