# Socket Sink Plugin

## Socket Sink: Overview
The Netify Sockets interface enables the direct streaming of real-time [network telemetry](https://www.netify.ai/products/netify-dpi/telemetry) and metadata from the DPI engine to client applications. It supports TCP sockets, UDP sockets, WebSockets, and file sockets, providing flexible options for both persistent, low-latency connections and local inter-process communication. This approach is ideal for custom integrations, live dashboards, or systems that require streaming telemetry data.

## Socket Sink: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-sink-socket.json`). The only global setting is the default port; all other options are defined per channel, allowing each telemetry stream to specify its own protocol, destination, and behavior.

## Socket Sink: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-sink-socket.json",
  "title": "Netify Sink Socket Configuration",
  "description": "JSON Schema for the Netify Sink Socket Plugin configuration.",
  "type": "object",
  "properties": {
    "default_port": {
      "type": "integer",
      "minimum": 1,
      "maximum": 65535,
      "description": "Default port used for TCP, UDP, and WebSockets (1-65535)."
    },
    "channels": {
      "type": "object",
      "description": "Channel configurations.",
      "additionalProperties": {
        "type": "object",
        "properties": {
          "enable": {
            "type": "boolean",
            "default": true,
            "description": "State of the output channel."
          },
          "bind_address": {
            "type": "string",
            "description": "Socket connection string. Supported protocols: unix, tcp, udp, ws, wss.",
            "pattern": "^(unix|tcp|udp|ws|wss)://.+$",
            "examples": [
              "unix:///var/run/netifyd/netify-sink.sock",
              "tcp://127.0.0.1:8080",
              "udp://0.0.0.0:9000",
              "ws://192.168.1.1:80"
            ]
          }
        },
        "required": ["bind_address"]
      },
      "examples": [
        {
          "flows": {
            "enable": true,
            "bind_address": "unix:///var/run/netifyd/flows.sock"
          },
          "aggregator": {
            "enable": true,
            "bind_address": "tcp://127.0.0.1:8080"
          }
        }
      ]
    }
  }
}

```

