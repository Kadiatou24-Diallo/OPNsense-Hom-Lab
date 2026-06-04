# Lab 03 - Network Connectivity Testing

## Objective
Verify connectivity between Kali Linux, OPNsense, and the Internet.

## Environment
- VMware Workstation
- OPNsense Firewall
- Kali Linux
- VMnet1 (LAN)
- VMnet8 (WAN)

## Tests Performed

### Test 1 - Connectivity to OPNsense

Command:

```bash
ping 192.168.105.2
```

Result:
- Successful replies received
- 0% packet loss
- Connectivity to the firewall verified

### Test 2 - Connectivity to the Internet

Command:

```bash
ping 8.8.8.8 -c 4
```

Result:
- Successful replies received
- 0% packet loss
- Internet connectivity verified

## Network Information

### OPNsense
- LAN IP: 192.168.105.2

### Kali Linux
- IP Address: 192.168.105.131

## Results

| Test | Status |
|--------|--------|
| Kali → OPNsense | ✅ Success |
| Kali → Internet | ✅ Success |

## Skills Learned
- ICMP testing
- Network troubleshooting
- Connectivity verification
- Firewall validation

## Status
✅ Completed
