
About Splunk Stream
Splunk Stream lets you capture, filter, index, and analyze streams of network event data.

A "stream" is a grouping of events defined by a specific network protocol and set of fields. When combined with logs, metrics, and other information, the streams that you capture with Splunk Stream can provide valuable insight into activities and suspicious behavior across your network infrastructure.

Use Splunk Stream to:

Passively capture live streams of network event data
Capture metadata and full packet streams for multiple network protocols
Collect NetFlow protocol data
Apply aggregation methods for statistical analysis of event data
Apply filters to minimize indexer requirements
Extract content from strings and generate hashes
Extract files from network traffic
Monitor network trends and app performance in pre-built dashboards
Deploy independent Stream forwarder to capture data on remote linux machines
Scale rapidly and unobtrusively with no need for tagging or instrumentation
For a detailed overview of the Splunk Stream components, see the Splunk Stream installation package overview.

To get started using Splunk Stream to capture network metadata and full network packets, see Configure Streams in the Splunk Stream User Manual.

Splunk Stream components
To deploy Splunk Stream, install three Stream packages on your existing Splunk Enterprise instances and/or compatible Linux machines.

Splunk App for Stream, packaged as splunk_app_stream
Splunk Add-on for Stream Forwarders, packaged as Splunk_TA_stream
Splunk Add-on for Stream Wire Data, packaged as Splunk_TA_stream_wire_data
Splunk Stream also provides the ability to deploy Independent Stream forwarders. This is packaged as a binary file <streamfwd> within the Splunk App for Stream installation.

For more about Splunk Stream components, see Splunk Stream installation package overview in this manual.

Supported Splunk Software configurations
Splunk Stream supports most Splunk Software configurations:

Splunk Cloud deployments.
Distributed deployment configurations, including deployment servers and indexer clusters
Single instance deployments, where a single instance of Splunk Enteprise is both the indexer and the search head
Independent Stream Forwarders on compatible Linux machines.


Splunk Stream installation package overview
As of Splunk Stream version 7.3, Splunk Stream is organized as three packages that must each be downloaded and installed.

Product name	Installation package name	Installed file name	Description
Splunk App for Stream	splunk_app_stream	splunk_app_stream/	When you install this package on your search heads, it provides:
the configuration user interface
container dashboards and dashboards for analysis of network events and flow data
filters for fine-tuning data capture
Splunk Add-on for Stream Forwarders	Splunk_TA_stream	Splunk_TA_stream/	You install this package on your Splunk forwarders and use it to extend the universal forwarders. You deploy it to search heads and indexers to collect local traffic. When you install a Stream forwarder on the same server as the Splunk App for Stream, you can upload PCAP data from the User Interface.
Splunk Add-on for Stream Wire Data	Splunk_TA_stream_wire_data	Splunk_TA_stream_wire_data/	When you install this package on your search heads it provides all the knowledge objects required at search and indexing time. In a distributed deployment the package can be deployed on search heads and indexers, and if present in your configuration, heavy forwarders.