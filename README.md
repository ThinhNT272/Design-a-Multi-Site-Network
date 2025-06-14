# Design-a-Multi-Site-Network

You have been hired as a network engineer to design a comprehensive network infrastructure for CMC Holdings, a mid-sized company with three buildings located in two different cities:
- Headquarters (HQ): 10 floors, Hanoi.
- R&D Office: 5 floors, Da Nang.
- Sales Office: 3 floors, also in Hanoi, 3km from HQ. 

The company has around 600 employees, with about 300 in HQ, 200 in R&D, and 100 in the Sales office. Each floor has multiple departments requiring internet access, internal communication, and high availability.

## Task Overview:

Your are responsible for designing the network infrastructure, including:
- LAN setup for each building.
- Inter-building connectivity.
- VPN configuration.
- IPv4/IPv6 addressing.
- Security & scalability considerations.

## Requirements:

1. Network Architecture
- Draw a logical network topology diagram for each site and the overall WAN.
- Include core, distribution, and access layers if used.
- Specify subnetting strategy (IPv4 and IPv6) and how subnets are distributed across departments.

2. VPN and WAN Connectivity
- Propose a site-to-site VPN solution to securely connect the three buildings over the internet.
- Configure redundant VPN tunnels using IPSec/IKEv2 or OpenVPN.
- Propose how to handle remote access VPN for employees working from home.

3. IPv4/IPv6 Interoperability
- Allocate IPv4 private addressing using a well-defined VLSM scheme.
- Assign IPv6 global unicast addresses using a simulated prefix.
- Explain how dual-stack configuration is used to support both IPv4 and IPv6.
- Specify mechanisms for interoperability (e.g., NAT64, DNS64, or tunneling if required).

4. Security and Management
- Include firewall placement and access control policies.
- Propose network monitoring and logging solutions.
- Suggest VLAN structure for isolating different departments (e.g., HR, Dev, Sales, Guest).

## Deliverables:
A detailed report including:
- Network diagrams (logical and physical)
- IP addressing plan (both IPv4 and IPv6)
- VPN design and configuration outline
- Security, monitoring, and expansion recommendations