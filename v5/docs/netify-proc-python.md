# Python Processor Plugin

## Python Processor: Overview
The Netify Python plugin enables direct access to live network telemetry from the Netify agent within Python scripts, making it an ideal platform for AI-driven analytics and intelligent network automation. By leveraging Python's extensive libraries for machine learning, data processing, and anomaly detection, this plugin allows developers to rapidly prototype custom AI models, predictive workflows, and advanced network intelligence features.

## Python Processor: Configuration
The plugin is configured via the JSON file referenced by its loader (typically `/etc/netifyd/netify-proc-python.json`). The configuration file provides a way to define debug mode and the location of the Python script.

## Python Processor: JSON Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://netify-ai.github.io/agent-docs/v5/schemas/netify-proc-python.json",
  "title": "Netify Python Processor Configuration",
  "description": "JSON Schema for the Netify Python Processor plugin configuration.",
  "type": "object",
  "properties": {
    "debug": {
      "type": "boolean",
      "description": "Specifies whether debug mode is enabled for the processor."
    },
    "script": {
      "type": "string",
      "description": "The file system path to the Python script."
    }
  },
  "required": [
    "debug",
    "script"
  ],
  "examples": [
    {
      "debug": false,
      "script": "${path_state_persistent}/netify-proc-python/netify-proc-python.python"
    }
  ]
}

```

