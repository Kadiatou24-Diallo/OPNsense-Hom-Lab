
# Lab 02 - Kali Linux Connectivity Through OPNsense

## Objective
Connect Kali Linux to the OPNsense LAN and verify network connectivity.

## Environment
- VMware Workstation
- OPNsense Firewall
- Kali Linux
- VMnet1 (LAN)
- VMnet8 (WAN)

## Tasks Performed
1. Connected Kali Linux to VMnet1.
2. Verified network adapter configuration.
3. Obtained an IP address from OPNsense DHCP.
4. Verified communication with the OPNsense firewall.
5. Verified Internet connectivity through OPNsense.

## Network Information

### OPNsense LAN
- Network: 192.168.105.0/24

### Kali Linux
- Interface: eth0
- IP Address: 192.168.105.131

## Results
- Kali Linux successfully connected to the LAN.
- DHCP assignment successful.
- OPNsense detected the client.
- Internet access operational.

## Skills Learned
- VMware virtual networking
- DHCP configuration
- Firewall client connectivity
- Network troubleshooting

## Status
✅ Completed
