# Lab 08: Firewall Logging and Log Analysis in OPNsense

## Lab Overview

This lab demonstrates how to monitor, filter, and analyze firewall events using the OPNsense Live View logging system. Firewall logs provide detailed information about network traffic processed by the firewall, including permitted and blocked connections, protocols, interfaces, source and destination IP addresses, and the firewall rule responsible for each event.

During this lab, real network traffic was generated and analyzed using the OPNsense Live View interface. Different filtering techniques were applied to investigate allowed and blocked traffic, as well as TCP and UDP communications. These techniques are commonly used by network administrators and SOC analysts to troubleshoot connectivity issues, monitor network activity, and investigate potential security incidents.

## Lab Objectives

After completing this lab, I was able to:

- Access the OPNsense Live View firewall logs.
- Understand the information displayed in each log entry.
- Differentiate between allowed (PASS) and blocked (BLOCK) traffic.
- Identify the firewall rule responsible for processing a packet.
- Filter firewall events by action and protocol.
- Analyze TCP and UDP traffic.
- Perform basic firewall log analysis similar to SOC analyst investigations.

  ## Prerequisites

Before performing this lab, the following requirements were met:

- OPNsense Firewall installed and configured.
- LAN and WAN interfaces configured correctly.
- Kali Linux machine connected to the LAN network.
- Ubuntu machine connected to the WAN network.
- Firewall rules configured from previous laboratories.
- Network connectivity verified between virtual machines.

## Environment

| Component | Details |
|-----------|---------|
| Firewall | OPNsense |
| Hypervisor | VMware Workstation Pro |
| Attacker Machine | Kali Linux |
| Client Machine | Ubuntu Linux |
| Host OS | Windows 11 |
| LAN Network | 192.168.105.0/24 |
| WAN Network | 192.168.152.0/24 |

## Network Topology

The laboratory uses the following network architecture:

```text
Internet
        │
Wi-Fi Router
        │
Windows Host
        │
VMnet8 (NAT)
        │
WAN (192.168.152.130)
     OPNsense
LAN (192.168.105.2)
        │
VMnet1 (Host-Only)
        │
Kali Linux (192.168.105.19)

Ubuntu Linux (192.168.152.132)
```

## Configuration Steps

### Step 1: Open Firewall Live View

Log in to the OPNsense web interface and navigate to:

```text
Firewall → Log Files → Live View
```

The Live View page displays firewall events in real time, allowing administrators to monitor and analyze network traffic processed by the firewall.

**Screenshot**

> Figure 1: OPNsense Firewall Live View.

![Firewall Live View](../screenshots/lab08/firewall-live-view.png)
### Step 2: Analyze Firewall Log Entries

Each firewall log entry contains important information that helps identify how the firewall processed a network packet.

The main fields include:

- **Interface** – Network interface where the packet was processed (LAN or WAN).
- **Time** – Timestamp indicating when the event occurred.
- **Protocol** – Network protocol such as TCP or UDP.
- **Source** – IP address that initiated the connection.
- **Destination** – Target IP address.
- **Action** – Indicates whether the packet was allowed (**PASS**) or blocked (**BLOCK**).
- **Label** – Firewall rule responsible for processing the packet.

These fields provide the information required to investigate network connectivity issues and security events.

**Screenshot**

> Figure 2: Firewall log entries displayed in Live View.

![Firewall Log Entries](../screenshots/lab08/firewall-log-entries.png)

### Step 3: Filter Allowed Traffic

To display only permitted traffic, the firewall logs were filtered using the following criteria:

| Field | Value |
|--------|-------|
| Action | PASS |

The filtered results displayed only packets that were successfully allowed by the firewall.

This filtering technique is commonly used to verify successful network communications and confirm that firewall rules are functioning correctly.

**Screenshot**

> Figure 3: Filtering firewall logs to display only allowed traffic.

![Allowed Traffic Filter](../screenshots/lab08/pass-filter.png)

### Step 4: Filter Blocked Traffic

To identify denied network connections, the firewall logs were filtered using the following criteria:

| Field | Value |
|--------|-------|
| Action | BLOCK |

The results showed packets that were denied by the firewall, including traffic blocked by the default WAN protection rules such as **Bogon Networks** and **Private Networks**.

Analyzing blocked events helps administrators identify unauthorized traffic, configuration issues, and potential security threats.

**Screenshot**

> Figure 4: Filtering firewall logs to display blocked traffic.

![Blocked Traffic Filter](../screenshots/lab08/block-filter.png)

### Step 5: Filter TCP Traffic

To analyze TCP communications, the firewall logs were filtered using:

| Field | Value |
|--------|-------|
| Protocol | TCP |

The filtered events displayed TCP packets processed by the firewall. Most entries corresponded to management traffic permitted by the **anti-lockout rule**, which protects administrator access to the OPNsense web interface.

**Screenshot**

> Figure 5: TCP traffic displayed in Live View.

![TCP Traffic Filter](../screenshots/lab08/tcp-filter.png)

### Step 6: Filter UDP Traffic

Finally, the firewall logs were filtered to display only UDP traffic.

| Field | Value |
|--------|-------|
| Protocol | UDP |

The results showed UDP traffic generated by the firewall itself, including DNS queries and other system services leaving the WAN interface.

Filtering traffic by protocol helps administrators isolate specific communication types during troubleshooting and security investigations.

**Screenshot**

> Figure 6: UDP traffic displayed in Live View.

![UDP Traffic Filter](../screenshots/lab08/udp-filter.png)

## Testing and Verification

The following verification steps were performed during this lab:

| Test | Expected Result | Status |
|------|-----------------|--------|
| Open Firewall Live View | Firewall logs displayed successfully | ✅ Passed |
| View real-time firewall events | Events updated automatically | ✅ Passed |
| Filter logs by Action (PASS) | Only allowed traffic displayed | ✅ Passed |
| Filter logs by Action (BLOCK) | Only blocked traffic displayed | ✅ Passed |
| Filter logs by Protocol (TCP) | Only TCP events displayed | ✅ Passed |
| Filter logs by Protocol (UDP) | Only UDP events displayed | ✅ Passed |
| Analyze firewall rules | Successfully identified the rule responsible for each event | ✅ Passed |

## Results

The OPNsense Live View interface successfully displayed real-time firewall events generated by the firewall.

Different filtering techniques were applied to isolate specific network events, including allowed traffic, blocked traffic, TCP communications, and UDP communications. The analysis also demonstrated how firewall rules are associated with each logged event, allowing administrators to quickly determine why a packet was permitted or denied.

This laboratory provided practical experience in reading and interpreting firewall logs, an essential skill for network administrators and SOC analysts.

## Key Learning Points

- Firewall logs provide visibility into all processed network traffic.
- Each log entry contains valuable information such as interface, protocol, source IP, destination IP, action, and firewall rule.
- Filtering logs significantly improves incident investigation efficiency.
- Allowed and blocked traffic can be analyzed independently.
- TCP and UDP traffic can be isolated for protocol-specific investigations.
- Firewall logging is a fundamental component of network monitoring and security operations.

  ## Conclusion

In this laboratory, I learned how to monitor and analyze firewall events using the OPNsense Live View logging system. I successfully filtered firewall logs by action and protocol, interpreted firewall decisions, and identified the rules responsible for processing network traffic.

These practical skills are essential for troubleshooting network issues, monitoring firewall activity, and performing security investigations in a Security Operations Center (SOC) environment.
