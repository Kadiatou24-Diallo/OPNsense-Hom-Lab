# Lab 09 – Packet Capture and Traffic Analysis in OPNsense

## Lab Overview

Packet capture is one of the most important techniques used by network engineers, security analysts, and incident responders to inspect network communications. It allows analysts to observe packets in real time, troubleshoot connectivity issues, verify firewall behavior, and investigate suspicious network activity.

In this laboratory, packet captures were performed using the built-in Packet Capture tool in OPNsense. The captured traffic was then analyzed using Wireshark to understand TCP communication, HTTP requests, and packet flow across different firewall interfaces.
## Objectives

After completing this lab, you will be able to:

- Capture network traffic using OPNsense Packet Capture.
- Export packet captures in PCAP format.
- Analyze captured traffic using Wireshark.
- Identify TCP three-way handshake packets.
- Analyze HTTP GET requests and HTTP 200 OK responses.
- Compare packet captures on LAN and WAN interfaces.
- Understand how the capture location affects packet visibility.
- Explain the importance of packet capture during security investigations.
  ## Prerequisites

Before starting this laboratory, the following requirements were completed:

- OPNsense firewall installed and configured.
- LAN and WAN interfaces configured correctly.
- Kali Linux connected to the LAN network.
- Ubuntu Linux connected to the WAN network.
- Basic firewall rules configured.
- HTTP server running on Kali Linux using Python.
- Wireshark installed on the analysis workstation.
  ## Lab Environment

| Component | Details |
|-----------|---------|
| Firewall | OPNsense |
| Hypervisor | VMware Workstation Pro |
| Host Operating System | Windows 11 |
| Firewall LAN | 192.168.105.2 |
| Firewall WAN | 192.168.152.130 |
| Kali Linux | 192.168.105.19 |
| Ubuntu Linux | 192.168.152.132 |
| Packet Analyzer | Wireshark |

## Network Topology

The following network topology was used during this laboratory.

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
# Part 1 – Packet Capture Fundamentals

## Step 1: Open Packet Capture

Log in to the OPNsense web interface and navigate to:

```text
Interfaces
    ↓
Diagnostics
        ↓
Packet Capture
```

The Packet Capture utility allows administrators to capture network packets directly from any firewall interface. Captured traffic can be downloaded in PCAP format and analyzed using Wireshark.

**Figure 1: OPNsense Packet Capture Page**

![Packet Capture](../screenshots/lab09/packet-capture-page.png)

## Step 2: Configure the Packet Capture

The packet capture was configured using the following parameters.

| Parameter | Value |
|-----------|-------|
| Interface | LAN (em1) |
| Promiscuous Mode | Disabled |
| Address Family | Any |
| Protocol | TCP |
| Host Address | Leave Empty |
| Port | 8080 |
| Packet Length | Default |
| Count | 50 |
| Description | Lab 09 - LAN HTTP Capture |

These settings allow OPNsense to capture HTTP traffic exchanged between the client and the web server running on Kali Linux.

**Figure 2: Packet Capture Configuration**

![Packet Capture Configuration](../screenshots/lab09/packet-capture-configuration.png)

## Step 3: Generate HTTP Traffic

While the packet capture was running, HTTP traffic was generated from the Ubuntu client by accessing the Python web server hosted on Kali Linux.

```bash
curl http://192.168.105.19:8080
```

The HTTP request generated TCP packets, an HTTP GET request, and the corresponding HTTP response. Once the traffic was captured, the capture was stopped and exported as a PCAP file for further analysis in Wireshark.

**Figure 3: HTTP Traffic Generation**

![HTTP Traffic Generation](../screenshots/lab09/http-traffic-generation.png)

# Part 2 – Packet Analysis with Wireshark

## Step 4: Open the PCAP File in Wireshark

After completing the packet capture, the PCAP file was downloaded from OPNsense and opened using Wireshark for detailed traffic analysis.

Wireshark provides a comprehensive view of captured packets and allows analysts to inspect network communications at different protocol layers.

