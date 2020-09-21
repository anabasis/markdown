# How to run searches and jobs using the Splunk SDK for Python

Searches run in different modes, determining when and how you can retrieve results:

- Normal: A normal search runs asynchronously. It returns a search job immediately. Poll the job to determine its status. You can retrieve the results when the search has finished. You can also preview the results if "preview" is enabled. Normal mode works with real-time searches.
- Blocking: A blocking search runs synchronously. It does not return a search job until the search has finished, so there is no need to poll for status. Blocking mode doesn't work with real-time searches.
- One-shot: A one-shot search is a blocking search that is scheduled to run immediately. Instead of returning a search job, this mode returns the results of the search once completed. Because this is a blocking search, the results are not available until the search has finished.
- Export: An export search is another type of search operation that runs immediately, does not create a job for the search, and starts streaming results immediately.

For those searches that produce search jobs (normal and blocking), the search results are saved for a period of time on the server and can be retrieved on request. For those searches that stream the results (one-shot and export), the search results are not retained on the server. If the stream is interrupted for any reason, the results are not recoverable without running the search again.

## The job APIs

The classes for working with jobs are:

- The [splunklib.client.Jobs](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Jobs) class for the collection of search jobs.
- The [splunklib.client.Job](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job) class for an individual search job.

Access these classes through an instance of the [splunklib.client.Service](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Service) class. Retrieve a collection, and from there you can access individual items in the collection and create new ones. For example, here's a simplified program for getting a collection of jobs and creating a new one:

```python
# Connect to Splunk Enterprise
service = client.connect(...)

# Get the collection of search jobs
jobs = service.jobs

# Create a search job
job = jobs.create(<em>query</em>)
```

## Code examples

