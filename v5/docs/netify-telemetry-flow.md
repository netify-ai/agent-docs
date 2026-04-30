# Flow Telemetry

## Flow: Overview
The Flow telemetry record contains per-flow metadata produced by the DPI engine and flow tracker. Emitted at detection and during DPI updates and completion, it reports detected application and protocol identifiers, hostnames (SNI/HTTP Host), TLS certificate details, and protocol-specific metadata used for classification and enrichment.

Use the Flow record for event-driven detection, enrichment, routing decisions, and early-alerting workflows. For periodic bandwidth and KPI reporting use the [Flow Stats Telemetry](https://www.netify.ai/documentation/agent/v5/integrations/telemetry/flow-stats), and for final per-session counters and end-state details use the [Flow Purge Telemetry](https://www.netify.ai/documentation/agent/v5/integrations/telemetry/flow-purge).

## Flow: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-flow.json",
  "title": "Netify Flow Telemetry",
  "description": "JSON Schema for Netify Agent v5 flow telemetry.",
  "type": "object",
  "properties": {
    "flow": {
      "type": "object",
      "description": "Container for per-flow network and DPI metadata.",
      "properties": {
        "app_ip_override": {
          "type": "boolean",
          "description": "Indicates whether application IP override logic was applied."
        },
        "app_proto_twins": {
          "type": "boolean",
          "description": "Indicates whether protocol twin detection metadata was applied."
        },
        "bt": {
          "type": "object",
          "description": "BitTorrent protocol metadata extracted from the flow.",
          "properties": {
            "info_hash": {
              "type": "string",
              "description": "BitTorrent info hash represented as a hexadecimal SHA-1 digest."
            }
          },
          "required": [
            "info_hash"
          ]
        },
        "category": {
          "type": "object",
          "description": "Application, domain, and protocol category identifiers.",
          "properties": {
            "application": {
              "type": "integer",
              "description": "Detected application category ID.",
              "externalDocs": {
                "description": "Application Categories",
                "url": "https://www.netify.ai/documentation/agent/applications/categories"
              }
            },
            "domain": {
              "type": "integer",
              "description": "Detected domain category ID.",
              "externalDocs": {
                "description": "Category Lists",
                "url": "https://www.netify.ai/documentation/agent/v5/configuration/category-lists"
              }
            },
            "local_network": {
              "type": "integer",
              "description": "Local network category ID.",
              "externalDocs": {
                "description": "Category Lists",
                "url": "https://www.netify.ai/documentation/agent/v5/configuration/category-lists"
              }
            },
            "other_network": {
              "type": "integer",
              "description": "Other network category ID.",
              "externalDocs": {
                "description": "Category Lists",
                "url": "https://www.netify.ai/documentation/agent/v5/configuration/category-lists"
              }
            },
            "overlay": {
              "type": "integer",
              "description": "Overlay network category ID, when applicable.",
              "externalDocs": {
                "description": "Overlay",
                "url": "https://www.netify.ai/documentation/agent/v5/configuration/overlay"
              }
            },
            "protocol": {
              "type": "integer",
              "description": "Detected protocol category ID.",
              "externalDocs": {
                "description": "Protocol Categories",
                "url": "https://www.netify.ai/documentation/agent/protocols/categories"
              }
            }
          },
          "required": [
            "application",
            "domain",
            "overlay",
            "protocol"
          ]
        },
        "conntrack": {
          "type": "object",
          "description": "Connection tracking metadata.",
          "properties": {
            "id": {
              "type": "integer",
              "description": "Conntrack flow identifier."
            },
            "mark": {
              "type": "integer",
              "description": "Conntrack mark value."
            },
            "reply_dst_ip": {
              "type": "string",
              "description": "Destination IP in the conntrack reply tuple."
            },
            "reply_dst_port": {
              "type": "integer",
              "description": "Destination port in the conntrack reply tuple."
            },
            "reply_src_ip": {
              "type": "string",
              "description": "Source IP in the conntrack reply tuple."
            },
            "reply_src_port": {
              "type": "integer",
              "description": "Source port in the conntrack reply tuple."
            }
          },
          "required": [
            "id",
            "mark",
            "reply_dst_ip",
            "reply_dst_port",
            "reply_src_ip",
            "reply_src_port"
          ]
        },
        "detected_application": {
          "type": "integer",
          "description": "Detected application ID.",
          "externalDocs": {
            "description": "Applications Catalog",
            "url": "https://www.netify.ai/documentation/agent/applications/catalog"
          }
        },
        "detected_application_name": {
          "type": "string",
          "description": "Detected application tag.",
          "externalDocs": {
            "description": "Applications Catalog",
            "url": "https://www.netify.ai/documentation/agent/applications/catalog"
          }
        },
        "detected_protocol": {
          "type": "integer",
          "description": "Detected protocol ID.",
          "externalDocs": {
            "description": "Protocols Catalog",
            "url": "https://www.netify.ai/documentation/agent/protocols/catalog"
          }
        },
        "detected_protocol_name": {
          "type": "string",
          "description": "Detected protocol name from the underlying DPI driver."
        },
        "detection_guessed": {
          "type": "boolean",
          "description": "True when protocol classification is inferred by default port rather than fully dissected."
        },
        "detection_packets": {
          "type": "integer",
          "description": "Number of packets used to complete DPI classification."
        },
        "detection_updated": {
          "type": "boolean",
          "description": "True when additional packets update an earlier classification."
        },
        "dhc_hit": {
          "type": "boolean",
          "description": "Indicates whether domain hint cache contributed to classification."
        },
        "dhcp": {
          "type": "object",
          "description": "DHCP metadata extracted from protocol payloads when available.",
          "properties": {
            "class_ident": {
              "type": "string",
              "description": "DHCP class identifier value observed in the flow."
            },
            "fingerprint": {
              "type": "string",
              "description": "DHCP fingerprint string extracted from client options."
            }
          },
          "required": [
            "class_ident",
            "fingerprint"
          ]
        },
        "digest": {
          "type": "string",
          "description": "Current unique flow digest based on known flow attributes."
        },
        "digest_prev": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "List of previous sibling digests associated with this flow."
        },
        "dns_host_name": {
          "type": "string",
          "description": "Hostname associated with a corresponding DNS query."
        },
        "fhc_hit": {
          "type": "boolean",
          "description": "Indicates whether flow hash cache contributed to classification."
        },
        "first_seen_at": {
          "type": "integer",
          "description": "Timestamp in Unix epoch milliseconds when the flow was first observed."
        },
        "gtp": {
          "type": "object",
          "description": "GTP tunnel metadata when encapsulated traffic is detected.",
          "properties": {
            "ip_dscp": {
              "type": "integer",
              "description": "DSCP value observed for the inner GTP payload network context."
            },
            "ip_version": {
              "type": "integer",
              "description": "IP version for the inner GTP payload network context.",
              "enum": [
                4,
                6
              ]
            },
            "local_ip": {
              "type": "string",
              "description": "Local tunnel endpoint IP for the inner GTP flow."
            },
            "local_port": {
              "type": "integer",
              "description": "Local tunnel endpoint port for the inner GTP flow."
            },
            "local_teid": {
              "type": "integer",
              "description": "Local tunnel endpoint identifier (TEID) for the inner GTP flow."
            },
            "other_ip": {
              "type": "string",
              "description": "Other tunnel endpoint IP for the inner GTP flow."
            },
            "other_port": {
              "type": "integer",
              "description": "Other tunnel endpoint port for the inner GTP flow."
            },
            "other_teid": {
              "type": "integer",
              "description": "Other tunnel endpoint identifier (TEID) for the inner GTP flow."
            },
            "other_type": {
              "type": "string",
              "description": "Other endpoint network type for the inner GTP flow.",
              "enum": [
                "local",
                "remote",
                "unsupported",
                "error"
              ]
            },
            "version": {
              "type": "integer",
              "description": "Observed GTP protocol version."
            }
          },
          "required": [
            "ip_dscp",
            "ip_version",
            "local_ip",
            "local_port",
            "local_teid",
            "other_ip",
            "other_port",
            "other_teid",
            "other_type",
            "version"
          ]
        },
        "host_server_name": {
          "type": "string",
          "description": "Hostname extracted from protocol metadata (for example TLS SNI or HTTP Host header)."
        },
        "http": {
          "type": "object",
          "description": "HTTP metadata extracted from protocol payloads when available.",
          "properties": {
            "url": {
              "type": "string",
              "description": "HTTP URL observed in the flow payload."
            },
            "user_agent": {
              "type": "string",
              "description": "HTTP User-Agent header value observed in the flow payload."
            }
          },
          "required": [
            "url",
            "user_agent"
          ]
        },
        "ip_dscp": {
          "type": "integer",
          "description": "Differentiated Services Code Point (DSCP) value."
        },
        "ip_nat": {
          "type": "boolean",
          "description": "Indicates whether Network Address Translation (NAT) was detected."
        },
        "ip_protocol": {
          "type": "integer",
          "description": "IP protocol number.",
          "externalDocs": {
            "description": "Wikipedia",
            "url": "https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers"
          }
        },
        "ip_version": {
          "type": "integer",
          "description": "IP version used by the flow.",
          "enum": [
            4,
            6
          ]
        },
        "last_seen_at": {
          "type": "integer",
          "description": "Timestamp in Unix epoch milliseconds when the flow was last observed."
        },
        "local_bytes": {
          "type": "integer",
          "description": "Bytes sent from the local endpoint in the current lifecycle."
        },
        "local_ip": {
          "type": "string",
          "description": "Local endpoint IP address."
        },
        "local_mac": {
          "type": "string",
          "description": "Local endpoint MAC address."
        },
        "local_origin": {
          "type": "boolean",
          "description": "Indicates whether the local endpoint originated the connection."
        },
        "local_packets": {
          "type": "integer",
          "description": "Packets sent from the local endpoint in the current lifecycle."
        },
        "local_port": {
          "type": "integer",
          "description": "Local endpoint port."
        },
        "local_rate": {
          "type": "number",
          "description": "Burst local rate for the flow in the current lifecycle."
        },
        "mdns": {
          "type": "object",
          "description": "mDNS metadata extracted from multicast DNS traffic when available.",
          "properties": {
            "answer": {
              "type": "string",
              "description": "mDNS answer domain name extracted from the flow."
            }
          },
          "required": [
            "answer"
          ]
        },
        "nfq": {
          "type": "object",
          "description": "NFQUEUE interface metadata when packet queue integration is enabled.",
          "properties": {
            "dst_iface": {
              "type": "string",
              "description": "Destination interface name reported by NFQUEUE for this flow."
            },
            "src_iface": {
              "type": "string",
              "description": "Source interface name reported by NFQUEUE for this flow."
            }
          },
          "required": [
            "dst_iface",
            "src_iface"
          ]
        },
        "other_bytes": {
          "type": "integer",
          "description": "Bytes sent from the other endpoint in the current lifecycle."
        },
        "other_ip": {
          "type": "string",
          "description": "Other endpoint IP address."
        },
        "other_mac": {
          "type": "string",
          "description": "Other endpoint MAC address when available."
        },
        "other_packets": {
          "type": "integer",
          "description": "Packets sent from the other endpoint in the current lifecycle."
        },
        "other_port": {
          "type": "integer",
          "description": "Other endpoint port."
        },
        "other_rate": {
          "type": "number",
          "description": "Current other transmit rate for the flow in the current lifecycle."
        },
        "other_type": {
          "type": "string",
          "description": "Other endpoint network type classification.",
          "enum": [
            "local",
            "remote",
            "broadcast",
            "multicast",
            "unsupported",
            "error",
            "unknown"
          ]
        },
        "risks": {
          "type": "object",
          "description": "Risk assessment data derived from nDPI protocol driver.",
          "properties": {
            "ndpi_risk_score": {
              "type": "integer",
              "description": "Aggregate nDPI risk score for the flow."
            },
            "ndpi_risk_score_client": {
              "type": "integer",
              "description": "nDPI risk score associated with client-side indicators."
            },
            "ndpi_risk_score_server": {
              "type": "integer",
              "description": "nDPI risk score associated with server-side indicators."
            },
            "risks": {
              "type": "array",
              "items": {
                "type": "integer"
              },
              "description": "Set of nDPI risk identifiers triggered for this flow."
            }
          },
          "required": [
            "ndpi_risk_score",
            "ndpi_risk_score_client",
            "ndpi_risk_score_server",
            "risks"
          ]
        },
        "soft_dissector": {
          "type": "boolean",
          "description": "Indicates whether a soft dissector was used."
        },
        "ssl": {
          "type": "object",
          "description": "TLS/SSL metadata extracted when encrypted traffic is detected.",
          "properties": {
            "alpn": {
              "type": "array",
              "items": {
                "type": "string"
              },
              "description": "List of Application-Layer Protocol Negotiation (ALPN) protocol identifiers offered or negotiated for the TLS session."
            },
            "alpn_server": {
              "type": "array",
              "items": {
                "type": "string"
              },
              "description": "List of Application-Layer Protocol Negotiation (ALPN) protocol identifiers provided."
            },
            "cipher_suite": {
              "type": "string",
              "description": "Hexadecimal TLS cipher suite identifier (for example 0x1303)."
            },
            "client_ja4": {
              "type": "string",
              "description": "JA4 TLS client fingerprint extracted from the handshake."
            },
            "client_sni": {
              "type": "string",
              "description": "Server Name Indication (SNI) value sent by the TLS client."
            },
            "encrypted_ch_version": {
              "type": "string",
              "description": "Encrypted client hello (ECH) version."
            },
            "fingerprint": {
              "type": "string",
              "description": "SHA-1 digest fingerprint of the observed TLS certificate."
            },
            "issuer_dn": {
              "type": "string",
              "description": "TLS server certificate issuer distinguished name, e.g. Let's Encrypt."
            },
            "server_cn": {
              "type": "string",
              "description": "TLS server common name."
            },
            "subject_dn": {
              "type": "string",
              "description": "TLS server certificate issuer distinguished name."
            },
            "version": {
              "type": "string",
              "description": "Hexadecimal TLS version value observed in handshake metadata."
            }
          }
        },
        "ssh": {
          "type": "object",
          "description": "SSH handshake metadata extracted when available.",
          "properties": {
            "client": {
              "type": "string",
              "description": "SSH client agent or software identification string."
            },
            "server": {
              "type": "string",
              "description": "SSH server agent or software identification string."
            }
          }
        },
        "ssdp": {
          "type": "object",
          "description": "SSDP metadata extracted when available.",
          "properties": {
            "user_agent": {
              "type": "string",
              "description": "SSDP User-Agent value observed in the flow."
            }
          },
          "required": [
            "user_agent"
          ]
        },
        "stun": {
          "type": "object",
          "description": "STUN address metadata extracted from STUN payloads when available.",
          "properties": {
            "mapped": {
              "type": "string",
              "description": "STUN mapped address (host:port) observed in the flow."
            },
            "other": {
              "type": "string",
              "description": "STUN other address (host:port) observed in the flow."
            },
            "peer": {
              "type": "string",
              "description": "STUN peer address (host:port) observed in the flow."
            },
            "relayed": {
              "type": "string",
              "description": "STUN relayed address (host:port) observed in the flow."
            },
            "response": {
              "type": "string",
              "description": "STUN response address (host:port) observed in the flow."
            }
          },
          "required": [
            "mapped",
            "other",
            "peer"
          ]
        },
        "tags": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "List of tags associated with this flow."
        },
        "tcp": {
          "type": "object",
          "description": "TCP health and error counters for TCP flows.",
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
        },
        "vlan_id": {
          "type": "integer",
          "description": "Observed VLAN ID."
        }
      },
      "required": [
        "app_ip_override",
        "app_proto_twins",
        "category",
        "detected_application",
        "detected_application_name",
        "detected_protocol",
        "detected_protocol_name",
        "detection_guessed",
        "detection_packets",
        "detection_updated",
        "dhc_hit",
        "digest",
        "digest_prev",
        "dns_host_name",
        "fhc_hit",
        "first_seen_at",
        "host_server_name",
        "ip_dscp",
        "ip_nat",
        "ip_protocol",
        "ip_version",
        "last_seen_at",
        "local_bytes",
        "local_ip",
        "local_mac",
        "local_origin",
        "local_packets",
        "local_port",
        "local_rate",
        "other_bytes",
        "other_ip",
        "other_mac",
        "other_packets",
        "other_port",
        "other_rate",
        "other_type",
        "soft_dissector",
        "total_bytes",
        "total_packets",
        "vlan_id"
      ]
    },
    "interface": {
      "type": "string",
      "description": "Interface name associated with this flow record."
    },
    "internal": {
      "type": "boolean",
      "description": "Indicates whether this flow is internal to the local network context."
    },
    "type": {
      "type": "string",
      "enum": [
        "flow",
        "flow_dpi_update",
        "flow_dpi_complete"
      ],
      "description": "Telemetry record type. Always flow."
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
        "app_ip_override": false,
        "category": {
          "application": 28,
          "domain": 0,
          "local_network": 0,
          "other_network": 0,
          "overlay": 0,
          "protocol": 22
        },
        "conntrack": {
          "id": 3603527535,
          "mark": 0,
          "reply_dst_ip": "192.168.4.44",
          "reply_dst_port": 35636,
          "reply_src_ip": "192.200.0.102",
          "reply_src_port": 443
        },
        "detected_application": 11354,
        "detected_application_name": "netify.tailscale",
        "detected_protocol": 196,
        "detected_protocol_name": "HTTP/S",
        "detection_guessed": false,
        "detection_updated": false,
        "dhc_hit": false,
        "digest": "c4c07ca55baa19a7fe3652bcd356765a7...",
        "digest_prev": [
          "463c53093403fcce8eeb01df5b5125df66a0f53b"
        ],
        "dns_host_name": "login.tailscale.com",
        "fhc_hit": false,
        "first_seen_at": 1772738467573,
        "host_server_name": "login.tailscale.com",
        "ip_dscp": 0,
        "ip_nat": false,
        "ip_protocol": 6,
        "ip_version": 4,
        "last_seen_at": 1772738467684,
        "local_ip": "192.168.4.44",
        "local_mac": "f8:e9:03:01:69:13",
        "local_origin": true,
        "local_port": 35636,
        "other_ip": "192.200.0.102",
        "other_mac": "3c:7c:3f:a1:ed:58",
        "other_port": 443,
        "other_type": "remote",
        "risks": {
          "ndpi_risk_score": 0,
          "ndpi_risk_score_client": 0,
          "ndpi_risk_score_server": 0,
          "risks": []
        },
        "soft_dissector": false,
        "ssl": {
          "alpn": [
            "h2",
            "http/1.1"
          ],
          "cipher_suite": "0x0000",
          "client_ja4": "t13d1817h2_e8a523a41297_...",
          "client_sni": "login.tailscale.com",
          "encrypted_ch_version": "0xfe0d",
          "version": "0x0303"
        },
        "vlan_id": 0
      },
      "interface": "wlp3s0",
      "internal": true,
      "type": "flow"
    }
  ]
}

```

