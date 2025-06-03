

## Overview

This practical guide provides hands-on experience with Nmap (Network Mapper), one of the most powerful network discovery and security auditing tools. We'll cover both command-line Nmap and Zenmap (GUI) usage for comprehensive network reconnaissance.


## Prerequisites

- Linux system (Kali Linux recommended) or Windows with Nmap installed
- Nmap installed (`sudo apt-get install nmap` on Linux)
- Zenmap installed (GUI version of Nmap)
- Basic networking knowledge
- Test network environment (virtual lab recommended)

## Lab Setup

For this practical, we'll assume a test network range of `192.168.70.0/24`. Adjust IP ranges according to your lab environment.

## Part 1: Command-Line Nmap Fundamentals

### Exercise 1: Host Discovery

**Objective:** Learn to identify active hosts on a network

#### Task 1.1: Ping Scan - Find Active Hosts

```bash
# Basic ping scan to discover live hosts
nmap -sn 192.168.70.1/24

# Save results to file
nmap -sn 192.168.70.1/24 > live_hosts.txt

# Scan specific range
nmap -sn 192.168.70.1-50

# Alternative ping scan methods
nmap -sn -PS80,443 192.168.70.1/24  # TCP SYN ping
nmap -sn -PA80,443 192.168.70.1/24  # TCP ACK ping
```

**Expected Output Analysis:**

- List of active IP addresses
- MAC addresses (if on same subnet)
- Vendor information from MAC addresses
- Response times

#### Task 1.2: No Ping Scan (Treat All Hosts as Up)

```bash
# Scan without ping (useful for firewall bypass)
nmap -Pn 192.168.70.1/24

# Compare with ping scan results
nmap -sn 192.168.70.1/24 > with_ping.txt
nmap -Pn --top-ports 10 192.168.70.1/24 > without_ping.txt
```

### Exercise 2: Port Scanning Techniques

**Objective:** Master different port scanning methods

#### Task 2.1: SYN Scan (Stealth Scan)

```bash
# Basic SYN scan on single host
nmap -sS 192.168.70.1

# SYN scan with specific ports
nmap -sS -p 22,80,443,3389 192.168.70.1

# SYN scan with port range
nmap -sS -p 1-1000 192.168.70.1

# Analyze the output
echo "Checking stealth scan results:"
nmap -sS 192.168.70.1 | grep -E "(open|closed|filtered)"
```

**Key Points:**

