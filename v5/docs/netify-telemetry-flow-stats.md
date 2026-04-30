# Flow Stats Telemetry

## Flow Stats: Overview
The Flow Stats telemetry provides periodic runtime statistics for
active flows, especially long-lived sessions such as streaming, VPN, and
persistent service connections. It complements flow metadata by reporting
ongoing packet, byte, and rate counters while a flow remains active.

This record is most valuable for near-real-time monitoring and QoE analysis.
Tracking directional throughput and TCP health metrics over time helps detect
performance degradation, congestion, and retransmission-related issues before
a flow is closed.

## Flow Stats: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-flow-stats.json",
  "title": "Netify Flow Stats Telemetry",
  "description": "JSON Schema for Netify Agent v5 flow stats telemetry.",
  "type": "object",
  "properties": {
    "flow": {
      "type": "object",
      "description": "Container for per-flow live statistics.",
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
          "items": { "type": "string" },
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
          "description": "Current local transmit rate for the flow."
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
          "description": "Current remote transmit rate for the flow."
        },
        "total_bytes": {
          "type": "integer",
          "description": "Total bytes seen for the flow in both directions."
        },
        "total_packets": {
          "type": "integer",
          "description": "Total packets seen for the flow in both directions."
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
          "required": ["resets", "retrans", "seq_errors"]
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
      "description": "Interface name associated with the flow update."
    },
    "internal": {
      "type": "boolean",
      "description": "Indicates whether this flow is internal to the local network context."
    },
    "type": {
      "type": "string",
      "const": "flow_stats",
      "description": "Telemetry record type. Always flow_stats."
    }
  },
  "required": [
    "flow",
    "interface",
    "internal",
    "type"
  ],
  "examples": [
    {
      "flow": {
        "detection_packets": 2,
        "digest": "c3086c57745b...",
        "digest_prev": [
          "fcd1061d4c2ba844..."
        ],
        "last_seen_at": 1772665318561,
        "local_bytes": 5559,
        "local_packets": 6,
        "local_rate": 5559,
        "other_bytes": 913,
        "other_packets": 6,
        "other_rate": 913,
        "tcp": {
          "resets": 0,
          "retrans": 0,
          "seq_errors": 0
        },
        "total_bytes": 613175,
        "total_packets": 1064
      },
      "interface": "wlp3s0",
      "internal": true,
      "type": "flow_stats"
    }
  ]
}

```

