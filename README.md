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

- **135/tcp:** Microsoft
