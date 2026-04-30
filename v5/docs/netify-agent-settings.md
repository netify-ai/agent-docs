# Agent Settings - Netify Agent v5

> [!TIP]
> This is an AI-optimized version of our documentation. 
> For the full human experience, including examples and optimizations, 
> view the **[Agent Settings Guide](https://www.netify.ai/documentation/agent/v5/configuration/settings)** on our website.
>
> **Source:** [Netify Documentation](https://www.netify.ai/documentation/agent/v5/configuration/settings)
> **Target Path:** /etc/netifyd.conf
> **Format:** INI / Plain Text

Agent settings in Netify v5 control the core behavior of the DPI engine, resource allocation, flow tracking, and performance tuning.

## Netifyd Main
The main configuration file - netifyd.conf - is generally used for mapping paths to the underlying operating system.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| profile | string | /etc/netifyd/profiles.d/00-default.conf | Specifies the configuration profile to load. |
| path_state_volatile | string | /var/run/netifyd | Specifies the directory for storing volatile data (ephemeral files). This value can be referenced in other configuration properties using the ${path_state_volatile} variable. |
| path_state_persistent | string | /etc/netifyd | Specifies the directory for storing persistent data. This value can be referenced in other configuration properties using the ${path_state_persistent} variable. |
| path_pid_file | string | ${path_state_volatile}/netifyd.pid | Specifies the path to the process ID (PID) file. |
| path_shared_data | string | /usr/share/netifyd | Specifies the path to shared data files. |
| path_license_manager | string | ${path_plugin_libdir}/libnetify-plm.so | Specifies the path to the license manager library. |
| path_server_socket | string | ${path_state_volatile}/netifyd.sock | Specifies the local Netify Agent API socket path. |
| path_uuid_serial | string | - | Specifies the path to a script that returns a unique custom serial for the agent. |
| auto_informatics | boolean-string | no | Specifies whether to enable automatic Netify Informatics integration. This option is intended to be managed exclusively by the --enable/disable-informatics command-line parameters. |

## Netifyd
Main environment configuration for the Netify daemon, controlling core behavior, resource allocation, and general tuning parameters.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| user | string | - | Specifies user to switch to on startup. |
| group | string | - | Specifies group to switch to on startup. |
| auto_flow_expiry | boolean-string | yes | Specifies the shutdown behavior for active flows. Enabling this forces an immediate expiration and purge; otherwise, the agent lingers until the flow cache is naturally cleared or a manual override (second termination signal) is triggered. |
| enable_coredumps | string | no | Specifies whether core dump files are saved when the agent or one of its plugins terminates unexpectedly. This should be set to no on production systems. |
| flow_map_buckets | integer | 128 | Specifies the number of buckets into which the main flow map is divided. The default is adequate for up to 5,000 flows. Increasing this value reduces the chances of flow map lock contention on systems that track a large number of flows. |
| max_capture_length | integer | 65535 | Specifies the maximum number of bytes to capture (copy) per packet. Reducing this may be appropriate for embedded systems, but reducing it too much will result in less accurate application/protocol detection. The maximum value is 65535, which is also the default. |
| max_detection_pkts | integer | 32 | Specifies the maximum number of packets to inspect per flow. This is a performance tuning option for embedded systems. Reducing this value too much will result in less accurate application/protocol detection, specifically TLS. Generally, a safe range for adequate detection accuracy is between 15 to 25 packets. |
| max_flows | integer | 0 | Specifies the maximum number of flows to track at any given moment. When this value is reached, new flows will stop being tracked until old flows expire. This option can be used to conserve memory on embedded systems or to set an upper safety limit to guard against DDoS attacks or network scanning tools. A value of 0 disables the maximum. |
| soft_dissectors | boolean-string | yes | Specifies whether to enable soft-dissectors, which are flow expressions defined in the application signatures file. For debugging or on embedded systems with very limited resources, it may be helpful to disable this feature. |
| syn_scan_protection | boolean-string | no | Specifies whether to enable SYN scan protection. When enabled, the agent will not track TCP flows until a SYN+ACK has been captured. This option can offer protection against network scanners and has the additional benefit of not tracking already established TCP flows when the Agent is first started. |
| ttl_idle_flow | integer | 30 | Specifies the time-to-live (TTL), in seconds, before an idle non-TCP flow is scheduled for expiry. |
| ttl_idle_tcp_flow | integer | 300 | Specifies the time-to-live (TTL), in seconds, before an idle TCP flow is scheduled for expiry. |
| update_interval | integer | 15 | Specifies how often (in seconds) to process the global flow maps. Flow statistics are made available, idle flows are expired, and other housekeeping is performed during this update period. The default of 15 seconds is appropriate in most cases. |
| use_getifaddrs | boolean-string | no | Specifies whether to periodically call getifaddrs(3) to update the associated IP addresses of each capture source, where applicable. This option is primarily intended for non-Linux systems where an on-demand system like Netlink is not available. This setting should be enabled for FreeBSD and variants. |

## Capture Defaults
Default settings for network interface packet capture operations and read timeouts.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| capture_type | string | pcap | Specifies the default capture method for -I (--internal) and -E (--external) command-line options. |
| read_timeout | integer | 500 | Specifies the packet capture timeout value (milliseconds). This is how long reads from PCAP or TPv3 capture sources will wait for packet data before being cancelled and retried. The default value of 500 ms is appropriate in almost all cases. |

## Threads
Configuration for multi-threading allocation across packet capture and flow detection processes.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| capture_base | integer | - | Specifies the base CPU ID from which to start capture threads. |
| detection_base | integer | - | Specifies the base CPU ID from which to start detection threads. |
| detection_cores | integer | - | Specifies the number of detection cores to start. |

## Flow Hash Cache
Settings for the flow hash cache, optimizing performance by persisting metadata for idle network flows.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| enable | boolean-string | yes | Specifies whether the flow hash cache is enabled. |
| save | string | persistent | Specifies the flow hash cache save policy. |
| cache_size | integer | 1000 | Specifies the maximum size of the flow hash cache (in bytes). |

## Dns Hint Cache
Configuration for the DNS Hint Cache (DHC), improving application flow detection accuracy when standard protocol metadata is missing.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| enable | boolean-string | yes | Specifies whether DNS hint caching is enabled. |
| save | string | persistent | Specifies the DNS hint cache persistence policy for restarts. |
| cache_size | integer | 1000 | Specifies the maximum size of the DNS hint cache (in bytes). |
| partial_lookups | boolean-string | no | Specifies whether the Netify agent applies DNS cache hinting only when a hostname is not extracted from the protocol. Setting this field to yes typically results in slightly lower application classification rates. Unknown applications that use a Content Delivery Network (Cloudflare, Fastly) to deliver content will no longer be classified as the CDN. The potential upside is fewer false positives due to shared IP usage across applications. |

## Netify API
Settings for connecting to the Netify Cloud API for signature updates, network intelligence, device discovery and informatics features.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| enable | boolean-string | no | Specifies whether the Netify API is enabled. By default, the API is disabled and will not connect to any resource outside of your network. |
| update_tick | integer | 30 | Specifies the number of seconds between API check-ins. |
| update_interval | integer | 86400 | Specifies the number of seconds between API updates. An API update checks for things like a new application signature file. |
| tls_verify | boolean-string | yes | Specifies whether to validate the API TLS certificate. This should always be set to true, except possibly in developer or CI/CD environments. |
| vendor | string | - | Specifies the vendor code. |

## Protocols
Policies for managing and filtering which network protocols are actively inspected by the engine.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| all | string | include | Specifies which protocols are enabled. This can be an effective way to manage CPU resources. |


## Base Template - /etc/netifyd.conf
```
profile = /etc/netifyd/profiles.d/00-default.conf
path_state_volatile = /var/run/netifyd
path_state_persistent = /etc/netifyd
path_pid_file = ${path_state_volatile}/netifyd.pid
path_shared_data = /usr/share/netifyd
path_license_manager = ${path_plugin_libdir}/libnetify-plm.so
path_server_socket = ${path_state_volatile}/netifyd.sock
# path_uuid_serial = 
auto_informatics = no
```


## Policy Template - /etc/netify/profiles.d/00-default.conf

```ini
[netifyd]
auto_flow_expiry = yes
flow_map_buckets = 128
max_capture_length = 65535
max_detection_pkts = 32
max_flows = 0
soft_dissectors = yes
syn_scan_protection = no
ttl_idle_flow = 30
ttl_idle_tcp_flow = 300
update_interval = 15
use_getifaddrs = false

[capture-defaults]
capture_type = pcap
read_timeout = 500

[threads]
#capture_base = 0
#detection_base = 8
#detection_cores = 8

[flow-hash-cache]
enable = yes
save = persistent
cache_size = 1000

[dns-hint-cache]
enable = yes
partial_lookups = no
save = persistent
cache_size = 1000

[privacy-filter]
private_external_addresses = no

[netify-api]
enable = yes
update_tick = 30
update_interval = 86400
tls_verify = yes

[protocols]
all = include

[netlink]
buffer_size = 32768
conntrack_buffer_size = 1048576
conntrack_idle_timeout = 3600
bridge_pvid_discovery = no
```
