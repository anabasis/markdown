# How to display search results using the Splunk SDK for Python

After you run a search, you can retrieve different output from the search job:

- Events : The untransformed events of the search. (There may not be any events if you have a transforming search and you did not specify any status buckets.)
- Results : The transformed results of the search after processing has been completed. If the search does not have transforming commands, the results are the same as the events. The result count will be less than the event count if there are transforming commands.
- Results preview : A preview of a search that is still in progress, or results from a real-time search. When the search is complete, the preview results are the same as the results. You must enable previews for non-real-time searches (previews are enabled automatically for real-time searches).
- Summary : Summary information about the fields of a search from the results that have been read thus far. Set "status_buckets" on the search job to a positive value to access this data.
- Timeline : The event distribution over time of the untransformed events that have been read thus far. Set "status_buckets" on the search job to a positive value to access this data.

This output is returned as a stream in XML, JSON, JSON_COLS, JSON_ROWS, CSV, ATOM, or RAW format. For examples, see [Sample output in different formats](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#output) below.

You can display the direct results without a parser or make your own parser. For convenience, the SDK includes a results reader for XML that properly parses and formats the results for you. For a comparison of XML output displayed without a reader versus the SDK's XML results reader, see [Results reader comparison](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#outputcompare), below.

## The search results APIs

Retrieve a search [job](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job) using the Job class. From the search job, you can retrieve events, results, preview results, the summary, and timeline information:

- The [Job.events](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.events) method returns a streaming handle to the job's events. You can specify additional parameters to the method; see [GET search/jobs/{search_id}/events](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#GET_search.2Fjobs.2F.7Bsearch_id.7D.2Fevents) in the REST API documentation for a list of valid parameters.
- The [Job.results](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.results) method returns a streaming handle to the job's search results. You can specify additional parameters to the method; see [GET search/jobs/{search_id}/results](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#GET_search.2Fjobs.2F.7Bsearch_id.7D.2Fresults) in the REST API documentation for a list of valid parameters.
- The [Job.preview](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.preview) method returns a streaming handle to the job's preview search results. You can specify additional parameters to the method; see [GET search/jobs/{search_id}/results_preview](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#GET_search.2Fjobs.2F.7Bsearch_id.7D.2Fresults_preview) in the REST API documentation for a list of valid parameters.
- The [Job.summary](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.summary) method returns a streaming handle to the job's summary. You can specify additional parameters to the method; see [GET search/jobs/{search_id}/summary](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#GET_search.2Fjobs.2F.7Bsearch_id.7D.2Fsummary) in the REST API documentation for a list of valid parameters.
- The [Job.timeline](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.timeline) method returns a streaming handle to the job's timeline results. You can specify additional parameters to the method; see [GET search/jobs/{search_id}/timeline](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#GET_search.2Fjobs.2F.7Bsearch_id.7D.2Ftimeline) in the REST API documentation for a list of valid parameters.

Splunk search results can be returned in a variety of formats including XML, JSON, and CSV. To make it easier to stream search results in XML format, they are returned as a stream of XML fragments, not as a single XML document. The results module supports incrementally reading one result record at a time from such a results stream. This module also provides a friendly iterator-based interface for accessing search results while avoiding buffering the result set, which can be very large.

To use the reader, instantiate [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) on a search result stream as follows:

```python
reader = ResultsReader(result_stream)
for item in reader:
    print(item)
print "Results are a preview: %s" % reader.is_preview
```

The [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) class returns dictionaries and Splunk messages from an XML results stream. If its field, is_preview, is set to True, the results are a preview from a running search; otherwise the results are from a completed search. The [Message](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.Message) class represents the informational messages that Splunk interleaves in the results stream. A Message object is comprised of two arguments--a string that gives the message (for instance, "DEBUG"), and a string that gives the message itself.

## Code examples

This section provides examples of how to use the job APIs, assuming you first [connect to a Splunk instance](http://dev.splunk.com/view/python-sdk/SP-CAAAECX):

- [To display results without a reader](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#basic)
- [To display results using a results reader](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#reader)
- [To paginate through a large set of results](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#paginating)
- [To display preview results](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#preview)
- [To work with results from an export search](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#export)

The following parameters are available:

- [Event, results, and results preview parameters](http://dev.splunk.com/view/python-sdk/SP-CAAAER5#resultsargsparams)

### To display results without a reader

You could just read the results as a stream of XML with no parsing, but you would need to create an XML parser for the data from scratch:

```python
# A blocking search
job = jobs.create("search * | head 100", **{"exec_mode": "blocking"})
print "...done!\n"

print "Search results:\n"

# Prints raw XML stream to the console
result_stream = job.results()
print result_stream.read()
```

If you don't want to create your own parser, a better method is described in the next section.

### To display results using a results reader

The Splunk SDK for Python contains its own results reader that parses and formats results for you. Use the [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) class for XML, which is the default format.

```python
# A blocking search
job = jobs.create("search * | head 100", **{"exec_mode": "blocking"})
print "...done!\n"

print "Search results:\n"

# Prints a parsed, formatted XML stream to the console
result_stream = job.results()
reader = results.ResultsReader(result_stream)
for item in reader:
    print(item)
print "Results are a preview: %s" % reader.is_preview
```

### To paginate through a large set of results

The maximum number of results you can retrieve at a time from your search results is determined by the maxresultrows field, which is specified in a Splunk configuration file. We don't recommend changing the default value of 50,000. If your job has more results than this limit, just retrieve your results in sets (0-49999, then 50000-99999, and so on), using the "count" and "offset" parameters to define how many results to retrieve at a time. Set the count (the number of results in a set) to maxresultrows (or a smaller value), and increment the offset by this same value to page through each set.
The following example shows how to determine the maximum number of results your system is configured to return:

```python
# Find out how many results your system is configured to return
maxresultrows = service.confs["limits"]["restapi"]["maxresultrows"]
print "Your system is configured to return a maximum of %s results" % maxresultrows
```

The following example continues the previous "blocking" search example and shows how to retrieve search results in sets, using a count of 10 for this example, and then displays the results in XML using the ResultsReader class:

```python
import splunklib.results as results

...

# A blocking search
job = jobs.create("search * | head 100", **{"exec_mode": "blocking"})
print "...done!\n"

# Page through results by looping through sets of 10 at a time
print "Search results:\n"
resultCount = job["resultCount"]  # Number of results this job returned
offset = 0;                       # Start at result 0
count = 10;                       # Get sets of 10 results at a time

while (offset < int(resultCount)):
    kwargs_paginate = {"count": count,
                       "offset": offset}

    # Get the search results and display them
    blocksearch_results = job.results(**kwargs_paginate)

    for result in results.ResultsReader(blocksearch_results):
        print result

    # Increase the offset to get the next set of results
    offset += count
```

### To display preview results

You can display preview results—a preview of the results of an in-progress search—by using the [Job.preview](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.preview) method. Job.preview returns, in a raw form, any results that have been generated so far from the server. You can then use a [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) to read the results, converting them into objects over which you can iterate.

For instance, the following code creates a new job with preview results enabled:

```python
import splunklib.client as client
import splunklib.results as results

service = client.connect(...)

job = service.jobs.create("search * | head 2", preview=True)
```

To determine whether preview has been successfully enabled, check for the following:

```python
job['isPreviewEnabled'] == '1'
```

To access the preview results, pass the handle returned by `Job.preview` to a new `ResultsReader`, and then iterate through the reader:

```python
rr = results.ResultsReader(job.preview())
for result in rr:
    if isinstance(result, results.Message):
        # Diagnostic messages may be returned in the results
        print '%s: %s' % (result.type, result.message)
    elif isinstance(result, dict):
        # Normal events are returned as dicts
        print result
if rr.is_preview:
    print "Preview of a running search job."
else:
    print "Job is finished. Results are final."
```

You can also enable preview on an existing job by calling [Job.enable_preview](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.enable_preview). Because enable_preview can take some time to propagate in the system, you might want to wait for it using something like the following:

```python
job.enable_preview()
while job['isPreviewEnabled'] == 0:
    sleep(0.2)
```

### To work with results from an export search

Working with search results from export searches is a little different than that of regular searches:

- A reporting (transforming) search returns a set of previews followed by the final events, each as separate elements.
- A non-reporting (non-transforming) search returns events as they are read from the index, each as separate elements.
- A real-time search returns multiple sets of previews, each preview as a separate element.
- For JSON output, each result set is not returned as a single JSON object, but rather each row is an individual object, where rows are separated by a new line and the last row of the set is indicated by "lastrow":true.

Here's sample JSON output that shows two results sets, each with five rows:

```json
{"preview":true,"offset":0,"result":{"sourcetype":"eventgen-2","count":"58509"}}
{"preview":true,"offset":1,"result":{"sourcetype":"splunk_web_service","count":"119"}}
{"preview":true,"offset":2,"result":{"sourcetype":"splunkd","count":"4153"}}
{"preview":true,"offset":3,"result":{"sourcetype":"splunkd_access","count":"12"}}
{"preview":true,"offset":4,"lastrow":true,"result":{"sourcetype":"splunkd_stderr","count":"2"}}
{"preview":true,"offset":0,"result":{"sourcetype":"eventgen-2","count":"60886"}}
{"preview":true,"offset":1,"result":{"sourcetype":"splunk_web_service","count":"119"}}
{"preview":true,"offset":2,"result":{"sourcetype":"splunkd","count":"4280"}}
{"preview":true,"offset":3,"result":{"sourcetype":"splunkd_access","count":"12"}}
{"preview":true,"offset":4,"lastrow":true,"result":{"sourcetype":"splunkd_stderr","count":"2"}}
```

This format allows results to be sent as a continuous stream of JSON data that is still easy to parse.

> Note: The Splunk SDK for Python does not include a JSON parser. To parse a JSON data stream, you will need to create your own parser.

Splunk recommends using the SDK's XML results reader to parse the output--we've already done some of the heavy lifting here, and the results reader handles the output appropriately. Return the results stream in XML (the default format), and use the [ResultsReader](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/results.html#splunklib.results.ResultsReader) class to parse and format the results.

## Sample output in different formats

The following is sample output in different formats for the search `"search index=_internal | head 1"`:

```xml
***** ATOM *****

<?xml version="1.0" encoding="UTF-8"?>
<!--This is to override browser formatting; see server.conf[httpServer] to disable. . . . . .-->
<?xml-stylesheet type="text/xml" href="/static/atom.xsl"?>
<feed xmlns="http://www.w3.org/2005/Atom" xmlns:s="http://dev.splunk.com/ns/rest">
  <title>Search Results</title>
  <id>/services/search/jobs/1378940275.16</id>
  <updated>2013-09-11T15:57:55-07:00</updated>
  <generator build="172409" version="5.0.4"/>
  <author>
    <name>Splunk</name>
  </author>
  <entry>
    <title>Result Offset 0</title>
    <id>/services/search/jobs/1378940275.16/results?output_mode=atom&amp;count=1&amp;offset=0</id>
    <updated>2013-09-11T15:57:55-07:00</updated>
    <link href="/services/search/jobs/1378940275.16/results?output_mode=atom&amp;count=1&amp;offset=0" rel="alternate"/>
    <content type="text/xml">
      <s:dict>
        <s:key id="_bkt">_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2</s:key>
        <s:key id="_cd">26:306023</s:key>
        <s:key id="_indextime">1378940272</s:key>
        <s:key id="_raw">09-11-2013 15:57:52.280 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0</s:key>
        <s:key id="_serial">0</s:key>
        <s:key id="_si">TESTBOX-mbp15.local
        _internal</s:key>
        <s:key id="_sourcetype">splunkd</s:key>
        <s:key id="_subsecond">.280</s:key>
        <s:key id="_time">2013-09-11T15:57:52.280-07:00</s:key>
        <s:key id="host">TESTBOX-mbp15.local</s:key>
        <s:key id="index">_internal</s:key>
        <s:key id="linecount">1</s:key>
        <s:key id="source">/Applications/splunk/var/log/splunk/metrics.log</s:key>
        <s:key id="sourcetype">splunkd</s:key>
        <s:key id="splunk_server">TESTBOX-mbp15.local</s:key>
      </s:dict>
    </content>
  </entry>
</feed>

***** CSV *****

"_bkt","_cd","_indextime","_raw","_serial","_si","_sourcetype","_subsecond","_time",host,index,linecount,source,sourcetype,"splunk_server"
"_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2","26:310460",1378940614,"09-11-2013 16:03:33.463 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0",0,"TESTBOX-mbp15.local
_internal",splunkd,".463","2013-09-11T16:03:33.463-07:00","TESTBOX-mbp15.local","_internal",1,"/Applications/splunk/var/log/splunk/metrics.log",splunkd,"TESTBOX-mbp15.local"

***** JSON *****

{"preview":false,"init_offset":0,"messages":[{"type":"DEBUG","text":"base lispy: [ AND index::_internal ]"},{"type":"DEBUG","text":"search context: user=\"admin\", app=\"search\", bs-pathname=\"/Applications/splunk/etc\""}],"results":[{"_bkt":"_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2","_cd":"26:311977","_indextime":"1378940738","_raw":"09-11-2013 16:05:37.503 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0","_serial":"0","_si":"TESTBOX-mbp15.local\n_internal","_sourcetype":"splunkd","_subsecond":".503","_time":"2013-09-11T16:05:37.503-07:00","host":"TESTBOX-mbp15.local","index":"_internal","linecount":"1","source":"/Applications/splunk/var/log/splunk/metrics.log","sourcetype":"splunkd","splunk_server":"TESTBOX-mbp15.local"}]}

***** JSON_COLS *****

{"preview":false,"init_offset":0,"messages":[{"type":"DEBUG","text":"base lispy: [ AND index::_internal ]"},{"type":"DEBUG","text":"search context: user=\"admin\", app=\"search\", bs-pathname=\"/Applications/splunk/etc\""}],"fields":["_bkt","_cd","_indextime","_raw","_serial","_si","_sourcetype","_subsecond","_time","host","index","linecount","source","sourcetype","splunk_server"],"columns":[["_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2"],["26:312397"],["1378940769"],["09-11-2013 16:06:08.538 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0"],["0"],[["TESTBOX-mbp15.local","_internal"]],["splunkd"],[".538"],["2013-09-11T16:06:08.538-07:00"],["TESTBOX-mbp15.local"],["_internal"],["1"],["/Applications/splunk/var/log/splunk/metrics.log"],["splunkd"],["TESTBOX-mbp15.local"]]}

***** JSON_ROWS *****

{"preview":false,"init_offset":0,"messages":[{"type":"DEBUG","text":"base lispy: [ AND index::_internal ]"},{"type":"DEBUG","text":"search context: user=\"admin\", app=\"search\", bs-pathname=\"/Applications/splunk/etc\""}],"fields":["_bkt","_cd","_indextime","_raw","_serial","_si","_sourcetype","_subsecond","_time","host","index","linecount","source","sourcetype","splunk_server"],"rows":[["_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2","26:312817","1378940800","09-11-2013 16:06:39.575 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0","0",["TESTBOX-mbp15.local","_internal"],"splunkd",".575","2013-09-11T16:06:39.575-07:00","TESTBOX-mbp15.local","_internal","1","/Applications/splunk/var/log/splunk/metrics.log","splunkd","TESTBOX-mbp15.local"]]}

***** RAW *****

09-11-2013 16:08:12.529 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0

***** XML *****

<?xml version='1.0' encoding='UTF-8'?>
<results preview='0'>
<meta>
<fieldOrder>
<field>_bkt</field>
<field>_cd</field>
<field>_indextime</field>
<field>_raw</field>
<field>_serial</field>
<field>_si</field>
<field>_sourcetype</field>
<field>_subsecond</field>
<field>_time</field>
<field>host</field>
<field>index</field>
<field>linecount</field>
<field>source</field>
<field>sourcetype</field>
<field>splunk_server</field>
</fieldOrder>
</meta>
    <result offset='0'>
        <field k='_bkt'>
            <value><text>_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2</text></value>
        </field>
        <field k='_cd'>
            <value><text>26:314447</text></value>
        </field>
        <field k='_indextime'>
            <value><text>1378940924</text></value>
        </field>
        <field k='_raw'><v xml:space='preserve' trunc='0'>09-11-2013 16:08:43.567 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0</v></field>
        <field k='_serial'>
            <value><text>0</text></value>
        </field>
        <field k='_si'>
            <value><text>TESTBOX-mbp15.local</text></value>
            <value><text>_internal</text></value>
        </field>
        <field k='_sourcetype'>
            <value><text>splunkd</text></value>
        </field>
        <field k='_subsecond'>
            <value><text>.567</text></value>
        </field>
        <field k='_time'>
            <value><text>2013-09-11T16:08:43.567-07:00</text></value>
        </field>
        <field k='host'>
            <value><text>TESTBOX-mbp15.local</text></value>
        </field>
        <field k='index'>
            <value><text>_internal</text></value>
        </field>
        <field k='linecount'>
            <value><text>1</text></value>
        </field>
        <field k='source'>
            <value><text>/Applications/splunk/var/log/splunk/metrics.log</text></value>
        </field>
        <field k='sourcetype'>
            <value><text>splunkd</text></value>
        </field>
        <field k='splunk_server'>
            <value><text>TESTBOX-mbp15.local</text></value>
        </field>
    </result>
</results>
```

## Results reader comparison

Here's a single search result. For comparison, the output is displayed in its raw output form, and then SDK's XML results reader:

```xml
***** Output from printing raw results *****

<?xml version='1.0' encoding='UTF-8'?>
<results preview='0'>
<meta>
<fieldOrder>
<field>_bkt</field>
<field>_cd</field>
<field>_indextime</field>
<field>_raw</field>
<field>_serial</field>
<field>_si</field>
<field>_sourcetype</field>
<field>_subsecond</field>
<field>_time</field>
<field>host</field>
<field>index</field>
<field>linecount</field>
<field>source</field>
<field>sourcetype</field>
<field>splunk_server</field>
</fieldOrder>
</meta>
    <result offset='0'>
        <field k='_bkt'>
            <value><text>_internal~26~6C4FD68A-A664-44BA-A4B1-22759F85DBE2</text></value>
        </field>
        <field k='_cd'>
            <value><text>26:314447</text></value>
        </field>
        <field k='_indextime'>
            <value><text>1378940924</text></value>
        </field>
        <field k='_raw'><v xml:space='preserve' trunc='0'>09-11-2013 16:08:43.567 -0700 INFO Metrics - group=tpool, id=indexertpool, qsize=0, workers=6, qwork_units=0</v></field>
        <field k='_serial'>
            <value><text>0</text></value>
        </field>
        <field k='_si'>
            <value><text>TESTBOX-mbp15.local</text></value>
            <value><text>_internal</text></value>
        </field>
        <field k='_sourcetype'>
            <value><text>splunkd</text></value>
        </field>
        <field k='_subsecond'>
            <value><text>.567</text></value>
        </field>
        <field k='_time'>
            <value><text>2013-09-11T16:08:43.567-07:00</text></value>
        </field>
        <field k='host'>
            <value><text>TESTBOX-mbp15.local</text></value>
        </field>
        <field k='index'>
            <value><text>_internal</text></value>
        </field>
        <field k='linecount'>
            <value><text>1</text></value>
        </field>
        <field k='source'>
            <value><text>/Applications/splunk/var/log/splunk/metrics.log</text></value>
        </field>
        <field k='sourcetype'>
            <value><text>splunkd</text></value>
        </field>
        <field k='splunk_server'>
            <value><text>TESTBOX-mbp15.local</text></value>
        </field>
    </result>
</results>

***** Output from the XML results reader *****

   _bkt -> main~6~6C4FD68A-A664-44BA-A4B1-22759F85DBE2
   _cd -> 6:676
   _indextime -> 1374277531
   _raw -> 201.239.213.240 - - [19/Jul/2013:16:18:30] "GET /category.screen?categoryId=BLUE_GIZMOS&JSESSIONID=CA8MO3AZ2USANA34878 HTTP 1.1" 200 1769 "http://www.bing.com" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)" 554
   _serial -> 4
   _si -> TESTBOX-mbp15.local,main
   _sourcetype -> access_combined_wcookie
   _time -> 2013-07-19T16:18:30.000-07:00
   host -> BigDBBook-www1
   index -> main
   linecount -> 1
   source -> /opt/log/BigDBBook-www1/access.log
   sourcetype -> access_combined_wcookie
   splunk_server -> TESTBOX-mbp15.local
```

## Event, results, and results preview parameters

Set these parameters when calling the following methods:

- [Job.events](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.events)
- [Job.results](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.results)
- [Job.preview](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/client.html#splunklib.client.Job.preview)

For more, see the [POST search/jobs](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTsearch#POST_search.2Fjobs) endpoint.

<table>
<tr><td>Parameter</td><td>Description</td><td>Applies to</td></tr>
<tr><td>count</td><td>A number that indicates the maximum number of results to return.</td><td>events, results, results preview</td></tr>
<tr><td>earliest_time</td><td>A time string that specifies the earliest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string. For a real-time search, specify "rt".</td><td>events</td></tr>
<tr><td>f</td><td>A string that contains the field to return for the event set.</td><td>events, results, results preview</td></tr>
<tr><td>field_list</td><td>A string that contains a comma-separated list of fields to return for the event set.</td><td>events, results, results preview</td></tr>
<tr><td>latest_time</td><td>A time string that specifies the earliest time in the time range to search. The time string can be a UTC time (with fractional seconds), a relative time specifier (to now), or a formatted time string. For a real-time search, specify "rt".</td><td>events</td></tr>
<tr><td>max_lines</td><td>The maximum number of lines that any single event's "_raw" field should contain.</td><td>events</td></tr>
<tr><td>offset</td><td>A number of the index of the first result (inclusive) from which to begin returning data. This value is 0-indexed.</td><td>events, results, results preview</td></tr>
<tr><td>output_mode</td><td>Specifies the output format of the results (XML, JSON, JSON_COLS, JSON_ROWS, CSV, ATOM, or RAW).</td><td>events, results, results preview</td></tr>
<tr><td>output_time_format</td><td>A string that contains a UTC time format.</td><td>events</td></tr>
<tr><td>search</td><td>A string that contains the post-processing search to apply to results.</td><td>events, results, results preview</td></tr>
<tr><td>segmentation</td><td>A string that contains the type of segmentation to perform on the data.</td><td>events</td></tr>
<tr><td>time_format</td><td>A string that contains the expression to convert a formatted time string from {start,end}_time into UTC seconds.</td><td>events</td></tr>
<tr><td>truncation_mode</td><td>A string that specifies how "max_lines" should be achieved ("abstract" or "truncate").</td><td>events</td></tr>
</table>