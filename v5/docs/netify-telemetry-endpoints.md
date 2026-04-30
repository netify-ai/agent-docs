# Endpoints Telemetry

## Endpoints: Overview
The Endpoints telemetry provides an inventory of discovered network endpoints, keyed by MAC address and mapped to observed IP addresses.  It offers a compact view of host identity at Layer 2 with associated Layer 3 addressing information.

This data is useful for endpoint visibility, asset tracking, and change detection across local segments. Monitoring the address set per MAC over time helps identify DHCP churn, dual-stack behavior, and potential identity inconsistencies in downstream analytics.

## Endpoints: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-endpoints.json",
  "title": "Netify Endpoints Telemetry",
  "description": "JSON Schema for Netify Agent v5 endpoints telemetry.",
  "type": "object",
  "properties": {
    "type": {
      "type": "string",
      "const": "endpoints",
      "description": "Telemetry record type. Always endpoints."
    }
  },
  "additionalProperties": {
    "type": "array",
    "items": {
      "type": "string"
    },
    "description": "Dynamic key for an observed endpoint MAC address. The value is a list of associated IP addresses (IPv4 and/or IPv6)."
  },
  "examples": [
    {
      "44:39:c4:8f:0d:92": [
        "fe80::4639:c4ff:fe8f:d92",
        "192.168.88.1"
      ],
      "60:e3:27:4a:a4:30": [
        "192.168.88.2"
      ],
      "9c:6b:00:30:a2:f0": [
        "192.168.88.196"
      ],
      "a4:7d:78:cb:e0:b6": [
        "fe80::a67d:78ff:fecb:e0b6",
        "192.168.88.198"
      ],
      "type": "endpoints"
    }
  ]
}

```

