# Expression Engine - Netify Agent v5

> [!TIP]
> This is an AI-optimized version of our documentation. 
> For the full human experience, including interactive examples and branding, 
> view the **[Expression Engine Guide](https://www.netify.ai/documentation/agent/v5/integrations/expression-engine)** on our website.

The expression engine is a powerful matching and filtering system used throughout the Netify Agent to define network policies (Flow Actions) and Network Intelligence rules. 

There are two syntax modes: Compact Syntax and Standard Syntax.

## 1. Compact Syntax
Optimized for simple matching of applications, protocols, and categories. 
- You can invert any rule using the logical NOT operator `!`.

### Prefixes
- `ai:` - Application ID or Tag (e.g., `ai:facebook`, `ai:119`)
- `pi:` - Protocol ID or Tag (e.g., `pi:dns`, `pi:5`)
- `ac:` - Application Category ID or Tag (e.g., `ac:social-media`, `ac:2`)
- `pc:` - Protocol Category ID or Tag (e.g., `pc:database`, `pc:2`)

**Example:**
`"criteria": "ac:social-media"` (Matches all social media traffic)
`"criteria": "!ai:youtube"` (Matches everything except YouTube)

## 2. Standard Syntax
Full-featured syntax for evaluating complex flow attributes.
- **Rule 1:** Each expression must be terminated with a semicolon (`;`).
- **Rule 2:** String values must be enclosed in single quotes (`'`).
- **Rule 3:** Lists (arrays) of expressions act as an implicit `OR` statement.
- **Rule 4:** `*` can be used to match all flows.

### Operators
- **Comparison:** `==` (Equal), `!=` (Not Equal), `>`, `>=`, `<`, `<=`
- **Boolean:** `!` (Not), `&&` or `and` (And), `||` or `or` (Or)
- **Regex:** Use the `rx:` prefix inside single quotes (e.g., `detected_hostname == 'rx:.*xxx';`)
- **Grouping:** Use parentheses `()` to override precedence.

**Examples:**
```json
"criteria": "application == 'youtube';"
"criteria": "(application == 'youtube' || protocol == 'icmp') && local_ip == '192.168.1.10';"
"criteria": "other_type == other_remote;"
```

### Implicit OR Example (Array Format)
```json
"criteria": [
    "application == 'youtube';",
    "category == 'adult';"
]
```

## 3. Data Types
- **address**: IP, CIDR, MAC, or Address Group (e.g., `192.168.1.1`, `10.0.0.0/8`, `00:11:22...`, `@iot`)
- **string**: Must be in single quotes (e.g., `'spotify'`)
- **integer**: Numeric (e.g., `5`, `80`)
- **hex**: Hexadecimal (e.g., `0x0303`)
- **boolean**: Flag variable that is either true/false by its presence or `!` negation (e.g., `detection_complete`, `!ip_nat`)
- **constant**: Specific enum constants without quotes (e.g., `other_remote`, `origin_local`)

## 4. Available Attributes

| Attribute Name | Type | Description |
|----------------|------|-------------|
| `application` (aliases: `app`, `app_id`) | mixed | Application tag or ID |
| `application_category` (aliases: `app_category`) | string | Application category tag |
| `category` (aliases: `cat`) | string | Broad category tag (app, domain, overlay, etc.) |
| `protocol` (aliases: `proto`, `proto_id`) | mixed | Protocol tag or ID |
| `protocol_category` (aliases: `proto_category`) | string | Protocol category tag |
| `detected_hostname` | string | Extracted hostname |
| `dns_hostname` | string | Hostname from DNS query |
| `local_ip` / `other_ip` | address | Local or Remote IP address |
| `local_mac` / `other_mac` | address | Local or Remote MAC address |
| `local_port` / `other_port` | integer | Local or Remote Port |
| `ip_protocol` | integer | IP protocol ID (e.g., 6 for TCP) |
| `other_type` | constant | E.g., `other_local`, `other_remote`, `other_multicast` |
| `origin` | constant | E.g., `origin_local`, `origin_other` |
| `vlan_id` | integer | VLAN ID |
| `tls_version` | hex | TLS version (e.g., `0x0303`) |
| `ip_nat` | boolean | Set if flow is found in CT table on WAN |
| `detection_complete` | boolean | Set if classification is finished |
