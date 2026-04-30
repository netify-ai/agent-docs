# Network Interfaces Telemetry

## Network Interfaces: Overview
The network Interfaces telemetry provides a live inventory of network
interfaces that the Netify Agent can observe. Each interface entry includes
addressing information, capture method, and role, making it easy to confirm
expected interface discovery and classification.

This dataset is useful for validating deployment state and troubleshooting
capture coverage. By reviewing interface role assignments alongside addresses
and MAC identifiers, you can quickly detect missing interfaces, unexpected
role changes, or source mismatches in downstream analytics.

## Network Interfaces: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-interfaces.json",
  "title": "Netify Network Interfaces Telemetry",
  "description": "JSON Schema for Netify Agent v5 network interfaces telemetry.",
  "type": "object",
  "properties": {
    "type": {
      "type": "string",
      "const": "interfaces",
      "description": "Telemetry record type. Always interfaces."
    }
  },
  "required": [
    "type"
  ],
  "additionalProperties": {
    "$ref": "#/$defs/interfaceAttributes",
    "description": "Per-interface object keyed by interface name."
  },
  "$defs": {
    "interfaceAttributes": {
      "type": "object",
      "description": "Attributes for a single monitored network interface.",
      "properties": {
        "addr": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "List of IP addresses assigned to the interface."
        },
        "capture_type": {
          "type": "string",
          "enum": [
            "nfqueue",
            "pcap",
            "tpv3"
          ],
          "description": "Capture type in use for the interface."
        },
        "mac": {
          "type": "string",
          "description": "Interface MAC address."
        },
        "role": {
          "type": "string",
          "enum": [
            "lan",
            "wan"
          ],
          "description": "Assigned network role for the interface."
        }
      },
      "required": [
        "capture_type",
        "mac",
        "role"
      ]
    }
  },
  "examples": [
    {
      "eno1": {
        "addr": [
          "fe80::4639:c4ff:fe8f:d92",
          "fdc0:1766:43b:0:4639:c4ff:fe8f:d92",
          "11.113.88.4"
        ],
        "capture_type": "pcap",
        "mac": "44:39:c4:8f:0d:92",
        "role": "wan"
      },
      "eno2": {
        "addr": [
          "192.168.4.44",
          "fe80::901:29c0:649b:7efe"
        ],
        "capture_type": "pcap",
        "mac": "f8:e9:03:01:69:13",
        "role": "lan"
      },
      "type": "interfaces"
    }
  ]
}

```

