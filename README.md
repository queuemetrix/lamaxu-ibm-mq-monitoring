# lamaxu-ibm-mq-monitoring

# README
[Web Site](https://www.queuemetrix.com)<br>
[Documentation](https://www.queuemetrix.net.au/confluence/)<br>
[Docker Hub](https://hub.docker.com/u/queuemetrix)

## Description
Lamaxu is a Java agent that remotely connects to IBM MQ and exposes all its available metrics in an easily consumed format, allowing it to be monitored by virtually any enterprise monitoring system.

## Licensing
The image is bundled with one(1) license to monitor one(1) queue manager.<br> 
Please register for a trial you require additional queue manager licenses, [Register for Trial License Now](https://www.queuemetrix.com/license_portal/trial-portal.html).

## Consumable data formats supported are;
- JMX Mbeans (Solarwinds, AppDynamics other)
- HTTP REST Web Service (with formats of both XML and JSON)
- Log file (XML or JSON formatted logs for consumption by SPLUNK)<br>

## Example Docker Build Command:
>*docker build --build-arg LMX_VERSION=1.0.8.1 -t queuemetrix/lamaxu-arm64:latest .*

## Example Docker Run Command:
>*docker run --name lamaxu00 --env JMX_IP=192.168.0.10 -p 8085:8085 -p 8443:8443 -p 3098:3098 -p 3099:3099 --detach queuemetrix/lamaxu-arm64:latest*<br>

>**IMPORTANT** <br>Please ensure environment the variable ***JMX_IP={public IP of container}*** is set. <br>This needs to be the public IP and not the internal container IP or remote JMX will not work.

## Example JMX URI
>*service:jmx:rmi://localhost:3098/jndi/rmi://localhost:3099/jmxrmi*

## Accessing the Web UI
>*http://localhost:8085/admin/dashboard/dist/#/mq/admin*

> Username: admin<br>
> Password: password
