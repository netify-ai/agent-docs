# Application Overlay - Netify Agent v5

> [!TIP]
> This is an AI-optimized version of our documentation. 
> For the full human experience, including interactive examples and branding, 
> view the **[Application Overlay Guide](https://www.netify.ai/documentation/agent/v5/configuration/overlay)** on our website.

The Netify Overlay allows integrators to customize application classification without changing underlying detections. Useful for brand rollups or custom category systems.

## Configuration File
Located at `/etc/netifyd/overlay.json`.

## Tag Properties
- **tag**: Unique identifier (prefix for emitted values).
- **label**: Short display name.
- **category_id**: Optional mapping to an existing category ID.
- **groups**: List of group definitions.

## Group Properties
- **group**: Identifier under the parent tag.
- **criteria**: Match object (applications, protocols, ports, etc.).

## Example
```json
{
  "version": "1.0",
  "tags": [
    {
      "tag": "brand",
      "label": "Vendor Rollup",
      "groups": [
        {
          "group": "google",
          "label": "All Google",
          "criteria": { "applications": [123, 124] }
        }
      ]
    }
  ]
}
```