The Wireshark interface consists of three main sections:

- **Packet List** – Displays all captured packets.
- **Packet Details** – Displays protocol information for the selected packet.
- **Packet Bytes** – Displays the raw hexadecimal representation of the selected packet.

**Figure 4: Packet Capture Opened in Wireshark**

![Wireshark Overview](../screenshots/lab09/wireshark-overview.png)

## Step 5: Analyze the TCP Three-Way Handshake

The captured traffic clearly shows the TCP connection establishment between the client and the web server.

The communication follows the standard TCP three-way handshake process:

1. **SYN** – The client requests to establish a connection.
2. **SYN, ACK** – The server acknowledges the request.
3. **ACK** – The client confirms the connection.

This process ensures that both systems are ready before data transmission begins.

**Figure 5: TCP Three-Way Handshake**

![TCP Handshake](../screenshots/lab09/tcp-three-way-handshake.png)

## Step 6: Analyze the HTTP Communication

After the TCP connection was established, the client sent an HTTP GET request to the Python web server running on Kali Linux.

```text
GET / HTTP/1.1
```

The server successfully responded with:

```text
HTTP/1.0 200 OK
```

This confirms that the web server successfully processed the request and returned the requested resource.

The packet capture demonstrates the complete HTTP communication between the client and the server.

**Figure 6: HTTP GET Request and HTTP 200 OK Response**

![HTTP Communication](../screenshots/lab09/http-communication.png)

## Step 7: Analyze TCP Connection Termination

After the HTTP response was delivered, both systems terminated the TCP session gracefully using the FIN/ACK process.

The TCP connection termination ensures that both devices properly close the communication channel and release network resources.

This behavior confirms that the HTTP session completed successfully without packet loss or communication errors.

**Figure 7: TCP Connection Termination**

![TCP Connection Termination](../screenshots/lab09/tcp-termination.png)

# Part 3 – Packet Capture on the LAN Interface

## Step 8: Analyze the LAN Capture

The first packet capture was performed on the **LAN interface (em1)** to observe HTTP traffic destined for the internal web server.

The captured packets included:

- TCP connection establishment (Three-Way Handshake)
- HTTP GET request
- HTTP 200 OK response
- TCP connection termination (FIN/ACK)

The capture demonstrated successful communication between the client and the web server hosted on Kali Linux.

**Figure 8: Packet Capture on the LAN Interface**

![LAN Packet Capture](../screenshots/lab09/lan-packet-capture.png)

# Part 4 – Packet Capture on the WAN Interface

## Step 9: Analyze the WAN Capture

A second packet capture was performed on the **WAN interface (em0)** while generating the same HTTP request from the Ubuntu machine.

The capture successfully recorded the complete TCP and HTTP communication between the Ubuntu client and the Kali web server.

Unlike the LAN capture, the WAN capture clearly showed the original source IP address of the Ubuntu workstation (**192.168.152.132**), demonstrating how packet visibility depends on the selected capture interface.

**Figure 9: Packet Capture on the WAN Interface**
![WAN Packet Capture](../screenshots/lab09/wan-packet-capture.png)

# Part 5 – Traffic Comparison

The same HTTP communication was captured from two different firewall interfaces to compare how network traffic appears at different observation points.

| Feature | LAN Capture | WAN Capture |
|----------|-------------|-------------|
| Capture Interface | LAN (em1) | WAN (em0) |
| Client Traffic Observed | Internal LAN Traffic | Ubuntu Client Traffic |
| HTTP GET Request | Yes | Yes |
| HTTP 200 OK Response | Yes | Yes |
| TCP Three-Way Handshake | Yes | Yes |
| TCP Session Termination | Yes | Yes |

This comparison demonstrates that packet visibility depends on the firewall interface where the capture is performed. Selecting the correct observation point is essential for effective network troubleshooting and traffic analysis.

# Part 6 – Traffic Flow Analysis Across Firewall Interfaces

During this laboratory, packet captures were performed on both the LAN and WAN interfaces while generating the same HTTP communication.

