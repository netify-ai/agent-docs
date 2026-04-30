# MQTT Sink Plugin

## MQTT Sink: Overview
The Netify MQTT Plugin enables the publishing of real-time [network telemetry](https://www.netify.ai/products/netify-dpi/telemetry) and metadata from the DPI engine to a message queue. Using the message queue's publish/subscribe (Pub/Sub) architecture, the plugin efficiently distributes telemetry streams to multiple subscribers without requiring persistent, resource-intensive point-to-point connections.

## MQTT Sink: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-sink-mqtt.json`).  You can use the built-in ${channel} variable to route different telemetry streams to separate topics. You can also append the Netify Agent’s UUID or serial number to ensure each instance publishes to a unique path.

## MQTT Sink: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-sink-mqtt.json",
  "title": "Netify Sink MQTT Configuration",
  "description": "JSON Schema for the Netify Sink MQTT Plugin configuration.",
  "type": "object",
  "required": ["host", "user", "pass", "topic"],
  "properties": {
    "host": {
      "type": "string",
      "format": "hostname",
      "description": "MQTT server hostname or IP address."
    },
    "port": {
      "type": "integer",
      "minimum": 1,
      "maximum": 65535,
      "default": 1883,
      "description": "Port used to connect to the MQTT server (typically 1883 for TCP or 8883 for SSL)."
    },
    "user": {
      "type": "string",
      "description": "Username used to authenticate with the MQTT server."
    },
    "pass": {
      "type": "string",
      "writeOnly": true,
      "description": "Password used to authenticate with the MQTT server."
    },
    "topic": {
      "type": "string",
      "description": "Topic to publish messages to. Supports template variables.",
      "pattern": "^[a-zA-Z0-9/\\._\\-${}]+$",
      "examples": ["netify/${uuid_agent}/${channel}"]
    },
    "message_qos": {
      "type": "integer",
      "enum": [0, 1, 2],
      "default": 1,
      "description": "QoS level: 0 (At most once), 1 (At least once), 2 (Exactly once)."
    },
    "message_retain": {
      "type": "boolean",
      "default": false,
      "description": "When true, the broker retains the last message on the topic for new subscribers."
    }
  }
}

```

