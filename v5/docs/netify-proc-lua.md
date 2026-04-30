# Lua Processor Plugin

## Lua Processor: Overview
The Netify Lua plugin enables direct integration with the Netify agent engine, allowing Lua scripts to hook into live network telemetry in real time. This plugin is an ideal option for rapid prototyping and custom applications, providing a flexible, lightweight environment for custom logic, data analysis, or experimental features without modifying the core DPI engine.

## Lua Processor: Configuration
Once enabled, the plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-proc-lua.json`). The configuration file provides a way to define the location of the Lua script.

## Lua Processor: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-proc-lua.json",
  "title": "Netify Lua Processor Configuration",
  "description": "JSON Schema for the Netify Lua Processor plugin configuration.",
  "type": "object",
  "properties": {
    "script": {
      "type": "string",
      "description": "The file system path to the Lua script."
    }
  },
  "required": [
    "script"
  ],
  "examples": [
    {
      "script": "/usr/local/bin/monitor.lua"
    }
  ]
}

```