The experiment demonstrated that the observed traffic depends on the interface where the capture is performed.

The WAN capture displayed the original Ubuntu client address (**192.168.152.132**) communicating with the internal web server, while the LAN capture provided a different view of the traffic reaching the internal network.

This experiment highlights an important networking concept:

- Packet captures represent the traffic visible at a specific observation point.
- Different firewall interfaces provide different perspectives of the same communication.
- Choosing the correct capture interface is critical when troubleshooting routing, firewall behavior, or network connectivity.

  # Part 7 – SOC Analyst Perspective

Packet capture is one of the primary investigative techniques used in Security Operations Centers (SOCs).

During incident investigations, analysts capture traffic on different firewall interfaces to understand how packets traverse the network.

Comparing captures from multiple interfaces helps analysts:

- Verify network connectivity.
- Identify routing issues.
- Understand firewall behavior.
- Validate security policies.
- Investigate suspicious network activity.
- Support incident response and forensic investigations.

This laboratory demonstrates how packet captures complement firewall logs by providing direct visibility into network communications.

Rather than relying on assumptions, packet captures allow administrators to validate the actual network behavior.

## Lessons Learned

During this laboratory, I learned that packet captures provide different perspectives depending on the selected firewall interface. I also confirmed that firewall logs and packet captures complement each other during troubleshooting. Comparing captures from both LAN and WAN interfaces improved my understanding of traffic flow and reinforced the importance of selecting the appropriate observation point during network analysis.

## Testing and Verification

The following tests were performed to verify the packet capture functionality and analyze network communications.

| Test | Expected Result | Status |
|------|-----------------|--------|
| Open OPNsense Packet Capture | Packet Capture page accessible | ✅ Passed |
| Configure Packet Capture | Capture parameters configured successfully | ✅ Passed |
| Capture HTTP Traffic | HTTP packets successfully captured | ✅ Passed |
| Export PCAP File | PCAP file downloaded successfully | ✅ Passed |
| Open PCAP in Wireshark | Capture opened without errors | ✅ Passed |
| Analyze TCP Handshake | SYN, SYN-ACK, and ACK packets identified | ✅ Passed |
| Analyze HTTP Traffic | HTTP GET and HTTP 200 OK identified | ✅ Passed |
| Compare LAN and WAN Captures | Traffic differences successfully analyzed | ✅ Passed |

## Results

The packet capture experiments successfully demonstrated how network traffic can be captured and analyzed using OPNsense and Wireshark.

HTTP communication between the Ubuntu client and the Kali Linux web server was successfully recorded and analyzed. The packet captures clearly showed the TCP three-way handshake, HTTP request, HTTP response, and TCP connection termination.

Capturing the same communication on both the LAN and WAN interfaces demonstrated that packet visibility depends on the selected observation point. This experiment reinforced the importance of selecting the appropriate interface when troubleshooting network communications.

## Key Learning Points

- Packet capture provides direct visibility into network communications.
- Wireshark allows detailed inspection of network protocols and packet contents.
- TCP communication follows the standard three-way handshake before data transmission.
- HTTP communication consists of requests and responses exchanged between clients and servers.
- Packet visibility depends on the interface where the capture is performed.
- Packet captures complement firewall logs during troubleshooting and security investigations.
- Comparing captures from multiple interfaces provides a deeper understanding of network traffic flow.

  ## Conclusion

In this laboratory, packet captures were successfully performed using OPNsense and analyzed with Wireshark.

The captured traffic demonstrated the complete lifecycle of an HTTP communication, including TCP connection establishment, HTTP request and response, and connection termination.

Performing packet captures on both the LAN and WAN interfaces highlighted the importance of selecting the correct observation point during network analysis. This practical exercise strengthened my understanding of packet analysis, traffic flow, and the relationship between firewall monitoring and packet inspection.

The knowledge gained from this laboratory provides a solid foundation for future IDS/IPS deployment, intrusion analysis, and Security Operations Center (SOC) investigations.