This section provides examples of how to use the job APIs, assuming you first [connect to a Splunk Enterprise instance](http://dev.splunk.com/view/python-sdk/SP-CAAAEE4):

- To list search jobs for the current user
- To create a blocking search and display properties of the job
- To create a normal search, poll for completion, and display results
- To create a basic one-shot search and display results

The following parameters are available for search jobs:

- Collection parameters
- Search job parameters (properties to set)
- Search job parameters (properties to retrieve)

### To list search jobs for the current user

This example shows how to use the splunklib.client.Jobs class to retrieve the collection of jobs available to the current user:

```python
# Get the collection of jobs and find out how many there are
jobs = service.jobs

print "There are", len(jobs), "jobs available to the current user"
print "\nSearch IDs:\n   " + "\n   ".join([job.sid for job in jobs])
```

### To create a blocking search and display properties of the job

Running a blocking search creates a search job and runs the search synchronously in "blocking" mode. The job is returned after the search has finished and all the results are in.

When you create a search job, you need to set the parameters of the job as a dictionary of key-value pairs. For a list of all the possible parameters, see [Search job parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE5#searchjobparams).

This example runs a blocking search, waits for the job to finish, and then displays some final statistics (see the example after this one to display the results):

```python
# Get the collection of jobs
jobs = service.jobs

# Run a blocking search--search everything, return 1st 100 events
kwargs_blockingsearch = {"exec_mode": "blocking"}
searchquery_blocking = "search * | head 100"

print "Wait for the search to finish..."

# A blocking search returns the job's SID when the search is done
job = jobs.create(searchquery_blocking, **kwargs_blockingsearch)
print "...done!\n"

# Get properties of the job
print "Search job properties"
print "Search job ID:        ", job["sid"]
print "The number of events: ", job["eventCount"]
print "The number of results:", job["resultCount"]
print "Search duration:      ", job["runDuration"], "seconds"
print "This job expires in:  ", job["ttl"], "seconds"
```

### To create a normal search, poll for completion, and display results

Running a normal search creates a search job and immediately returns the search ID, so you need to poll the job to find out when the search has finished.

When you create a search job, set the parameters of the job as named arguments to the [Jobs.create](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Jobs.create) method. For a list of all the possible parameters, see [Search job parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAEE5#searchjobparams).

This example runs a normal search, waits for the job to finish, and then displays the results in XML using the [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#module-splunklib.results) class along with some final statistics:

```python
import sys
from time import sleep
import splunklib.results as results

# ...
#
# Initialize your service like so
# import splunklib.client as client
# service = client.connect(username="admin", password="yourpassword")

searchquery_normal = "search * | head 10"
kwargs_normalsearch = {"exec_mode": "normal"}
job = service.jobs.create(searchquery_normal, **kwargs_normalsearch)

# A normal search returns the job's SID right away, so we need to poll for completion
while True:
    while not job.is_ready():
        pass
    stats = {"isDone": job["isDone"],
             "doneProgress": float(job["doneProgress"])*100,
              "scanCount": int(job["scanCount"]),
              "eventCount": int(job["eventCount"]),
              "resultCount": int(job["resultCount"])}

    status = ("\r%(doneProgress)03.1f%%   %(scanCount)d scanned   "
              "%(eventCount)d matched   %(resultCount)d results") % stats

    sys.stdout.write(status)
    sys.stdout.flush()
    if stats["isDone"] == "1":
        sys.stdout.write("\n\nDone!\n\n")
        break
    sleep(2)

# Get the results and display them
for result in results.ResultsReader(job.results()):
    print result

job.cancel()
sys.stdout.write('\n')
```

### To create a basic one-shot search and display results

The simplest way to get data out of Splunk Enterprise is with a one-shot search, which creates a synchronous search. Unlike normal or blocking searches, the one-shot search does not create and return a search job, but rather it blocks until the search finishes and then returns a stream containing the events. To set properties for the search (for example, to specify a time range to search), create a dictionary with the property key-value pairs. Some common properties are:

- output_mode: Specifies the output format of the results (XML, JSON, JSON_COLS, JSON_ROWS, CSV, ATOM, or RAW).
- earliest_time: Specifies the earliest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.
- latest_time: Specifies the latest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.
- rf: Specifies one or more fields to add to the search.

For a full list of possible properties, see the list of Search job parameters, although most of these parameters don't apply to a one-shot search.

This example runs a one-shot search within a specified time range by calling the [one-shot](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Jobs.oneshot) method. It then displays the results in XML using the [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) class.

> Note : If you don't see any search results, that means there aren't any in the specified time range. Just modify the date and time as needed for your data set.

```python
import splunklib.results as results
...
# Run a one-shot search and display the results using the results reader

# Set the parameters for the search:
# - Search everything in a 24-hour time range starting June 19, 12:00pm
# - Display the first 10 results
kwargs_oneshot = {"earliest_time": "2014-06-19T12:00:00.000-07:00",
                  "latest_time": "2014-06-20T12:00:00.000-07:00"}
searchquery_oneshot = "search * | head 10"

oneshotsearch_results = service.jobs.oneshot(searchquery_oneshot, **kwargs_oneshot)

# Get the results and display them using the ResultsReader
reader = results.ResultsReader(oneshotsearch_results)
for item in reader:
    print(item)
```

> Important: By default, one-shot searches will return a maximum of 100 events, even if there are more than 100 events in the search results. To return more than 100 events, add the following parameter to your one-shot search's arguments:
> "count": 0
> The count parameter, when set to zero, indicates that there is no limit to the number of events to be returned.

### To create a basic export search and display results

An export search is the most reliable way to return a large set of results because exporting returns results in a stream, rather than as a search job that is saved on the server. So any server-side limitations to the number of results that can be returned don't apply to export searches.

You can run an export search in normal and real-time modes. Export searches run right away, and immediately start streaming data. Running an export search is more efficient than running a preview search because it streams results directly to you, instead of having to write them out to disk to make them available later. As soon as results are ready, you will receive them.

Like one-shot searches, export searches run immediately, do not create jobs for the search, and start streaming results immediately. To set properties for the search (for example, to specify a time range to search), create a dictionary with the property key-value pairs. Some common properties are:

- search_mode: Specifies the search mode (normal or realtime).
- earliest_time: Specifies the earliest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.
- latest_time: Specifies the latest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.
- rf: Specifies one or more fields to add to the search.

For a full list of possible properties, see the list of Search job parameters.

This example runs an [export](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Jobs.export) search of your internal index over the last hour by calling the export method. This example then displays the results in XML using the ResultsReader class. Finally, it displays whether the results are a preview from a running search or from a completed search. (For more about displaying preview results, see [To display preview results](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#preview).)

> Note : If you don't see any search results, that means there aren't any in the specified time range. Just modify the date and time as needed for your data set.

```python
import splunklib.results as results
...
# Run an export search and display the results using the results reader.

# Set the parameters for the search:
# - Search everything in the last hour
# - Run a normal-mode search
# - Search internal index
kwargs_export = {"earliest_time": "-1h",
                 "latest_time": "now",
                 "search_mode": "normal"}
searchquery_export = "search index=_internal"

exportsearch_results = service.jobs.export(searchquery_export, **kwargs_export)

# Get the results and display them using the ResultsReader
reader = results.ResultsReader(exportsearch_results)
for result in reader:
    if isinstance(result, dict):
        print "Result: %s" % result
    elif isinstance(result, results.Message):
        # Diagnostic messages may be returned in the results
        print "Message: %s" % result

# Print whether results are a preview from a running search
print "is_preview = %s " % reader.is_preview
```

### Collection parameters

By default, all entries are returned when you retrieve a collection. But by using the parameters below, you can also specify the number of entities to return and how to sort them. These parameters are available whenever you retrieve a collection.

<table>
<tr><td>Parameter</td><td>Description</td></tr>
<tr><td>count</td><td>A number that indicates the maximum number of entities to return. A value of "0" indicates no maximum.</td></tr>
<tr><td>offset</td><td>A number that specifies the index of the first entity to return.</td></tr>
<tr><td>search</td><td>A string that specifies a search expression to filter the response with, matching field values against the search expression. For example, "search=foo" matches any object that has "foo" as a substring in a field, and "search=field_name%3Dfield_value" restricts the match to a single field.</td></tr>
<tr><td>sort_dir</td><td>An enum value that specifies how to sort entities. Valid values are "asc" (ascending order) and "desc" (descending order).</td></tr>
<tr><td>sort_key</td><td>A string that specifies the field to sort by.</td></tr>
<tr><td>sort_mode</td><td>An enum value that specifies how to sort entities. Valid values are "auto", "alpha" (alphabetically), "alpha_case" (alphabetically, case sensitive), or "num" (numerically).</td></tr>
</table>

### Search job parameters

#### Properties to set

The parameters you can use for search jobs correspond to the parameters for the [search/jobs endpoint](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#search.2Fjobs) in the REST API.

This table summarizes the properties you can set for a search job. For examples of setting these properties, see [To create a blocking search and display properties of the job](http://dev.splunk.com/view/python-sdk/SP-CAAAEE5#blockingjob) and [To create a normal search, poll for completion, and display results](http://dev.splunk.com/view/python-sdk/SP-CAAAEE5#normaljob).

<table>
<tr><td>Parameter</td><td>Description</td></tr>
<tr><td>search</td><td>Required. A string that contains the search query.</td></tr>
<tr><td>auto_cancel</td><td>The number of seconds of inactivity after which to automatically cancel a job. 0 means never auto-cancel.</td></tr>
<tr><td>auto_finalize_ec</td><td>The number of events to process after which to auto-finalize the search. 0 means no limit.</td></tr>
<tr><td>auto_pause</td><td>The number of seconds of inactivity after which to automatically pause a job. 0 means never auto-pause.</td></tr>
<tr><td>earliest_time</td><td>A time string that specifies the earliest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string. For a real-time search, specify "rt".</td></tr>
<tr><td>enable_lookups</td><td>A Boolean that indicates whether to apply lookups to events.</td></tr>
<tr><td>exec_mode</td><td>An enum value that indicates the search mode ("blocking", "oneshot", or "normal").</td></tr>
<tr><td>force_bundle_replication</td><td>A Boolean that indicates whether this search should cause (and wait depending on the value of "sync_bundle_replication") bundle synchronization with all search peers.</td></tr>
<tr><td>id</td><td>A string that contains a search ID. If unspecified, a random ID is generated.</td></tr>
<tr><td>index_earliest</td><td>A string that specifies the time for the earliest (inclusive) time bounds for the search, based on the index time bounds. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.</td></tr>
<tr><td>index_latest</td><td>A string that specifies the time for the latest (inclusive) time bounds for the search, based on the index time bounds. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string.</td></tr>
<tr><td>latest_time</td><td>A time string that specifies the latest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string. For a real-time search, specify "rt".</td></tr>
<tr><td>max_count</td><td>The number of events that can be accessible in any given status bucket.</td></tr>
<tr><td>max_time</td><td>The number of seconds to run this search before finalizing. Specify 0 to never finalize.</td></tr>
<tr><td>namespace</td><td>A string that contains the application namespace in which to restrict searches.</td></tr>
<tr><td>now</td><td>A time string that sets the absolute time used for any relative time specifier in the search.</td></tr>
<tr><td>reduce_freq</td><td>The number of seconds (frequency) to run the MapReduce reduce phase on accumulated map values.</td></tr>
<tr><td>reload_macros</td><td>A Boolean that indicates whether to reload macro definitions from the macros.conf configuration file.</td></tr>
<tr><td>remote_server_list</td><td>A string that contains a comma-separated list of (possibly wildcarded) servers from which to pull raw events. This same server list is used in subsearches.</td></tr>
<tr><td>rf</td><td>A string that adds one or more required fields to the search.
<tr><td>rt_blocking</td><td>A Boolean that indicates whether the indexer blocks if the queue for this search is full. For real-time searches.</td></tr>
<tr><td>rt_indexfilter</td><td>A Boolean that indicates whether the indexer pre-filters events. For real-time searches.</td></tr>
<tr><td>rt_maxblocksecs</td><td>The number of seconds indicating the maximum time to block. 0 means no limit. For real-time searches with "rt_blocking" set to "true".</td></tr>
<tr><td>rt_queue_size</td><td>The number indicating the queue size (in events) that the indexer should use for this search. For real-time searches.</td></tr>
<tr><td>search_listener</td><td>A string that registers a search state listener with the search. Use the format: search_state;results_condition;http_method;uri;</td></tr>
<tr><td>search_mode</td><td>An enum value that indicates the search mode ("normal" or "realtime"). If set to "realtime", searches live data. A real-time search is also specified by setting "earliest_time" and "latest_time" parameters to "rt", even if the search_mode is normal or is not set.</td></tr>
<tr><td>spawn_process</td><td>A Boolean that indicates whether to run the search in a separate spawned process. Searches against indexes must run in a separate process.</td></tr>
<tr><td>status_buckets</td><td>The maximum number of status buckets to generate. 0 means to not generate timeline information.</td></tr>
<tr><td>sync_bundle_replication</td><td>A Boolean that indicates whether this search should wait for bundle replication to complete.</td></tr>
<tr><td>time_format</td><td>A string that specifies the format to use to convert a formatted time string from {start,end}_time into UTC seconds.</td></tr>
<tr><td>timeout</td><td>The number of seconds to keep this search after processing has stopped.</td></tr>
</table>

#### Properties to retrieve

This table summarizes the properties that are available for an existing search job.
<table>
<tr><td>Property</td><td>Description</td></tr>
<tr><td>cursorTime</td><td>The earliest time from which no events are later scanned.</td></tr>
<tr><td>delegate</td><td>For saved searches, specifies jobs that were started by the user.</td></tr>
<tr><td>diskUsage</td><td>The total amount of disk space used, in bytes.</td></tr>
<tr><td>dispatchState</td><td>The state of the search. Can be any of QUEUED, PARSING, RUNNING, PAUSED, FINALIZING, FAILED, DONE.</td></tr>
<tr><td>doneProgress</td><td>A number between 0 and 1.1 that indicates the approximate progress of the search.</td></tr>
<tr><td>dropCount</td><td>For real-time searches, the number of possible events that were dropped due to the "rt_queue_size".</td></tr>
<tr><td>eai:acl</td><td>The access control list for this job.</td></tr>
<tr><td>eventAvailableCount</td><td>The number of events that are available for export.</td></tr>
<tr><td>eventCount</td><td>The number of events returned by the search.</td></tr>
<tr><td>eventFieldCount</td><td>The number of fields found in the search results.</td></tr>
<tr><td>eventIsStreaming</td><td>A Boolean that indicates whether the events of this search are being streamed.</td></tr>
<tr><td>eventIsTruncated</td><td>A Boolean that indicates whether events of the search have not been stored.</td></tr>
<tr><td>eventSearch</td><td>Subset of the entire search before any transforming commands.</td></tr>
<tr><td>eventSorting</td><td>A Boolean that indicates whether the events of this search are sorted, and in which order ("asc" for ascending, "desc" for descending, and "none" for not sorted).</td></tr>
<tr><td>isDone</td><td>A Boolean that indicates whether the search has finished.</td></tr>
<tr><td>isFailed</td><td>A Boolean that indicates whether there was a fatal error executing the search (for example, if the search string syntax was invalid).</td></tr>
<tr><td>isFinalized</td><td>A Boolean that indicates whether the search was finalized (stopped before completion).</td></tr>
<tr><td>isPaused</td><td>A Boolean that indicates whether the search has been paused.</td></tr>
<tr><td>isPreviewEnabled</td><td>A Boolean that indicates whether previews are enabled.</td></tr>
<tr><td>isRealTimeSearch</td><td>A Boolean that indicates whether the search is a real time search.</td></tr>
<tr><td>isRemoteTimeline</td><td>A Boolean that indicates whether the remote timeline feature is enabled.</td></tr>
<tr><td>isSaved</td><td>A Boolean that indicates whether the search is saved indefinitely.</td></tr>
<tr><td>isSavedSearch</td><td>A Boolean that indicates whether this is a saved search run using the scheduler.</td></tr>
<tr><td>isZombie</td><td>A Boolean that indicates whether the process running the search is dead, but with the search not finished.</td></tr>
<tr><td>keywords</td><td>All positive keywords used by this search. A positive keyword is a keyword that is not in a NOT clause.</td></tr>
<tr><td>label</td><td>A custom name created for this search.</td></tr>
<tr><td>messages</td><td>Errors and debug messages.</td></tr>
<tr><td>numPreviews</td><td>Number of previews that have been generated so far for this search job.</td></tr>
<tr><td>performance</td><td>A representation of the execution costs.</td></tr>
<tr><td>priority</td><td>An integer between 0-10 that indicates the search's priority.</td></tr>
<tr><td>remoteSearch</td><td>The search string that is sent to every search peer.</td></tr>
<tr><td>reportSearch</td><td>If reporting commands are used, the reporting search.</td></tr>
<tr><td>request</td><td>GET arguments that the search sends to splunkd.</td></tr>
<tr><td>resultCount</td><td>The total number of results returned by the search, after any transforming commands have been applied (such as stats or top).</td></tr>
<tr><td>resultIsStreaming</td><td>A Boolean that indicates whether the final results of the search are available using streaming (for example, no transforming operations).</td></tr>
<tr><td>resultPreviewCount</td><td>The number of result rows in the latest preview results.</td></tr>
<tr><td>runDuration</td><td>A number specifying the time, in seconds, that the search took to complete.</td></tr>
<tr><td>scanCount</td><td>The number of events that are scanned or read off disk.</td></tr>
<tr><td>searchEarliestTime</td><td>The earliest time for a search, as specified in the search command rather than the "earliestTime" parameter. It does not snap to the indexed data time bounds for all-time searches (as "earliestTime" and "latestTime" do).</td></tr>
<tr><td>searchLatestTime</td><td>The latest time for a search, as specified in the search command rather than the "latestTime" parameter. It does not snap to the indexed data time bounds for all-time searches (as "earliestTime" and "latestTime" do).</td></tr>
<tr><td>searchProviders</td><td>A list of all the search peers that were contacted.</td></tr>
<tr><td>sid</td><td>The search ID number.</td></tr>
<tr><td>ttl</td><td>The time to live, or time before the search job expires after it has finished.</td></tr>
</table>