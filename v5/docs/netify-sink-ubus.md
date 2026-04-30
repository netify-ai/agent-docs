# ubus Sink Plugin

## ubus Sink: Overview
The Netify ubus interface enables the real-time export of [network telemetry](https://www.netify.ai/products/netify-dpi/telemetry) and metadata from the DPI engine to local applications via the ubus messaging system, commonly used on OpenWrt, PrplOS and other embedded Linux distributions. This method provides lightweight, low-latency inter-process communication, making it ideal for local monitoring, automation scripts, and system integration.

## ubus Sink: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-sink-ubus.json`).

## ubus Sink: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-sink-ubus.json",
  "title": "Netify Sink ubus Configuration",
  "description": "JSON Schema for the Netify Sink ubus Plugin configuration.",
  "type": "object",
  "properties": {
    "channels": {
      "type": "object",
      "description": "Channel configurations.",
      "additionalProperties": {
        "type": "object",
        "properties": {
          "path": {
            "type": "string",
            "description": "The ubus object path to invoke (e.g., 'netify.metadata').",
            "pattern": "^[a-zA-Z0-9\\._\\-]+$"
          },
          "function": {
            "type": "string",
            "description": "The specific ubus method to call for published metadata.",
            "examples": [
              "notify",
              "publish",
              "set"
            ]
          }
        },
        "required": [
          "path",
          "function"
        ]
      },
      "examples": [
        {
          "flows": {
            "path": "netify.metadata",
            "function": "publish"
          },
          "intelligence": {
            "path": "netify.metadata",
            "function": "notify"
          }
        }
      ]
    }
  }
}

```

