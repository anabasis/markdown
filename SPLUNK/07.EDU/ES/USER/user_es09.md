# Using Splunk Enterprise Security

## 9. Protocol Intelligence

### Objectives

- Describe how stream data is input into Splunk events
- List the types of stream events
- Use ES protocol intelligence to analyze captured stream data

### Security Intelligence: Protocol Intelligence

Protocol intelligence is ES’s set of tools for analyzing network traffic

|||
|:--|:--|
|Protocol Center|An overview dashboard showing protocol activity across the network|
|Traffic Size|An analytical dashboard showing network traffic rates and trends|
|DNS|A pair of dashboards showing both an overview of activity of DNS queries as well as a search interface|
|SSL|A pair of dashboards for analyzing SSL certificate activity|
|Email|A pair of dashboards for analyzing email activity|

### Protocol Intelligence Use Cases

- Captures network traffic directly without 3rd party vendor software or log data
- Use it to:
  - Monitor suspicious network traffic
  - Correlate logged vs. actual activity
  - Gain direct access to network traffic for SSL, HTTP, DNS, and SMTP activity
  - Configure correlation searches that can monitor network traffic

### Splunk Stream

- Traffic is captured using Splunk Stream <http://docs.splunk.com/Documentation/StreamApp>
- The Stream add-on is deployed on forwarders and listens to traffic
- Traffic data is forwarded to indexers and made available to ES
- Additional captures can be set up within ES

### Stream Data Flow

- Splunk ES With Stream app Execute and display search results
- Indexers Store captured stream data
- Captured data does not include message content unless specifically configured
- Production Servers with forwarders and Stream add-on
- Capture network data and forward to indexers

### Stream Events

- Stream events are stored with stream:xxxx source types
  - Examples: tcp, udp, dns, smtp, http
- Standard field extractions:
  - Capture time, type, size, source/dest info
- Depending on specific source type, additional fields are extracted
  - HTTP: cookies, request parameters, etc.
  - SMTP: sender, receiver, subject, summary of body
  - DNS: DNS query, query type, DNS host, etc.

### Protocol Intelligence > Protocol Center

### Protocol Intelligence > Traffic Size Analysis

### Protocol Intelligence > DNS Activity

### Protocol Intelligence > SSL Activity

### Protocol Intelligence > Email Activity

### Creating a Stream Capture

- Investigating a notable or source event?
  - You can create a temporary stream capture for the source or destination server
  - Then investigate the stream data for that server
- You can also Stream capture
  - From a correlation search
  - By an adaptive response action

### Scenarios: Data Exfiltration

- Detect data exfiltration using protocol intelligence dashboards:
  - Email Activity? Examine Top Email Sources
    - Look for sudden spikes in email output from single accounts or
    - Spikes in the Large Emails display
  - DNS Activity? Examine Queries per Domain
    - Look for unfamiliar domains getting large numbers of lookups
- See an endpoint / server that may be involved in data exfiltration?
1. Create a stream capture for it and analyze the data
2. Look for sensitive information, intellectual property, etc.

### Lab Exercise 9: Protocol Intelligence

- Time: 20 minutes
- Task:
- Use Protocol Intelligence and related tools to investigate a suspected data exfiltration event
