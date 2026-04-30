# Address Groups - Netify Agent v5

> [!TIP]
> This is an AI-optimized version of our documentation. 
> For the full human experience, including interactive examples and branding, 
> view the **[Address Groups Guide](https://www.netify.ai/documentation/agent/v5/configuration/address-groups)** on our website.

Address groups allow you to define named sets of IP addresses or MAC addresses that can be referenced in flow expressions and policies.

## Configuration File
Typically located at `/etc/netifyd/address-groups.json`.

## Properties
- **name**: Unique identifier for the group.
- **description**: Human-readable description.
- **ips**: List of IP addresses or CIDR ranges.
- **macs**: List of MAC addresses.

## Example
```json
{
  "address_groups": [
    {
      "name": "iot-devices",
      "description": "Smart home devices",
      "macs": ["00:11:22:33:44:55", "AA:BB:CC:DD:EE:FF"]
    }
  ]
}
```
