# Log Sink Plugin

## Log Sink: Overview
The Netify log output interface enables the export of [network telemetry](https://www.netify.ai/products/netify-dpi/telemetry) and metadata from the DPI engine to local log files, which can be consumed by monitoring systems, automation scripts, or custom tools. This method is most often used for development and debugging, providing a simple, reliable way to capture telemetry without requiring persistent network connections or additional messaging layers.

## Log Sink: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-sink-log.json`). Top-level properties set global defaults, while per-channel properties override those defaults and define log prefixes. Use the global settings for shared behavior, and apply per-channel overrides when specific streams need separate files, naming, or overwrite policies.

## Log Sink: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-sink-log.json",
  "title": "Netify Sink Log Configuration",
  "description": "JSON Schema for the Netify Sink Log Plugin configuration.",
  "type": "object",
  "properties": {
    "overwrite": {
      "type": "boolean",
      "default": false,
      "description": "Default file overwrite policy."
    },
    "log_path": {
      "type": "string",
      "default": "/tmp",
      "description": "Default output folder for log files."
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
          "overwrite": {
            "type": "boolean",
            "description": "Channel-specific overwrite policy."
          },
          "log_path": {
            "type": "string",
            "description": "Channel-specific output folder for log files."
          },
          "log_name": {
            "type": "string",
            "minLength": 1,
            "description": "Prefix prepended to generated log filenames.",
            "examples": ["netify-intel", "security-endpoints"]
          }
        },
        "required": ["log_name"]
      },
      "examples": [
        {
          "endpoints": {
            "overwrite": true,
            "log_path": "/var/log/telemetry",
            "log_name": "netify-endpoints"
          },
          "intel": {
            "log_path": "/var/log/telemetry",
            "log_name": "netify-intel-"
          }
        }
      ]
    }
  }
}

```

