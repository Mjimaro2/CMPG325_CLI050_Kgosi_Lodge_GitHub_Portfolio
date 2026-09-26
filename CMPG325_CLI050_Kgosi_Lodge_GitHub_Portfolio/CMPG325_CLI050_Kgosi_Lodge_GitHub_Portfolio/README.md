# CMPG 325 — CLI-050 Kgosi Lodge & Conference Centre Network

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![IPv4](https://img.shields.io/badge/IPv4-172.30.24.0%2F23-informational)
![Wireless](https://img.shields.io/badge/Wireless-WPA2--PSK%20%2B%20AES-success)

## Project Overview

This repository documents the individual CMPG 325 Computer Networks semester project for **Kgosi Lodge & Conference Centre (Mahikeng)**.

**Student:** MASEKE, KJ  
**Student Number:** 37890123  
**Project ID:** CMPG325-2026-050  
**Client ID:** CLI-050  
**Industry:** Hospitality

The solution is implemented in Cisco Packet Tracer and uses the assigned **172.30.24.0/23** IPv4 block. It addresses the required 40% growth consideration, WPA2-PSK wireless security hardening, and the client change request for Guest Wi-Fi isolated from internal resources.

## Client Requirements

- Cisco Packet Tracer implementation.
- Address block: `172.30.24.0/23`.
- Appropriate connectivity and network services.
- Expected 40% user growth within three years.
- WPA2-PSK wireless security hardening.
- Guest Wi-Fi for visitors.
- Guest Wi-Fi isolated from internal resources.
- Working and testable Packet Tracer implementation.
- GitHub evidence covering design, addressing, configuration, testing, troubleshooting and reflection.

## Architecture

The design uses **VLAN segmentation**, **802.1Q trunking**, **router-on-a-stick inter-VLAN routing**, DHCP, DNS, HTTP, NAT/PAT, SSH and an extended ACL for Guest isolation.

### VLAN and IPv4 plan

| VLAN | Purpose | Network | Mask | Gateway |
|---:|---|---|---|---|
| 10 | Management | 172.30.24.0/26 | 255.255.255.192 | 172.30.24.1 |
| 20 | Staff | 172.30.24.64/26 | 255.255.255.192 | 172.30.24.65 |
| 30 | Servers | 172.30.24.128/27 | 255.255.255.224 | 172.30.24.129 |
| 40 | IoT | 172.30.24.160/27 | 255.255.255.224 | 172.30.24.161 |
| 50 | Guest Wi-Fi | 172.30.24.192/26 | 255.255.255.192 | 172.30.24.193 |
| 99 | Network Management | 172.30.25.0/27 | 255.255.255.224 | 172.30.25.1 |

Reserved address space remains available for future users and services.

## Topology

```text
                         INTERNET
                            |
                         ISP-R1
                            |
                      203.0.113.0/30
                            |
                        Kgosi-R1
                    Router-on-a-Stick
                            |
                       802.1Q TRUNK
                            |
                        SW1-CORE
                     /      |                           /       |                      WEB-DNS    AP-STAFF   AP-GUEST
              VLAN 30     VLAN 20    VLAN 50
                         /                                 Staff Wi-Fi     Guest Wi-Fi
                         |
                      802.1Q
                       TRUNK
                         |
                     SW2-ACCESS
                  /       |                      Staff   Management   IoT
              VLAN 20    VLAN 10   VLAN 40
```

## Security Design

### Wireless

Both wireless networks use:

- WPA2-PSK
- AES
- Separate SSIDs
- Separate VLANs

**Staff SSID:** `Kgosi-Staff`  
**Guest SSID:** `Kgosi-Guest`

### Guest isolation

Guest traffic is placed in VLAN 50 and filtered by the `GUEST-ISOLATION` ACL.

| Traffic | Expected |
|---|---|
| Guest → Gateway | Allowed |
| Guest → Internet | Allowed |
| Guest → Management | Blocked |
| Guest → Staff | Blocked |
| Guest → Servers | Blocked |
| Guest → IoT | Blocked |
| Guest → Network Management | Blocked |

## Services

- DHCP on Kgosi-R1
- Internal DNS on WEB-DNS-SERVER
- Internal HTTP server
- Simulated Internet HTTP server
- NAT/PAT for outbound traffic
- SSH administration
- Switch management VLAN

## Infrastructure Addressing

| Device | Interface | IPv4 | Mask | Gateway |
|---|---|---|---|---|
| Kgosi-R1 | G0/0.10 | 172.30.24.1 | /26 | — |
| Kgosi-R1 | G0/0.20 | 172.30.24.65 | /26 | — |
| Kgosi-R1 | G0/0.30 | 172.30.24.129 | /27 | — |
| Kgosi-R1 | G0/0.40 | 172.30.24.161 | /27 | — |
| Kgosi-R1 | G0/0.50 | 172.30.24.193 | /26 | — |
| Kgosi-R1 | G0/0.99 | 172.30.25.1 | /27 | — |
| Kgosi-R1 | G0/1 | 203.0.113.2 | /30 | 203.0.113.1 |
| ISP-R1 | G0/0 | 203.0.113.1 | /30 | — |
| ISP-R1 | G0/1 | 198.51.100.1 | /24 | — |
| SW1-CORE | VLAN 99 | 172.30.25.2 | /27 | 172.30.25.1 |
| SW2-ACCESS | VLAN 99 | 172.30.25.3 | /27 | 172.30.25.1 |
| WEB-DNS-SERVER | Fa0 | 172.30.24.130 | /27 | 172.30.24.129 |
| INTERNET-SERVER | Fa0 | 198.51.100.10 | /24 | 198.51.100.1 |

## Testing

Required evidence should demonstrate:

1. VLAN configuration.
2. Trunk configuration.
3. Router subinterfaces.
4. DHCP leases.
5. Staff → internal server.
6. Staff → Internet.
7. DNS resolution.
8. NAT translations.
9. Guest DHCP.
10. Guest → internal server **blocked**.
11. Guest → internal networks **blocked**.
12. Guest → Internet **allowed**.
13. WPA2-PSK/AES on both APs.
14. SSH administration.
15. Routing table.

## Evidence

Place screenshots from the student's own Packet Tracer implementation in `Evidence/`.

Recommended names:

```text
01-topology.png
02-ip-addressing-plan.png
03-sw1-vlan-configuration.png
04-sw2-vlan-configuration.png
05-trunk-configuration.png
06-router-interfaces.png
07-dhcp-bindings.png
08-internal-server.png
09-dns-test.png
10-internet-test.png
11-nat-translations.png
12-guest-ip-address.png
13-guest-internal-blocked.png
14-guest-internet-success.png
15-guest-acl.png
16-staff-wpa2.png
17-guest-wpa2.png
18-ssh-login.png
19-routing-table.png
```

## Repository Structure

```text
CMPG325_CLI050_Kgosi_Lodge_GitHub_Portfolio/
├── README.md
├── Packet-Tracer/
│   └── CMPG325-2026-050_CLI-050_Kgosi-Lodge.pkt
├── Configuration/
│   ├── Kgosi-R1-config.txt
│   ├── ISP-R1-config.txt
│   ├── SW1-CORE-config.txt
│   ├── SW2-ACCESS-config.txt
│   └── Wireless-configuration.txt
├── Documentation/
│   ├── Project-Report.md
│   ├── IP-Addressing-Plan.csv
│   ├── Test-Plan.md
│   └── Troubleshooting.md
├── Evidence/
├── Topology/
│   └── topology.mmd
└── Video/
    └── Demonstration-Script.md
```

## Academic Integrity

The final `.pkt`, screenshots, testing results and reflection should come from the student's own Packet Tracer implementation. Do not submit another student's configuration or evidence as your own.

## Author

**MASEKE, KJ**  
CMPG 325 — Computer Networks  
Project ID: CMPG325-2026-050  
Client ID: CLI-050  
Kgosi Lodge & Conference Centre (Mahikeng)
