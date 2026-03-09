# lamaxu-ibm-mq-monitoring
Lamaxu is an IBM MQ Monitoring tool that that exposes metrics in JMX format

For more information, please get in touch, <a href="https://www.queuemetrix.com" />www.queuemetrix.com</a>

## LAMAXU - Pronounced LAMASSU 
(LAMASSU is an ancient Assyrian protective deity)

Lamaxu is a unique IBM MQ monitoring ‘Agentless’ agent, and Web Service API, that allows organizations to build comprehensive monitoring solutions for MQ that meet their own unique business requirements. By providing an open API to build upon, Lamaxu enables you to focus on what to monitor and measure, rather than how.

Lamaxu simplifies the process of gaining insightful IBM MQ metrics from complex distributed MQ systems. The Lamaxu REST API exposes almost all of IBM MQ’s internal workings, and metrics (e.g. Events, Statistics, Statuses, configuration etc). Lamaxu enables the centralised collection of every MQ metric data point that is meaningful to your business.

Lamaxu’s API provides easy access to MQ events, statistics, status information, configuration plus a number of unique, aggregated performance metrics such as messages rates and queue backlog times. The REST API provides a key/value response, available in either JSON or XML format that can be leveraged in many ways, such as to build your own customised web dashboards.

The Lamaxu process is designed to be executed as a service, and can be run on the same, or remote server as the queue managers from which it’s collecting data. Object configuration, status, event, accounting and statistics data is collected at configurable intervals, defined in the config.xml file, and persisted to an in-memory data cache.

<a href="https://www.queuemetrix.com/docuflow/doc/media/2025/11/12255300-690afc4a948b3.png"><img src="https://www.queuemetrix.com/docuflow/docuflow/doc/media/2025/11/12255300-690afc4a948b3.png" alt="12255300.png" width="800" /></a>

## Typical use cases are:

1. To access queue manager metrics which are otherwise inaccessible due to partially implemented support for monitoring IBM MQ in major monitoring platforms. Some examples include message age and depth of a subscription.
2. The ability to collect all data including events allows for the opportunity to draw out unique information by automatically joining multiple sources of data. Some examples include real time message volume statistics for subscriptions.
3. Centralising event and accounting / statistics data and applying a lifecycle policy based on age of data allows for the implementation of proper system capacity planning driven by real data. Out of the box, this data is merely available, extracting real uses from it in a self-managing way is where the value is.