- Half-open scan (doesn't complete TCP handshake)
- Harder to detect than full TCP connect
- Requires root privileges

#### Task 2.2: TCP Connect Scan

```bash
# Full TCP connection scan
nmap -sT 192.168.70.1

# Compare with SYN scan
echo "=== SYN Scan Results ==="
nmap -sS 192.168.70.1
echo "=== TCP Connect Scan Results ==="
nmap -sT 192.168.70.1
```

#### Task 2.3: UDP Scan

```bash
# UDP service discovery
nmap -sU 192.168.70.2

# Common UDP ports
nmap -sU -p 53,67,68,69,123,161,162 192.168.70.2

# Combined TCP and UDP scan
nmap -sS -sU -p T:80,443,U:53,161 192.168.70.2
```

**Note:** UDP scans are slower and may require patience

### Exercise 3: Service and Version Detection

**Objective:** Identify services and their versions

#### Task 3.1: Service Version Detection

```bash
# Basic service detection
nmap -sV 192.168.70.2

# Intensive version detection
nmap -sV --version-intensity 5 192.168.70.2

# Light version detection (faster)
nmap -sV --version-light 192.168.70.2

# Service detection with common ports
nmap -sV --top-ports 100 192.168.70.2
```

#### Task 3.2: Banner Grabbing

```bash
# Grab service banners
nmap -sV --script=banner 192.168.70.2

# HTTP banner grabbing
nmap -p 80,443 --script http-title,http-server-header 192.168.70.2
```

### Exercise 4: Operating System Detection

**Objective:** Identify target operating systems

#### Task 4.1: OS Fingerprinting

```bash
# Basic OS detection
nmap -O 192.168.70.2

# OS detection with service detection
nmap -O -sV 192.168.70.2

# Aggressive OS detection
nmap -O --osscan-guess 192.168.70.2

# OS detection without ping
nmap -O -Pn 192.168.70.2
```

#### Task 4.2: Advanced OS Detection

```bash
# Detailed OS information
nmap -O --osscan-limit 192.168.70.2

# OS detection with script scanning
nmap -O -sC 192.168.70.2
```

### Exercise 5: Comprehensive Scanning

**Objective:** Perform thorough system analysis

#### Task 5.1: Aggressive Scan

```bash
# Complete aggressive scan
nmap -A 192.168.70.2

# Aggressive scan with all ports
nmap -A -p- 192.168.70.2

# Timed aggressive scan
nmap -A -T4 192.168.70.2
```

#### Task 5.2: Full Port Range Scanning

```bash
# Scan all 65,535 TCP ports
nmap -p- 192.168.70.2

# Fast scan of all ports
nmap -p- -T4 192.168.70.2

# All ports with service detection
nmap -p- -sV 192.168.70.2

# Monitor progress
nmap -p- --stats-every 30s 192.168.70.2
```

### Exercise 6: Firewall Evasion Techniques

**Objective:** Learn methods to bypass basic security measures

#### Task 6.1: Fragmentation

```bash
# Fragment packets to bypass firewalls
nmap -f 192.168.70.2

# Maximum fragmentation
nmap -ff 192.168.70.2

# Custom MTU
nmap --mtu 16 192.168.70.2
```

#### Task 6.2: Decoy Scanning

```bash
# Use decoy IP addresses
nmap -D 192.168.70.10,192.168.70.11,ME 192.168.70.2

# Random decoys
nmap -D RND:5 192.168.70.2

# Source port specification
nmap --source-port 53 192.168.70.2
```

#### Task 6.3: Invalid Checksums

```bash
# Send packets with bad checksums
nmap --badsum 192.168.70.2

# Combine with other evasion techniques
nmap --badsum -f 192.168.70.2
```

### Exercise 7: Script Scanning (NSE)

**Objective:** Utilize Nmap Scripting Engine for advanced reconnaissance

#### Task 7.1: Default Scripts

```bash
# Run default NSE scripts
nmap -sC 192.168.70.2

# Combine with service detection
nmap -sC -sV 192.168.70.2

# Default scripts on specific ports
nmap -sC -p 80,443 192.168.70.2
```

#### Task 7.2: Vulnerability Scanning

```bash
# Run vulnerability detection scripts
nmap --script=vuln 192.168.70.2

# Specific vulnerability categories
nmap --script=auth,brute 192.168.70.2
nmap --script=discovery,safe 192.168.70.2

# HTTP vulnerability scanning
nmap --script=http-vuln* -p 80,443 192.168.70.2
```

#### Task 7.3: Custom Script Usage

```bash
# SMB enumeration
nmap --script=smb-enum* 192.168.70.2

# DNS information gathering
nmap --script=dns* 192.168.70.2

# FTP enumeration
nmap --script=ftp* -p 21 192.168.70.2
```

### Exercise 8: Advanced Reconnaissance

**Objective:** Perform comprehensive security assessment

#### Task 8.1: Full Ethical Hacking Reconnaissance

```bash
# Comprehensive scan with vulnerability detection
nmap -A -p- --script=vuln 192.168.70.2

# Save detailed results
nmap -A -p- --script=vuln 192.168.70.2 -oA full_recon

# Timed comprehensive scan
nmap -A -T4 -p- --script=vuln 192.168.70.2
```

#### Task 8.2: Network Mapping

```bash
# Discover network topology
nmap -sn --traceroute 192.168.70.1/24

# Route discovery
nmap --traceroute 192.168.70.1

# Network range analysis
nmap -sL 192.168.70.1/24  # List scan
```

## Part 2: Zenmap GUI Operations

### Exercise 9: Zenmap Basics

**Objective:** Master the graphical interface for Nmap

#### Task 9.1: Setting Up Zenmap

```
1. Launch Zenmap
2. Familiarize with the interface:
   - Target field
   - Profile dropdown
   - Command field
   - Scan button
```

#### Task 9.2: Profile-Based Scanning

**Ping Scan Profile:**

- Command: `nmap -sn <target>`
- Use case: Host discovery
- Target: `192.168.70.129`

**Quick Scan Profile:**

- Command: `nmap -T4 -F <target>`
- Use case: Fast port scan
- Target: `192.168.70.129`

### Exercise 10: Zenmap Advanced Profiles

**Objective:** Explore different scanning profiles

#### Task 10.1: Quick Scan Plus

```
Profile: Quick scan plus
Command: nmap -sV -T4 -O -F --version-light <target>
Purpose: Lightweight service and OS detection
Target: 192.168.70.129

Analysis Points:
- Service versions detected
- Operating system identification
- Open ports and services
```

#### Task 10.2: Intense Scan

```
Profile: Intense scan
Command: nmap -T4 -A -v <target>
Purpose: Comprehensive system analysis
Target: 192.168.70.129

Expected Results:
- OS detection
- Service versions
- Script scan results
- Traceroute information
```

#### Task 10.3: Intense Scan Plus UDP

```
Profile: Intense scan plus UDP
Command: nmap -sS -sU -T4 -A -v <target>
Purpose: Complete TCP and UDP analysis
Target: 192.168.70.129

Note: This scan will take longer due to UDP scanning
```

### Exercise 11: Specialized Zenmap Scans

**Objective:** Perform targeted scans for specific purposes

#### Task 11.1: All TCP Ports

```
Profile: Intense scan, all TCP ports
Command: nmap -p 1-65535 -T4 -A -v <target>
Purpose: Comprehensive port coverage
Target: 192.168.70.129

Expected Findings:
- All open TCP ports
- Services on non-standard ports
- Complete service fingerprinting
```

#### Task 11.2: No Ping Scan

```
Profile: Intense scan, no ping
Command: nmap -T4 -A -v -Pn <target>
Purpose: Scan hosts that block ICMP
Target: 192.168.70.129

Use Cases:
- Hosts behind firewalls
- Windows systems with ping disabled
- Stealth reconnaissance
```

#### Task 11.3: Slow Comprehensive Scan

```
Profile: Slow comprehensive scan
Command: nmap -sS -sU -T2 -A -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script all <target>
Purpose: Maximum detail with stealth
Target: 192.168.70.129

Warning: This scan is very slow but extremely thorough
```

## Part 3: Output Analysis and Reporting

### Exercise 12: Understanding Nmap Output

**Objective:** Learn to interpret scan results effectively

#### Task 12.1: Port States

```bash
# Generate different port states
nmap -p 1-1000 192.168.70.129

# Understanding states:
# - open: Service actively accepting connections
# - closed: Port accessible but no service listening
# - filtered: Packet filtered by firewall/security device
# - unfiltered: Accessible but can't determine if open/closed
# - open|filtered: Either open or filtered (can't determine)
# - closed|filtered: Either closed or filtered (can't determine)
```

#### Task 12.2: Service Information Analysis

```bash
# Detailed service analysis
nmap -sV -A 192.168.70.129

# Focus on specific services
nmap -sV -p 135,139,445 192.168.70.129

# Script-based service enumeration
nmap -sC -sV -p 135,139,445 192.168.70.129
```

### Exercise 13: Output Formats and Reporting

**Objective:** Generate professional scan reports

#### Task 13.1: Multiple Output Formats

```bash
# Generate all output formats
nmap -A 192.168.70.129 -oA comprehensive_scan

# Individual formats
nmap -A 192.168.70.129 -oN normal_output.txt
nmap -A 192.168.70.129 -oX xml_output.xml
nmap -A 192.168.70.129 -oG greppable_output.txt

# Examine outputs
cat normal_output.txt
```

#### Task 13.2: Custom Reporting

```bash
# Create summary report
echo "=== Network Scan Summary ===" > scan_report.txt
echo "Date: $(date)" >> scan_report.txt
echo "Target: 192.168.70.129" >> scan_report.txt
echo "" >> scan_report.txt

nmap -A 192.168.70.129 | grep -E "(open|Host|MAC)" >> scan_report.txt
```

## Part 4: Real-World Scenarios

### Exercise 14: Network Security Assessment

**Objective:** Apply Nmap in practical security testing scenarios

#### Task 14.1: Windows Network Assessment

```bash
# Comprehensive Windows host analysis
nmap -sS -sU -A --script=smb*,ms* 192.168.70.129

# Focus on Windows-specific services
nmap -p 135,139,445,3389,5985 -sV -sC 192.168.70.129

# Check for common Windows vulnerabilities
nmap --script=vuln -p 445 192.168.70.129
```

#### Task 14.2: Web Server Assessment

```bash
# Web server enumeration
nmap -p 80,443,8080,8443 --script=http* 192.168.70.129

# SSL/TLS analysis
nmap -p 443 --script=ssl* 192.168.70.129

# Web application discovery
nmap -p 80,443 --script=http-enum 192.168.70.129
```

#### Task 14.3: Network Infrastructure Scanning

```bash
# Router/Switch discovery
nmap -sU -p 161 --script=snmp* 192.168.70.1/24

# DNS server analysis
nmap -sU -p 53 --script=dns* 192.168.70.2

# Network printer discovery
nmap -p 9100,515,631 192.168.70.1/24
```

### Exercise 15: Performance Optimization

**Objective:** Optimize scans for different scenarios

#### Task 15.1: Timing Templates

```bash
# Compare timing templates
echo "=== Paranoid (T0) ==="
time nmap -T0 -p 80,443 192.168.70.129

echo "=== Sneaky (T1) ==="
time nmap -T1 -p 80,443 192.168.70.129

echo "=== Polite (T2) ==="
time nmap -T2 -p 80,443 192.168.70.129

echo "=== Normal (T3) ==="
time nmap -T3 -p 80,443 192.168.70.129

echo "=== Aggressive (T4) ==="
time nmap -T4 -p 80,443 192.168.70.129

echo "=== Insane (T5) ==="
time nmap -T5 -p 80,443 192.168.70.129
```

#### Task 15.2: Parallel Scanning

```bash
# Adjust parallelism
nmap --min-parallelism 100 --max-parallelism 256 192.168.70.129

# Host group sizes
nmap --min-hostgroup 50 --max-hostgroup 100 192.168.70.1/24

# Rate limiting
nmap --min-rate 100 --max-rate 1000 192.168.70.129
```

## Part 5: Troubleshooting and Best Practices

### Exercise 16: Common Issues and Solutions

**Objective:** Handle typical scanning challenges

#### Task 16.1: Permission Issues

```bash
# Check if running as root (required for some scans)
whoami

# Raw socket permissions
nmap -sS 192.168.70.129  # Requires root
nmap -sT 192.168.70.129  # Works as regular user
```

#### Task 16.2: Firewall Detection

```bash
# Detect firewall presence
nmap -A --reason 192.168.70.129

# Bypass techniques
nmap -f -D RND:10 --source-port 53 192.168.70.129

# Multiple evasion techniques
nmap -f -T2 --data-length 200 -D RND:5 192.168.70.129
```

### Exercise 17: Ethical Considerations

**Objective:** Understand responsible scanning practices

#### Task 17.1: Scan Throttling

```bash
# Respectful scanning
nmap -T2 --scan-delay 1s 192.168.70.129

# Bandwidth consideration
nmap --max-rate 50 192.168.70.129

# Host consideration
nmap --max-hostgroup 5 192.168.70.1/24
```

## Part 6: Advanced Techniques

### Exercise 18: Custom Scripts and Automation

**Objective:** Develop advanced scanning workflows

#### Task 18.1: Bash Automation

```bash
#!/bin/bash
# Automated network assessment script

TARGET_NETWORK="192.168.70.0/24"
OUTPUT_DIR="scan_results_$(date +%Y%m%d_%H%M%S)"

mkdir -p $OUTPUT_DIR

# Host discovery
echo "Performing host discovery..."
nmap -sn $TARGET_NETWORK > $OUTPUT_DIR/live_hosts.txt

# Extract live IPs
grep -oP '(?<=Nmap scan report for )[0-9.]+' $OUTPUT_DIR/live_hosts.txt > $OUTPUT_DIR/target_ips.txt

# Scan each host
while read -r ip; do
    echo "Scanning $ip..."
    nmap -A -oA $OUTPUT_DIR/scan_$ip $ip
done < $OUTPUT_DIR/target_ips.txt

echo "Scans completed. Results in $OUTPUT_DIR/"
```

#### Task 18.2: Python Integration

```python
#!/usr/bin/env python3
# Python script for automated Nmap scanning

import subprocess
import json
import os
from datetime import datetime

def run_nmap_scan(target, options):
    """Execute Nmap scan and return results."""
    cmd = f"nmap {options} {target}"
    result = subprocess.run(cmd.split(), capture_output=True, text=True)
    return result.stdout

def main():
    target = "192.168.70.129"
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    
    scans = {
        "host_discovery": "-sn",
        "port_scan": "-sS --top-ports 1000",
        "service_detection": "-sV",
        "os_detection": "-O",
        "vulnerability_scan": "--script=vuln"
    }
    
    results = {}
    
    for scan_name, options in scans.items():
        print(f"Running {scan_name}...")
        results[scan_name] = run_nmap_scan(target, options)
        
        # Save individual results
        with open(f"{scan_name}_{timestamp}.txt", "w") as f:
            f.write(results[scan_name])
    
    print("All scans completed!")

if __name__ == "__main__":
    main()
```

## Common Port Reference

|Port|Service|Protocol|Purpose|
|---|---|---|---|
|21|FTP|TCP|File Transfer Protocol|
|22|SSH|TCP|Secure Shell|
|23|Telnet|TCP|Remote terminal|
|25|SMTP|TCP|Email transmission|
|53|DNS|TCP/UDP|Domain Name System|
|80|HTTP|TCP|Web traffic|
|110|POP3|TCP|Email retrieval|
|135|RPC|TCP|Microsoft RPC|
|139|NetBIOS|TCP|Windows file sharing|
|143|IMAP|TCP|Email access|
|161|SNMP|UDP|Network management|
|389|LDAP|TCP|Directory services|
|443|HTTPS|TCP|Secure web traffic|
|445|SMB|TCP|Windows file sharing|
|993|IMAPS|TCP|Secure IMAP|
|995|POP3S|TCP|Secure POP3|
|3389|RDP|TCP|Remote Desktop|
|5985|WinRM|TCP|Windows Remote Management|

## Timing Template Reference

|Template|Speed|Use Case|
|---|---|---|
|T0 (Paranoid)|Extremely slow|IDS evasion|
|T1 (Sneaky)|Very slow|Stealth scanning|
|T2 (Polite)|Slow|Bandwidth consideration|
|T3 (Normal)|Default|Standard scanning|
|T4 (Aggressive)|Fast|Time-sensitive scans|
|T5 (Insane)|Very fast|Fast networks only|

## Script Categories

|Category|Purpose|Example Scripts|
|---|---|---|
|auth|Authentication bypass|http-auth-finder|
|brute|Brute force attacks|ssh-brute|
|discovery|Information gathering|dns-brute|
|dos|Denial of service|http-slowloris|
|exploit|Vulnerability exploitation|ms17-010|
|fuzzer|Fuzzing/testing|http-form-fuzzer|
|intrusive|Invasive testing|http-sql-injection|
|malware|Malware detection|http-malware-host|
|safe|Safe for production|banner|
|version|Version detection|http-server-header|
|vuln|Vulnerability detection|vuln|

## Best Practices Summary

1. **Authorization**: Always obtain proper authorization before scanning
2. **Documentation**: Keep detailed records of all scans performed
3. **Timing**: Use appropriate timing templates for the environment
4. **Stealth**: Consider detection avoidance when required
5. **Accuracy**: Verify results with multiple scan types
6. **Reporting**: Generate comprehensive reports for stakeholders
7. **Updates**: Keep Nmap and scripts updated regularly
8. **Ethics**: Follow responsible disclosure practices

## Troubleshooting Guide

### Common Issues:

1. **"Operation not permitted"**
    
    - Solution: Run as root or use -sT instead of -sS
2. **"Host seems down"**
    
    - Solution: Use -Pn to skip host discovery
3. **Slow UDP scans**
    
    - Solution: Reduce port range or use --max-retries 1
4. **Filtered ports**
    
    - Solution: Try fragmentation (-f) or decoy scans (-D)
5. **No OS detection**
    
    - Solution: Ensure at least one open and one closed port

## Conclusion

This comprehensive practical guide covers the essential aspects of network mapping with Nmap, from basic host discovery to advanced vulnerability assessment. The exercises progress from fundamental concepts to real-world application scenarios, providing hands-on experience with both command-line and GUI operations.

Remember to always:

- Use these techniques responsibly and with proper authorization
- Start with non-intrusive scans before moving to aggressive techniques
- Document your findings thoroughly
- Consider the impact of your scans on target systems
- Stay updated with the latest Nmap features and security considerations
