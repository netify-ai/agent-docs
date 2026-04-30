# HTTP Sink Plugin

## HTTP Sink: Overview
The Netify HTTP POST interface enables the delivery of real-time [network telemetry](https://www.netify.ai/products/netify-dpi/telemetry) and metadata from the DPI engine to external systems via standard HTTP requests. This method allows telemetry data to be pushed to web services, APIs, or automation platforms without maintaining a persistent connection.

## HTTP Sink: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-sink-http.json`).

## HTTP Sink: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-sink-http.json",
  "title": "Netify Sink HTTP Configuration",
  "description": "JSON Schema for the Netify Sink HTTP Plugin configuration.",
  "type": "object",
  "properties": {
    "timeout_connect": {
      "type": "integer",
      "default": 30,
      "description": "Default connection timeout in seconds."
    },
    "timeout_transfer": {
      "type": "integer",
      "default": 300,
      "description": "Default transfer timeout in seconds."
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
          "timeout_connect": {
            "type": "integer",
            "description": "Connection timeout override in seconds."
          },
          "timeout_transfer": {
            "type": "integer",
            "description": "Transfer timeout override in seconds."
          },
          "url": {
            "type": "string",
            "description": "The destination URL. Supports variables like ${uuid_agent}.",
            "pattern": "^https?://[a-zA-Z0-9\\.\\-_/\\${}]+$",
            "examples": ["https://api.example.com/v1/${uuid_agent}/telemetry"]
          },
          "headers": {
            "type": "object",
            "description": "HTTP headers to add to the request. Supports variables like ${uuid_agent}.",
            "additionalProperties": {
              "type": "string"
            },
            "examples": [
              {
                "X-UUID": "${uuid_agent}",
                "X-UUID-Serial": "${uuid_serial}"
              }
            ]
          }
        },
        "required": ["url", "headers"]
      },
      "examples": [
        {
          "flows": {
            "enable": true,
            "url": "https://sink.example.com/v1/flows",
            "headers": {
              "X-UUID": "${uuid_agent}",
              "X-UUID-Serial": "${uuid_serial}"
            }
          },
          "intel": {
            "enable": true,
            "url": "https://sink.example.com/v1/intel",
            "headers": {
              "X-UUID": "${uuid_agent}",
              "X-UUID-Serial": "${uuid_serial}"
            }
          }
        }
      ]
    }
  }
}

```

