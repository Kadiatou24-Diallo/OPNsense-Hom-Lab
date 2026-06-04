# Network Topology

## Overview
This document describes the network architecture of the OPNsense home lab.

## Network Architecture

Internet
    │
    ▼
VMnet8 (WAN)
    │
    ▼
OPNsense Firewall
    │
    ▼
VMnet1 (LAN)
    │
    ▼
Kali Linux

## Components

### OPNsense
- Firewall and router
- DHCP server
- Network monitoring

### WAN Interface
- Connected to VMware NAT network (VMnet8)
- Provides Internet access

### LAN Interface
- Connected to VMware Host-only network (VMnet1)
- Provides internal network connectivity

### Kali Linux
- Connected to VMnet1
- Receives IP address from OPNsense DHCP
- Used for security testing and network analysis

## Objectives
- Learn firewall administration
- Practice network segmentation
- Monitor network traffic
- Build hands-on cybersecurity skills

## Status
✅ Operational
