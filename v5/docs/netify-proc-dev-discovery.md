# Device Discovery Processor Plugin

## Device Discovery: Overview
Netify device discovery utility identifies all devices communicating within the local network in near real-time and on a continual basis.
This lets administrators keep tabs on anything from static, mainstay devices like desktops, printers and routers, to more transient devices, like
smartphones and tablets, that regularly come and go from the network.

The plugin is responsible for collecting metadata from the host where the agent is installed and securely sending
it up to the Netify cloud for analysis. All classification is done in the cloud and the plugin receives the results of the classification from the
machine learning models.

## Device Discovery: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-proc-dev-discovery.json`).

## Device Discovery: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-proc-dev-discovery.json",
  "title": "Netify Device Discovery Processor Configuration",
  "description": "JSON Schema for the Netify Device Discovery Processor plugin configuration.",
  "type": "object",
  "properties": {
    "format": {
      "$ref": "#/$defs/formatType",
      "description": "Default payload format used for all outputs unless overridden in sinks."
    },
    "compressor": {
      "$ref": "#/$defs/compressorType",
      "description": "Default compression policy used for all outputs unless overridden in sinks."
    },
    "max_confidence": {
      "type": "integer",
      "minimum": 0,
      "maximum": 100,
      "default": 80,
      "description": "Confidence threshold (0-100) required before discovery stops for a device."
    },
    "max_devices": {
      "type": "integer",
      "default": 0,
      "description": "Maximum number of devices to track in memory. 0 for unlimited."
    },
    "max_device_age": {
      "type": "integer",
      "default": 0,
      "description": "Maximum idle time (seconds) before a device is purged from memory."
    },
    "max_ja4_clients": {
      "type": "integer",
      "default": 10,
      "description": "Maximum number of extracted JA4 client hashes to collect on any given MAC address."
    },
    "max_mdns_services": {
      "type": "integer",
      "default": 10,
      "description": "Maximum number of extracted mDNS service strings to collect on any given MAC address."
    },
    "max_http_user_agents": {
      "type": "integer",
      "default": 10,
      "description": "Maximum number of extracted HTTP user agent strings to collect on any given MAC address."
    },
    "max_ssdp_user_agents": {
      "type": "integer",
      "default": 10,
      "description": "Maximum number of extracted SSDP user agent strings to collect on any given MAC address."
    },
    "path_device_cache": {
      "type": "string",
      "description": "Filesystem path for persistent device cache."
    },
    "process_all_macs": {
      "type": "boolean",
      "default": false,
      "description": "If false, only local/unicast MAC addresses are processed."
    },
    "device_mac_ignore": {
      "type": "array",
      "description": "List of MAC addresses to exclude from discovery.",
      "items": {
        "type": "string",
        "pattern": "^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$"
      }
    },
    "netify_api": {
      "type": "object",
      "description": "Netify API object definition.",
      "enable": {
        "type": "boolean",
        "default": true,
        "description": "Enables or disables this Netify Clound discovery engine."
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
            },
            "flush": {
              "type": "boolean",
              "default": true,
              "description": "The flush policy."
            }
          }
        }
      }
    }
  },
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
      "compressor": "gz",
      "format": "json",
      "max_confidence": 80,
      "max_devices": 1500,
      "max_device_age": 4147200,
      "path_device_cache": "${path_state_persistent}/device-discovery-cache.json",
      "device_mac_ignore": [
        "00:00:00:00:00:00",
        "ff:ff:ff:ff:ff:ff"
      ],
      "max_ja4_clients": {
        "max": 10,
        "update_min": 1,
        "update_ttl": 3600
      },
      "max_mdns_services": 10,
      "max_http_user_agents": 10,
      "max_ssdp_user_agents": 10,
      "netify_api": {
        "enable": true
      },
      "sinks": {
        "sink-log": {
          "devices": {
            "enable": true,
            "flush": true,
            "format": "json",
            "compressor": "none"
          }
        }
      }
    }
  ]
}

```

