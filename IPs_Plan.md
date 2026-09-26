# IP Addressing Plan

This plan uses the IPv4 site blocks already selected in the README and divides them into VLAN subnets. Each site's allocation is contiguous so it can be summarized for inter-site routing. IPv6 uses the simulated enterprise prefix `2001:db8:aaa::/48`, with one `/52` per site and a `/64` per VLAN. 

## Addressing principles

- Use RFC 1918 private IPv4 space `172.16.0.0/16` for internal networks. This is a private-use block, not a public “Class B” allocation.
- Keep each site's VLANs inside its assigned contiguous block: HQ `172.16.0.0/22`, R&D `172.16.4.0/22`, Sales `172.16.8.0/23`. This supports straightforward route summaries (`/22`, `/22`, `/23`) and leaves the rest of `172.16.0.0/16` available for growth.
- Give user, voice, server, management, and guest devices separate VLANs to contain broadcast traffic and apply distinct security policies. Guest access should be filtered from internal networks at the firewall.
- Reserve the first usable IPv4 address in each VLAN as the shared virtual default gateway (HSRP). Use the next addresses for the two routers, then DHCP for clients; keep infrastructure addresses out of dynamic pools. The gateway convention is `.1` virtual, `.2` and `.3` router addresses.
- Use IPv6 `/64` per VLAN, the standard size for ordinary IPv6 LANs. The `::1` address is the shared virtual gateway; configure the routers with distinct addresses such as `::2` and `::3`. Use SLAAC and/or DHCPv6 as appropriate.
- Masks and usable ranges below account for the gateway and leave remaining addresses for infrastructure and clients. In IPv4, `/23` provides 510 usable addresses and `/24` provides 254.

## Headquarters — `172.16.0.0/22`, `2001:db8:aaa:1000::/52`

The `/22` contains four `/24` networks (1,022 usable addresses total), matching the README's estimate of roughly 900–1,000 hosts while allowing some growth. Four VLANs are shown as a practical starting point; departments can be split further if the topology requires it, using remaining space in the site block.

| VLAN / purpose | IPv4 subnet | Usable IPv4 range | Virtual gateway | IPv6 subnet | IPv6 virtual gateway |
| --- | --- | --- | --- | --- | --- |
| VLAN 10 — Users / departments | `172.16.0.0/23` | `172.16.0.1–172.16.1.254` | `172.16.0.1` | `2001:db8:aaa:1010::/64` | `2001:db8:aaa:1010::1` |
| VLAN 20 — Voice | `172.16.2.0/24` | `172.16.2.1–172.16.2.254` | `172.16.2.1` | `2001:db8:aaa:1020::/64` | `2001:db8:aaa:1020::1` |
| VLAN 30 — Servers / services | `172.16.3.0/25` | `172.16.3.1–172.16.3.126` | `172.16.3.1` | `2001:db8:aaa:1030::/64` | `2001:db8:aaa:1030::1` |
| VLAN 40 — Network management | `172.16.3.128/26` | `172.16.3.129–172.16.3.190` | `172.16.3.129` | `2001:db8:aaa:1040::/64` | `2001:db8:aaa:1040::1` |
| VLAN 50 — Guest Wi-Fi | `172.16.3.192/26` | `172.16.3.193–172.16.3.254` | `172.16.3.193` | `2001:db8:aaa:1050::/64` | `2001:db8:aaa:1050::1` |

The user VLAN gets the largest subnet because it carries the bulk of employee endpoints (the README assumes three devices per employee). Voice, servers, management, and guest networks have separate smaller IPv4 pools, with room to enlarge or subdivide the remaining address plan as actual device counts become known.

## R&D Office — `172.16.4.0/22`, `2001:db8:aaa:2000::/52`

R&D is estimated at 600–700 hosts. Its `/22` provides 1,022 usable addresses and aligns with the README's site allocation. The large user pool accommodates employee devices; the other VLANs separate voice, lab/server systems, management, and visitors.

| VLAN / purpose | IPv4 subnet | Usable IPv4 range | Virtual gateway | IPv6 subnet | IPv6 virtual gateway |
| --- | --- | --- | --- | --- | --- |
| VLAN 10 — Users / departments | `172.16.4.0/23` | `172.16.4.1–172.16.5.254` | `172.16.4.1` | `2001:db8:aaa:2010::/64` | `2001:db8:aaa:2010::1` |
| VLAN 20 — Voice | `172.16.6.0/24` | `172.16.6.1–172.16.6.254` | `172.16.6.1` | `2001:db8:aaa:2020::/64` | `2001:db8:aaa:2020::1` |
| VLAN 30 — R&D lab / servers | `172.16.7.0/25` | `172.16.7.1–172.16.7.126` | `172.16.7.1` | `2001:db8:aaa:2030::/64` | `2001:db8:aaa:2030::1` |
| VLAN 40 — Network management | `172.16.7.128/26` | `172.16.7.129–172.16.7.190` | `172.16.7.129` | `2001:db8:aaa:2040::/64` | `2001:db8:aaa:2040::1` |
| VLAN 50 — Guest Wi-Fi | `172.16.7.192/26` | `172.16.7.193–172.16.7.254` | `172.16.7.193` | `2001:db8:aaa:2050::/64` | `2001:db8:aaa:2050::1` |

