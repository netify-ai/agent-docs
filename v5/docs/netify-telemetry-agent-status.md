# Agent Status Telemetry

## Agent Status: Overview
The Agent Status telemetry captures the agent's operational heartbeat, providing a clear snapshot of runtime health. It reports key metrics such as CPU utilization, memory footprint, cache performance, flow table activity, packet throughput, and overall uptime, all in a single, structured record. These metrics give you immediate insight into how the engine performs under current network conditions.

## Agent Status: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-telemetry-agent-status.json",
  "title": "Netify Agent Status Telemetry",
  "description": "JSON Schema for Netify Agent v5 status and performance statistics telemetry.",
  "type": "object",
  "properties": {
    "agent_version": {
      "type": "string",
      "description": "Semantic version of the running Netify Agent."
    },
    "build_version": {
      "type": "string",
      "description": "Full agent build string including platform and enabled feature flags."
    },
    "cpu_cores": {
      "type": "integer",
      "description": "Number of CPU cores available to the agent process."
    },
    "cpu_system": {
      "type": "number",
      "description": "Current cumulative system CPU usage reported by the agent."
    },
    "cpu_system_prev": {
      "type": "number",
      "description": "Previous system CPU usage sample."
    },
    "cpu_user": {
      "type": "number",
      "description": "Current cumulative user CPU usage reported by the agent."
    },
    "cpu_user_prev": {
      "type": "number",
      "description": "Previous user CPU usage sample."
    },
    "dhc_status": {
      "type": "boolean",
      "description": "Status flag for the DNS hint cache subsystem."
    },
    "fhc_status": {
      "type": "boolean",
      "description": "Status flag for the flow hash cache subsystem."
    },
    "flow_count": {
      "type": "integer",
      "description": "Current number of tracked flows."
    },
    "flow_count_prev": {
      "type": "integer",
      "description": "Previous tracked flow count sample."
    },
    "flows_active": {
      "type": "integer",
      "description": "Number of active flows."
    },
    "flows_expired": {
      "type": "integer",
      "description": "Number of flows that have expired."
    },
    "flows_expiring": {
      "type": "integer",
      "description": "Number of flows currently in the process of expiring."
    },
    "flows_in_use": {
      "type": "integer",
      "description": "Number of flow table entries currently in use."
    },
    "flows_purged": {
      "type": "integer",
      "description": "Number of flows purged by maintenance routines."
    },
    "maxrss_kb": {
      "type": "integer",
      "description": "Current high-water resident set size in kilobytes."
    },
    "maxrss_kb_prev": {
      "type": "integer",
      "description": "Previous high-water resident set size sample in kilobytes."
    },
    "memrss_kb": {
      "type": "integer",
      "description": "Current resident memory usage in kilobytes."
    },
    "memrss_kb_prev": {
      "type": "integer",
      "description": "Previous resident memory usage sample in kilobytes."
    },
    "tcm_kb": {
      "type": "integer",
      "description": "Current TCMalloc-managed memory in kilobytes."
    },
    "tcm_kb_prev": {
      "type": "integer",
      "description": "Previous TCMalloc-managed memory sample in kilobytes."
    },
    "timestamp": {
      "type": "integer",
      "description": "Unix epoch timestamp (seconds) when the status record was generated."
    },
    "type": {
      "type": "string",
      "const": "agent_status",
      "description": "Telemetry record type. Always agent_status."
    },
    "update_imf": {
      "type": "integer",
      "description": "Internal update indicator for incremental maintenance/metrics refresh."
    },
    "update_interval": {
      "type": "integer",
      "description": "Status publication interval in seconds."
    },
    "uptime": {
      "type": "integer",
      "description": "Agent uptime in seconds."
    }
  },
  "required": [
    "agent_version",
    "build_version",
    "cpu_cores",
    "cpu_system",
    "cpu_system_prev",
    "cpu_user",
    "cpu_user_prev",
    "dhc_status",
    "fhc_status",
    "flow_count",
    "flow_count_prev",
    "flows_active",
    "flows_expired",
    "flows_expiring",
    "flows_in_use",
    "flows_purged",
    "maxrss_kb",
    "maxrss_kb_prev",
    "memrss_kb",
    "memrss_kb_prev",
    "tcm_kb",
    "tcm_kb_prev",
    "timestamp",
    "type",
    "update_imf",
    "update_interval",
    "uptime"
  ],
  "examples": [
    {
      "agent_version": "5.2.0",
      "build_version": "Netify Agent/5.2.0-...",
      "cpu_cores": 8,
      "cpu_system": 3.148931,
      "cpu_system_prev": 3.134846,
      "cpu_user": 6.237091,
      "cpu_user_prev": 6.206476,
      "dhc_status": true,
      "fhc_status": true,
      "flow_count": 140,
      "flow_count_prev": 135,
      "flows_active": 55,
      "flows_expired": 21,
      "flows_expiring": 0,
      "flows_in_use": 120,
      "flows_purged": 11,
      "maxrss_kb": 85504,
      "maxrss_kb_prev": 85504,
      "memrss_kb": 84508,
      "memrss_kb_prev": 84380,
      "tcm_kb": 48141,
      "tcm_kb_prev": 48076,
      "timestamp": 1772661775,
      "type": "agent_status",
      "update_imf": 1,
      "update_interval": 15,
      "uptime": 1740
    }
  ]
}

```

