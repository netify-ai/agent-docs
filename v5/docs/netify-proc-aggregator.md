# Aggregator Processor Plugin

## Aggregator Processor: Overview
The Aggregator Processor plugin creates structured telemetry by aggregating individual flow records from the Netify Agent's memory.
This aggregated data is typically used for high-level dashboards with bandwidth usage statistics and key network metrics.

The plugin emits several predefined summary formats (see the Telemetry section). Each format provides aggregated metrics - counts, bytes, and flow totals - plus metadata such as application, protocol, IPs, MACs, ports, VLAN, and interface statistics.

## Aggregator Processor: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-proc-aggregator.json`).  The format and compressor properties define global defaults, which can be overridden per channel. See the examples below for how to apply channel-level overrides.

## Aggregator Processor: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-proc-aggregator.json",
  "title": "Netify Aggregator Plugin Configuration",
  "description": "JSON Schema for the Netify Aggregator plugin configuration.",
  "type": "object",
  "properties": {
    "aggregator": {
      "type": "integer",
      "description": "Aggregator format type.",
      "externalDocs": {
        "description": "Aggregator telemetry formats",
        "url": "https://www.netify.ai/documentation/agent/v5/processor/aggregator#telemetry"
      }
    },
    "log_interval": {
      "type": "integer",
      "default": 60,
      "description": "Interval in seconds between aggregate summary reports."
    },
    "privacy_mode": {
      "type": "boolean",
      "description": "If true, aggregation will not include breakdowns by MAC address or IP address."
    },
    "format": {
      "$ref": "#/$defs/formatType",
      "description": "Default payload format used for all outputs unless overridden in sinks."
    },
    "compressor": {
      "$ref": "#/$defs/compressorType",
      "description": "Default compression policy used for all outputs unless overridden in sinks."
    },
    "batched_rows": {
      "type": "integer",
      "default": 0,
      "description": "Limits the number of aggregate records processed in a single batch. Use 0 for unlimited."
    },
    "nested": {
      "type": "boolean",
      "default": false,
      "description": "Layout format for aggregate rows. 'true' for nested objects, 'false' for flat key-value pairs.",
      "externalDocs": {
        "description": "Flat versus nested information",
        "url": "https://www.netify.ai/documentation/agent/v5/processor/aggregator#nested"
      }
    },
    "criteria": {
      "type": "object",
      "description": "An object array of supported flow criteria expressions.",
      "externalDocs": {
        "description": "See Expression Engine documentation</a>",
        "url": "/documentation/agent/v5/integrations/expression-engine"
      }
    },
    "sinks": {
      "type": "object",
      "description": "Sink configuration.",
      "additionalProperties": {
        "type": "object",
        "description": "The specific output target (e.g., 'sink-mqtt', 'sink-http').",
        "additionalProperties": {
          "type": "object",
          "description": "The specific channel or topic for the target (e.g., 'aggregate').",
          "properties": {
            "enable": {
              "type": "boolean",
              "default": true,
              "description": "Enables or disables this specific sink channel."
            },
            "format": {
              "$ref": "#/$defs/formatType"
            },
            "compressor": {
              "$ref": "#/$defs/compressorType"
            }
          }
        }
      }
    }
  },
  "required": [
    "aggregator",
    "batched_rows",
    "compressor",
    "format",
    "log_interval",
    "nested",
    "privacy_mode",
    "sinks"
  ],
  "$defs": {
    "formatType": {
      "type": "string",
      "enum": ["json", "msgpack"],
      "description": "The serialization format for the data payload."
    },
    "compressorType": {
      "type": "string",
      "enum": ["none", "gz"],
      "description": "The compression algorithm applied to the payload."
    }
  },
  "examples": [
    {
      "aggregator": 1,
      "log_interval": 15,
      "privacy_mode": false,
      "format": "json",
      "compressor": "gz",
      "batched_rows": 0,
      "nested": false,
      "criteria": [
        "ip_nat == false;",
        "vlan_id == 10;"
      ],
      "sinks": {
        "sink-log": {
          "aggregate": {
            "format": "json",
            "compressor": "none"
          }
        }
      }
    },
    {
      "aggregator": 2,
      "log_interval": 60,
      "privacy_mode": false,
      "format": "json",
      "compressor": "gz",
      "batched_rows": 0,
      "nested": false,
      "sinks": {
        "sink-log": {
          "aggregate": {
            "format": "json",
            "compressor": "none"
          }
        },
        "sink-mqtt": {
          "aggregate": {
            "format": "msgpack",
            "compressor": "gz"
          }
        }
      }
    }
  ]
}

```

