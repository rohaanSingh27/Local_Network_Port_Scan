# Local Network Port Scanning & Network Reconnaissance

## 1. Project Overview

This project focuses on performing basic network reconnaissance and port scanning on my local network using Nmap and Wireshark.

The objective was to understand how devices communicate over a network, identify open TCP ports, detect running services, and observe TCP packet behaviour during a port scan.

All scanning activities were performed on my own laptop and local network for educational purposes.

## 2. Tools Used

- **Nmap 7.98** – Network discovery and port scanning.
- **Wireshark 4.6.9** – Network traffic capture and packet analysis.
- **Windows Command Prompt** – Executing network commands.

## 3. Objectives

- Understand local network configuration.
- Identify active hosts on the local subnet.
- Perform TCP SYN port scanning.
- Detect services running on open ports.
- Analyse TCP communication using Wireshark.
- Understand SYN, SYN-ACK, and RST packets.

## 4. Network Configuration

I used the Windows `ipconfig` command to identify my network configuration.

| Parameter | Value |
|---|---|
| Network Interface | Wi-Fi |
| IPv4 Address | 10.204.62.26 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.204.62.150 |
| Network Range | 10.204.62.0/24 |

The initial and final network configuration screenshots both showed the same IPv4 address: `10.204.62.26`.

## 5. Host Discovery and Network Scanning

I performed a TCP SYN scan on the local subnet using:

```bash
nmap -sS 10.204.62.0/24
```

The scan reported two active hosts.

### Observed Results

| IP Address | Open Port | Service |
|---|---|---|
| 10.204.62.150 | 53/tcp | DNS |
| 10.204.62.26 | 135/tcp | MSRPC |
| 10.204.62.26 | 139/tcp | NetBIOS-SSN |
| 10.204.62.26 | 445/tcp | Microsoft-DS |

**Important observation:** The IP address `10.204.62.26` belonged to my own laptop, as confirmed by the Windows `ipconfig` output. Therefore, these ports were observed on my laptop and not identified as a separate device.

## 6. TCP SYN Port Scanning

To examine the selected ports on my laptop, I executed:

```bash
nmap -sS -p 135,139,445 10.204.62.26
```

The scan identified the following open ports:

- **135/tcp:** Microsoft RPC
- **139/tcp:** NetBIOS Session Service
- **445/tcp:** Microsoft-DS

A SYN scan helps identify port states by examining TCP responses without completing a full TCP connection.

## 7. Service Version Detection

To identify the services associated with the open ports, I used Nmap service detection:

```bash
nmap -sV -p 135,139,445 10.204.62.26 -oN service_detection.txt
```

### Observations

| Port | Service Identified |
|---|---|
| 135/tcp | Microsoft Windows RPC |
| 139/tcp | Microsoft NetBIOS-SSN |
| 445/tcp | Microsoft-DS |

The output also indicated a Windows operating system family.

The scan results were saved in `service_detection.txt`.

## 8. Wireshark Packet Analysis

I used Wireshark to capture and analyse network packets generated during a local Nmap scan.

For this experiment, I selected the loopback capture adapter and executed:

```bash
nmap -sS -p 135,139,445 127.0.0.1
```

I observed different TCP packet types during the scan.

### A. SYN Packets

SYN packets are sent by the scanner to initiate TCP communication and check the status of target ports.

Display filter:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

### B. SYN-ACK Packets

SYN-ACK packets indicate that a TCP connection request has received a response from a listening port.

Display filter:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

### C. RST Packets

RST packets are used to reset or terminate TCP communication.

During a SYN scan, Nmap may send RST packets to avoid completing the normal TCP handshake.

Display filter:

```text
tcp.flags.reset == 1
```

The captured packets helped me understand how TCP communication works during port scanning.

## 9. Security Observations

- Open ports indicate services listening for network communication.
- Ports 135, 139, and 445 are associated with Windows networking functionality.
- Unnecessary exposed services can increase the attack surface.
- Open ports do not automatically indicate vulnerabilities.
- Network services should be configured according to security requirements.

## 10. Practical Challenges

During this experiment, I encountered a few practical challenges:

- Understanding the difference between host discovery and port scanning.
- Interpreting Nmap output and identifying open ports.
- Selecting the correct network interface in Wireshark.
- Understanding the purpose of SYN, SYN-ACK, and RST packets.
- Saving scan results in the correct directory.

Working through these challenges improved my understanding of network reconnaissance and packet analysis.

## 11. Project Files

```text
Local_Network_Port_Scan/
│
├── README.md
├── service_detection.txt
├── screenshots/
│   ├── nmap-version.png
│   ├── ipconfig.png
│   ├── port-scan.png
│   ├── service-detection.png
│   ├── syn-packets.png
│   ├── syn-ack-packets.png
│   └── rst-packets.png
│
└── scan-results/
```


## 12. Conclusion

This project provided practical experience with network reconnaissance, TCP SYN scanning, service detection, and packet analysis.

By using Nmap and Wireshark, I learned how to identify open ports, understand the services associated with them, and observe TCP packet behaviour.

This exercise strengthened my understanding of basic network security concepts and provided a foundation for further learning in cybersecurity.

---

**Project Type:** Practical Cybersecurity Task  
**Tools:** Nmap, Wireshark, Windows  
**Focus:** Network Reconnaissance and Port Scanning
