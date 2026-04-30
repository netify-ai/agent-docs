# Network Interface Stats Telemetry

## Network Interface Stats: Overview
The network Interface Stats and Global Stats telemetry records are your primary window into the Netify DPI engine's interaction with your network hardware. By providing real-time packet and byte telemetry for every active interface, it allows you to monitor exactly how your traffic is being observed and handled. This visibility ensures that your classification engine is receiving the raw data it needs to maintain high-accuracy visibility.

The interface_stats record provides stats per network interface, while the global_stats record provides the same information summarized across all monitored network interfaces.

## Network Interface Stats: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-interface-stats.json",
  "title": "Netify Network Interface Stats Telemetry",
  "description": "JSON Schema for Netify Agent v5 network interface and global stats telemetry.",
  "type": "object",
  "oneOf": [
    {
      "description": "Per-interface statistics object keyed by interface name.",
      "properties": {
        "type": {
          "type": "string",
          "const": "interface_stats",
          "description": "Telemetry record type for per-interface statistics."
        }
      },
      "required": ["type"],
      "additionalProperties": {
        "$ref": "#/$defs/statsAttributes"
      }
    },
    {
      "description": "Global statistics object summarized across all monitored network interfaces.",
      "properties": {
        "type": {
          "type": "string",
          "const": "global_stats",
          "description": "Telemetry record type for summarized global statistics."
        }
      },
      "required": ["type"],
      "allOf": [
        {
          "$ref": "#/$defs/statsAttributes"
        }
      ]
    }
  ],
  "$defs": {
    "statsAttributes": {
      "type": "object",
      "description": "Common network interface statistics counters.",
      "properties": {
        "capture_dropped": {
          "type": "integer",
          "description": "Total number of packets dropped by the packet capture layer."
        },
        "capture_filtered": {
          "type": "integer",
          "description": "Total packets filtered out (removed) in the case where Berkely Packet Filters are in use."
        },
        "discarded": {
          "type": "integer",
          "description": "Total packet counts being discarded prior to entering the DPI engine for analysis."
        },
        "discarded_bytes": {
          "type": "integer",
          "description": "Total number of discarded bytes."
        },
        "ethernet": {
          "type": "integer",
          "description": "Total number of supported Ethernet header counts seen."
        },
        "flow_dropped": {
          "type": "integer",
          "description": "Total number of packets dropped during flow processing."
        },
        "fragmented": {
          "type": "integer",
          "description": "Number of fragmented IP packets observed."
        },
        "icmp": {
          "type": "integer",
          "description": "Total number of ICMP packets."
        },
        "igmp": {
          "type": "integer",
          "description": "Total number of IGMP packets."
        },
        "ip": {
          "type": "integer",
          "description": "Total number of IP packets."
        },
        "ip_bytes": {
          "type": "integer",
          "description": "Total number of bytes carried in IP packets."
        },
        "largest_bytes": {
          "type": "integer",
          "description": "Largest frame size, in bytes."
        },
        "mpls": {
          "type": "integer",
          "description": "Total number of MPLS packets."
        },
        "pppoe": {
          "type": "integer",
          "description": "Total number of PPPoE packets."
        },
        "queue_dropped": {
          "type": "integer",
          "description": "Total number of packets dropped due to internal queue limits."
        },
        "raw": {
          "type": "integer",
          "description": "Total number of raw packets captured on the interface."
        },
        "tcp": {
          "type": "integer",
          "description": "Total number of TCP packets."
        },
        "tcp_resets": {
          "type": "integer",
          "description": "Total number of TCP reset (RST) packets."
        },
        "tcp_seq_errors": {
          "type": "integer",
          "description": "Total number of TCP sequence errors."
        },
        "udp": {
          "type": "integer",
          "description": "Total number of UDP packets."
        },
        "vlan": {
          "type": "integer",
          "description": "Total number of VLAN tagged packets."
        },
        "wire_bytes": {
          "type": "integer",
          "description": "Total number of bytes observed on the wire, including frame overhead."
        }
      },
      "required": [
        "capture_dropped",
        "capture_filtered",
        "discarded",
        "discarded_bytes",
        "ethernet",
        "flow_dropped",
        "fragmented",
        "icmp",
        "igmp",
        "ip",
        "ip_bytes",
        "largest_bytes",
        "mpls",
        "pppoe",
        "queue_dropped",
        "raw",
        "tcp",
        "tcp_resets",
        "tcp_seq_errors",
        "udp",
        "vlan",
        "wire_bytes"
      ]
    }
  },
  "examples": [
    {
      "type": "interface_stats",
      "eth0": {
        "capture_dropped": 0,
        "capture_filtered": 0,
        "discarded": 12,
        "discarded_bytes": 1012,
        "ethernet": 772,
        "flow_dropped": 0,
        "fragmented": 0,
        "icmp": 3,
        "igmp": 2,
        "ip": 760,
        "ip_bytes": 261408,
        "largest_bytes": 1514,
        "mpls": 0,
        "pppoe": 0,
        "queue_dropped": 0,
        "raw": 772,
        "tcp": 648,
        "tcp_resets": 0,
        "tcp_seq_errors": 0,
        "udp": 107,
        "vlan": 0,
        "wire_bytes": 279648
      }
    },
    {
      "capture_dropped": 0,
      "capture_filtered": 0,
      "discarded": 13,
      "discarded_bytes": 1070,
      "ethernet": 2049,
      "flow_dropped": 0,
      "fragmented": 0,
      "icmp": 4,
      "igmp": 13,
      "ip": 2036,
      "ip_bytes": 1218047,
      "largest_bytes": 4369,
      "mpls": 0,
      "pppoe": 0,
      "queue_dropped": 0,
      "raw": 2049,
      "tcp": 1844,
      "tcp_resets": 4,
      "tcp_seq_errors": 0,
      "type": "global_stats",
      "udp": 175,
      "vlan": 0,
      "wire_bytes": 1266911
    }
  ]
}

```