Lab systems are separated from employee endpoints so access to research resources can be controlled with firewall or inter-VLAN ACL policy. Restrict management access to authorized administrator devices, and allow guest clients Internet access only.

## Sales Office — `172.16.8.0/23`, `2001:db8:aaa:3000::/52`

The `/23` provides 510 usable IPv4 addresses, within the README's 300–400 host estimate with capacity for growth. It is split into two `/24` networks for users and supporting services.

| VLAN / purpose | IPv4 subnet | Usable IPv4 range | Virtual gateway | IPv6 subnet | IPv6 virtual gateway |
| --- | --- | --- | --- | --- | --- |
| VLAN 10 — Users / departments | `172.16.8.0/24` | `172.16.8.1–172.16.8.254` | `172.16.8.1` | `2001:db8:aaa:3010::/64` | `2001:db8:aaa:3010::1` |
| VLAN 20 — Voice | `172.16.9.0/25` | `172.16.9.1–172.16.9.126` | `172.16.9.1` | `2001:db8:aaa:3020::/64` | `2001:db8:aaa:3020::1` |
| VLAN 30 — Network management / infrastructure | `172.16.9.128/26` | `172.16.9.129–172.16.9.190` | `172.16.9.129` | `2001:db8:aaa:3030::/64` | `2001:db8:aaa:3030::1` |
| VLAN 40 — Guest Wi-Fi | `172.16.9.192/26` | `172.16.9.193–172.16.9.254` | `172.16.9.193` | `2001:db8:aaa:3040::/64` | `2001:db8:aaa:3040::1` |

Sales users receive the largest pool because they account for most endpoints. The remaining IPv4 space is divided among voice, infrastructure, and guest access. If a separate server or printer VLAN is needed, reallocate or subdivide these pools after confirming device counts.

## Routed links and VPN addressing

Keep point-to-point and transit links outside the site LAN summaries, in a separate reserved range such as `172.16.254.0/24`. A `/30` is sufficient for a legacy IPv4 point-to-point link (two usable addresses); `/31` is also an option if all equipment supports it. For example:

| Link | IPv4 subnet | Endpoint addresses |
| --- | --- | --- |
| HQ–Sales fiber transit | `172.16.254.0/30` | HQ `172.16.254.1`, Sales `172.16.254.2` |
| HQ–R&D VPN routed interface | `172.16.254.4/30` | HQ `172.16.254.5`, R&D `172.16.254.6` |
| Sales–R&D VPN routed interface | `172.16.254.8/30` | Sales `172.16.254.9`, R&D `172.16.254.10` |

These are example internal tunnel/transit addresses; public VPN peer addresses must come from the ISP. Redundant VPN tunnels need additional unique transit subnets and distinct WAN paths/peers as the topology and equipment support. Do not reuse one subnet on multiple routed links.

For IPv6 point-to-point links, allocate a unique `/64` from a separately reserved portion of the enterprise `/48` (or use `/127` where supported and appropriate), and assign distinct addresses to each endpoint. Site `/52` blocks above are reserved for site LAN prefixes, so do not overlap them with WAN links.

## IPv6 allocation and interoperability

The README described the IPv6 site allocations as `2001:db8:aaa:1::/52`, `...:2::/52`, and `...:3::/52`. A `/52` boundary advances in increments of `0x10` in the fourth hextet, so this plan uses correctly aligned site blocks `2001:db8:aaa:1000::/52`, `2001:db8:aaa:2000::/52`, and `2001:db8:aaa:3000::/52`. This also avoids the invalid shorthand `2001:db8:aaa:3/52` in the README. Each `/52` contains 256 `/64` VLAN prefixes, leaving room for future floors, departments, and services.

Deploy dual stack on clients, gateways, servers, and routed links so IPv4 and IPv6 operate side by side. Use DNS with both A and AAAA records where services support both protocols. The scenario requests an interoperability mechanism: if IPv6-only clients must reach IPv4-only services, provide NAT64 with DNS64 at a controlled boundary; otherwise dual stack is sufficient for the described network and no translation is needed. Apply equivalent firewall and segmentation policies to both address families.

## Implementation notes

- Configure HSRP virtual gateways consistently with the table; router interface addresses `.2` and `.3` (or their IPv6 equivalents) are examples for the two participating routers.
- Exclude gateway, router, switch, AP, printer, server, and other statically assigned addresses from DHCP scopes. The listed usable ranges include these addresses; reserve them operationally.
- Use DHCP scopes for IPv4 clients. For IPv6, use SLAAC or DHCPv6 according to endpoint and policy needs.
- The tables are a logical allocation proposal. Confirm actual host counts, VLAN assignments, DHCP reservations, and firewall rules during implementation. Avoid assigning the same subnet to separate floors if they are routed as separate VLANs.
