# Project Report

## Introduction
This project designs and simulates a secure network for Kgosi Lodge & Conference Centre (Mahikeng) using Cisco Packet Tracer.

## Requirements
The design uses 172.30.24.0/23, supports expected 40% growth, implements WPA2-PSK wireless security, and provides Guest Wi-Fi isolated from internal resources.

## Design
VLANs 10, 20, 30, 40, 50 and 99 separate management, staff, servers, IoT, guests and network management. Router-on-a-stick provides inter-VLAN routing.

## Security
WPA2-PSK/AES protects both SSIDs. Guest VLAN 50 is filtered by an extended ACL. SSH is used for device administration and unused switch ports are disabled.

## Services
DHCP, DNS, HTTP and NAT/PAT are included.

## Testing
Testing covers VLANs, trunks, routing, DHCP, DNS, NAT, wireless security, guest isolation and end-to-end connectivity.

## Conclusion
The design addresses the stated client requirements while retaining address space for growth. Final conclusions must be supported by the student's own test evidence.
