# Network Interfaces - Netify Agent v5

> [!TIP]
> This is an AI-optimized version of our documentation. 
> For the full human experience, including interactive examples and branding, 
> view the **[Network Interfaces Guide](https://www.netify.ai/documentation/agent/v5/configuration/network-interfaces)** on our website.

The Netify Agent requires configuration for each network interface it should listen on. Interfaces are typically categorized by their role: **LAN** (internal) or **WAN** (external).

## Interface Properties [capture-interface-NAME]

| Property | Type | Options | Description |
|----------|------|---------|-------------|
| capture_type | enum | pcap, tpv3, nfqueue | The packet capture method to use. |
| role | enum | lan, wan | The role of the interface. |
| address | array | - | List of IP addresses or ranges associated with this interface. |
| filter | string | - | BPF filter to apply to the capture. |
| conntrack_counters | boolean | - | Enable conntrack byte/packet counters. |
| fanout_mode | enum | hash, lb, cpu, rollover, random, qm | Packet fanout mode for multi-core processing. |

## Global Network Settings [netifyd]

These properties define how the agent identifies local vs remote traffic across all interfaces.

| Property | Type | Description |
|----------|------|-------------|
| lan_networks | array | List of IP ranges to be treated as local. |
| vlan_networks | array | List of VLAN IDs to be treated as local. |
| ignore_networks | array | List of IP ranges to ignore entirely. |
| ignore_macs | array | List of MAC addresses to ignore entirely. |
