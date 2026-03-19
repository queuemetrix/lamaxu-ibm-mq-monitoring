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

<br>

---

# Lamaxu MQ MCP Server

An [MCP (Model Context Protocol)](https://modelcontextprotocol.io) server that exposes IBM MQ queue manager data from a running Lamaxu agent to AI assistants such as Claude.

## Setup

### 1. Build the server

```bash
cd mcp-server
npm install
npm run build
```

This compiles the TypeScript source to `mcp-server/dist/index.js`.

### 2. Configure Claude Code

The project includes a `.mcp.json` file at the repository root that Claude Code picks up automatically:

```json
{
  "mcpServers": {
    "lamaxu": {
      "command": "node",
      "args": ["C:/Users/mattb/GitHub/lamaxu/mcp-server/dist/index.js"],
      "env": {
        "LAMAXU_URL": "http://localhost:8085",
        "LAMAXU_USERNAME": "admin",
        "LAMAXU_PASSWORD": "password"
      }
    }
  }
}
```

Update `LAMAXU_URL`, `LAMAXU_USERNAME`, and `LAMAXU_PASSWORD` to match your environment before use.

### Environment variables

| Variable | Default | Description |
|---|---|---|
| `LAMAXU_URL` | `http://localhost:8085` | Base URL of the Lamaxu HTTP gateway |
| `LAMAXU_USERNAME` | _(none)_ | Basic auth username (omit if auth is disabled) |
| `LAMAXU_PASSWORD` | _(none)_ | Basic auth password (omit if auth is disabled) |

---

## Available Tools

### `get_agent_status`

Returns the Lamaxu agent configuration: all configured queue managers, gateway settings, and cache refresh interval.

**Parameters:** none

**Example prompt:**
> What queue managers is Lamaxu monitoring?

---

### `get_queue_status`

Returns queue status and metrics for a queue manager — current depth, message age, uncommitted messages, open input/output process counts, etc.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name (e.g. `DEMO`) |
| `queueName` | No | Filter to a specific queue name. Omit to return all queues. |

**Example prompts:**
> Show me all queues on DEMO with messages in them.
> What is the current depth of DEV.QUEUE.1 on QM1?

---

### `get_channel_status`

Returns channel status and metrics — channel type, connection name, status, message count, remote application tag, etc.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |
| `channelName` | No | Filter to a specific channel name. Omit to return all channels. |

**Example prompts:**
> Are there any stopped or retrying channels on QM1?
> Show the status of the TO.QM2 channel.

---

### `get_qmgr_status`

Returns queue manager status and attributes — overall status, connection count, channel initiator status, standby status, installation details.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |

**Example prompt:**
> Is DEMO running and how many connections does it have?

---

### `get_listener_status`

Returns listener status — transport type, IP address, port, and current status.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |

**Example prompt:**
> Is the TCP listener on QM1 running?

---

### `get_topic_status`

Returns topic status and attributes — topic string, type, publisher count, subscriber count.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |
| `topicName` | No | Filter to a specific topic name. Omit to return all topics. |

**Example prompt:**
> How many subscribers are on the ORDERS topic on DEMO?

---

### `get_subscription_status`

Returns subscription status — subscription name, type, topic string, depth, message age, durable flag.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |

**Example prompt:**
> List all durable subscriptions on QM1.

---

### `get_statistics`

Returns statistical data collected by Lamaxu. Can be filtered by object type and/or object name.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |
| `objectType` | No | Filter by type: `QUEUE_STATUS`, `CHANNEL_STATUS`, `STATISTICS`, etc. |
| `objectName` | No | Filter by a specific object name |

**Example prompts:**
> Show me queue statistics for DEV.DEAD.LETTER.QUEUE on DEMO.
> Get all channel statistics for QM1.

---

### `browse_queue`

Browses messages currently sitting on a queue **without removing them**. Returns message metadata (ID, correlID, length, put timestamp) and message content.

**Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `broker` | Yes | Queue manager name |
| `queueName` | Yes | Name of the queue to browse |

**Example prompts:**
> Browse the messages on DEV.DEAD.LETTER.QUEUE on DEMO.
> What messages are sitting on the ERROR.QUEUE queue?

---

## Data freshness

All data is served from the Lamaxu agent's in-memory cache, which is refreshed on a configurable interval (default: 60 seconds). Tools do not trigger a live PCF query — they reflect the state at the last cache sweep.

---

## Development

To run the server directly without building:

```bash
cd mcp-server
npm run dev
```

This uses `tsx` to execute the TypeScript source directly.
