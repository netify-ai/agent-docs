# Flow Purge Telemetry

## Flow Purge: Overview
The Flow Purge telemetry is emitted when a tracked flow is removed from the engine, typically after normal connection closure (TCP), inactivity timeout, or internal purge conditions. It captures final flow counters and end-state details at the point the flow leaves active tracking.  

Use this record for post-session analysis, accounting, and flow lifecycle validation. Comparing purge records with periodic [Flow Stats Telemetry](https://www.netify.ai/documentation/agent/v5/integrations/telemetry/flow-stats) updates helps explain why sessions ended and whether termination behavior aligns with network expectations.

## Flow Purge: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-flow-purge.json",
  "title": "Netify Flow Purge Telemetry",
  "description": "JSON Schema for Netify Agent v5 flow purge telemetry.",
  "type": "object",
  "properties": {
    "flow": {
      "type": "object",
      "description": "Container for final per-flow statistics at purge time.",
      "properties": {
        "detection_packets": {
          "type": "integer",
          "description": "Number of packets used for application/protocol detection."
        },
        "digest": {
          "type": "string",
          "description": "Current flow digest identifier."
        },
        "digest_prev": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "List of previous digest identifiers associated with the flow."
        },
        "last_seen_at": {
          "type": "integer",
          "description": "Timestamp in Unix epoch milliseconds when the flow was last observed."
        },
        "local_bytes": {
          "type": "integer",
          "description": "Bytes sent from the local endpoint."
        },
        "local_packets": {
          "type": "integer",
          "description": "Packets sent from the local endpoint."
        },
        "local_rate": {
          "type": "integer",
          "description": "Final local transmit rate for the flow."
        },
        "other_bytes": {
          "type": "integer",
          "description": "Bytes sent from the remote endpoint."
        },
        "other_packets": {
          "type": "integer",
          "description": "Packets sent from the remote endpoint."
        },
        "other_rate": {
          "type": "integer",
          "description": "Final remote transmit rate for the flow."
        },
        "tcp": {
          "type": "object",
          "description": "TCP health and error counters. Omitted for non-TCP flows.",
          "properties": {
            "resets": {
              "type": "integer",
              "description": "Count of observed TCP reset packets for the flow."
            },
            "retrans": {
              "type": "integer",
              "description": "Count of observed TCP retransmissions for the flow."
            },
            "seq_errors": {
              "type": "integer",
              "description": "Count of TCP sequence anomalies for the flow."
            }
          },
          "required": [
            "resets",
            "retrans",
            "seq_errors"
          ]
        },
        "total_bytes": {
          "type": "integer",
          "description": "Total bytes seen for the flow in both directions."
        },
        "total_packets": {
          "type": "integer",
          "description": "Total packets seen for the flow in both directions."
        }
      },
      "required": [
        "detection_packets",
        "digest",
        "digest_prev",
        "last_seen_at",
        "local_bytes",
        "local_packets",
        "local_rate",
        "other_bytes",
        "other_packets",
        "other_rate",
        "total_bytes",
        "total_packets"
      ]
    },
    "interface": {
      "type": "string",
      "description": "Interface name associated with the purged flow."
    },
    "internal": {
      "type": "boolean",
      "description": "Indicates whether the flow is internal to the local network context."
    },
    "reason": {
      "type": "string",
      "enum": ["closed", "expired"],
      "description": "Purge reason reported by the engine."
    },
    "type": {
      "type": "string",
      "const": "flow_purge",
      "description": "Telemetry record type. Always flow_purge."
    }
  },
  "required": [
    "flow",
    "interface",
    "internal",
    "reason",
    "type"
  ],
  "examples": [
    {
      "flow": {
        "detection_packets": 12,
        "digest": "fb69e87ed3b...",
        "digest_prev": [
          "d7fddd35cc27..."
        ],
        "last_seen_at": 1772665905392,
        "local_bytes": 0,
        "local_packets": 0,
        "local_rate": 387,
        "other_bytes": 0,
        "other_packets": 0,
        "other_rate": 300,
        "tcp": {
          "resets": 0,
          "retrans": 0,
          "seq_errors": 0
        },
        "total_bytes": 13968,
        "total_packets": 51
      },
      "interface": "wlp3s0",
      "internal": true,
      "reason": "closed",
      "type": "flow_purge"
    }
  ]
}

```

