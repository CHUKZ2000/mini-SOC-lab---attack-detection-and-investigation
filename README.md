# Mini SOC Lab - Attack Detection & Investigation

## Project Overview

This project demonstrates the creation of a mini SOC (Security Operations Center) lab environment using VMware Workstation, pfSense, Splunk Enterprise, Rocky Linux, Windows 10, and Parrot OS.

The lab simulates real-world cyber attacks such as reconnaissance and brute-force attacks while collecting and analyzing logs through Splunk SIEM for threat detection and incident investigation.

## Repository Description

A hands-on Mini SOC Lab built with Splunk, pfSense, Rocky Linux, and VMware to simulate, detect, and investigate cyber attacks in a real-world environment.

## Objectives

- Build a virtual SOC environment
- Configure centralized logging
- Simulate real-world attacks
- Detect suspicious activities using Splunk
- Investigate attack behavior
- Create professional incident reports

## Tools & Technologies Used

| Tool | Purpose |
|------|---------|
| VMware Workstation | Virtualization |
| pfSense | Firewall and network routing |
| Splunk Enterprise | SIEM and log monitoring |
| Rocky Linux | Log server |
| Parrot OS | Attacker machine |
| Windows 10 | Monitoring system |
| rsyslog | Log collection and forwarding |

## Skills Demonstrated

- SIEM Monitoring
- Threat Detection
- Incident Investigation
- Log Analysis
- Firewall Configuration
- Linux Administration
- Network Security
- Splunk Querying
- Virtualization
- Security Monitoring

## Lab Architecture

```text
                    Internet
                        |
                    pfSense
                        |
 ------------------------------------------------
 |              |              |                |
Windows 10   Rocky Linux    Parrot OS     Windows Server
(Splunk)      (Log Server)   (Attacker)      (Target)
```

## Lab Environment Setup

### Step 1 - Virtualization Setup

Installed VMware Workstation to create and manage multiple virtual machines for the SOC environment.

#### Benefits

- Safe malware analysis and isolation
- Network simulation and security testing
- Snapshot and recovery capability

### Step 2 - pfSense Configuration

Configured pfSense as the firewall and router for the lab environment.

#### VM Configuration

- RAM: 2GB recommended
- CPU: 1-2 cores
- Disk: 20GB

#### Network Adapters

| Adapter | Purpose |
|---------|---------|
| WAN | NAT |
| LAN 1 | Windows 10 |
| LAN 2 | Windows Server |
| LAN 3 | Rocky Linux |
| LAN 4 | Parrot OS |

#### Initial Setup

- Assigned WAN and LAN interfaces
- Configured LAN IP
- Accessed web interface
- Configured firewall rules

Default access:

```bash
https://10.1.1.1
```
![SOC Lab Screenshot 02](screenshots/soc-lab-02.png)

![SOC Lab Screenshot 03](screenshots/soc-lab-03.png)


## Virtual Machines

### Windows 10

Used as the Splunk SIEM monitoring machine.

### Windows Server 2022

Target system for attack simulation.

#### Specifications

- 4GB RAM
- 60GB Storage

![SOC Lab Screenshot 04](screenshots/soc-lab-04.png)

![SOC Lab Screenshot 05](screenshots/soc-lab-05.png)
  

### Rocky Linux

Used as centralized logging server with rsyslog.

### Parrot OS

Used as attacker machine for penetration testing and attack simulation.

## rsyslog Installation & Configuration

Installed and configured rsyslog on Rocky Linux for centralized logging.

### Installation

```bash
sudo dnf update -y
sudo dnf install rsyslog -y
```

### Verify Installation

```bash
rsyslogd -v
```

### Start Service

```bash
sudo systemctl start rsyslog
sudo systemctl enable rsyslog
```

### Configuration File

```bash
/etc/rsyslog.conf
```

### Enable TCP & UDP Logging

Added:

```bash
$AllowedSender UDP, 10.1.3.0/24
```

### Restart rsyslog

```bash
sudo systemctl restart rsyslog
```

### Firewall Rules

```bash
sudo firewall-cmd --permanent --add-port=514/udp
sudo firewall-cmd --permanent --add-port=514/tcp
sudo firewall-cmd --reload
```

## Splunk SIEM Setup

Installed Splunk Enterprise on Windows 10 and Splunk Universal Forwarder on Rocky Linux.

### Splunk Access

```bash
http://localhost:8000
```

### Configuration Steps

- Enabled Splunk Forwarder
- Enabled receiving port 9997
- Restarted Splunk services
- Verified data ingestion

## Attack Simulation

### Reconnaissance Attack

Performed Nmap scans from Parrot OS.

#### SYN Scan

```bash
sudo nmap -sS <rocky-linux-ip>
```

#### Aggressive Scan

```bash
sudo nmap -A <rocky-linux-ip>
```

### Brute Force Attack

Used Hydra to simulate SSH brute-force attempts.

```bash
hydra -l admin -P rockyou.txt ssh://<windows-server-ip>
```

## Detection & Investigation

### Indicators Observed

- Multiple failed SSH login attempts
- Port scanning activity
- High volume authentication failures
- Suspicious repeated connection attempts

## Splunk Searches

### Failed SSH Logins

```spl
index=* "Failed password"
```

### Port Scan Detection

```spl
index=* "nmap"
```

### Authentication Monitoring

```spl
index=* ssh
| stats count by src_ip
```

## Incident Report

### Incident Title

Internal Reconnaissance and Brute Force Attack Detected

### Severity

High

### Executive Summary

Suspicious activity was detected originating from an internal Parrot OS machine. The attacker performed reconnaissance and brute-force attacks targeting a Rocky Linux server.

### Attack Details

#### Phase 1 - Reconnaissance

- Tool Used: Nmap
- Activity: Port scanning and service enumeration

#### Phase 2 - Brute Force

- Tool Used: Hydra
- Target Service: SSH
- Activity: Multiple failed authentication attempts

### Source & Target

| Type | IP Address |
|------|------------|
| Source IP | 10.1.4.100 |
| Target IP | 10.1.3.100 |

### Evidence Collected

- Failed password attempts in logs
- High log volume spikes
- Repeated SSH authentication failures
- Multiple network scan attempts

### Impact Assessment

- No confirmed successful compromise
- System exposed to reconnaissance activity
- Demonstrated vulnerability to password attacks

### Response Actions

- Identified malicious source IP
- Recommended blocking attacker IP using pfSense
- Suggested SSH hardening measures
- Recommended centralized monitoring improvements

## Recommendations

- Disable SSH password authentication
- Enable key-based authentication
- Implement IDS/IPS solutions
- Deploy Suricata or Snort on pfSense
- Enforce strong password policies
- Monitor repeated login failures

## Screenshots

### Extracted Lab Screenshots

![SOC Lab Screenshot 01](screenshots/soc-lab-01.png)

![SOC Lab Screenshot 06](screenshots/soc-lab-06.png)

![SOC Lab Screenshot 07](screenshots/soc-lab-07.png)

![SOC Lab Screenshot 08](screenshots/soc-lab-08.png)

![SOC Lab Screenshot 09](screenshots/soc-lab-09.png)

![SOC Lab Screenshot 10](screenshots/soc-lab-10.png)

![SOC Lab Screenshot 11](screenshots/soc-lab-11.png)

![SOC Lab Screenshot 12](screenshots/soc-lab-12.png)

![SOC Lab Screenshot 13](screenshots/soc-lab-13.png)

![SOC Lab Screenshot 14](screenshots/soc-lab-14.png)

![SOC Lab Screenshot 15](screenshots/soc-lab-15.png)

## Future Improvements

- Integrate Sysmon
- Deploy Active Directory
- Add phishing detection scenarios
- Implement automated alerting
- Integrate Suricata IDS/IPS
- Build custom Splunk dashboards

## Learning Outcomes

Through this project, I gained hands-on experience in:

- SOC Operations
- SIEM Deployment
- Threat Detection
- Log Monitoring
- Incident Investigation
- Linux Administration
- Firewall Management
- Security Monitoring

## Author

**Obu Chukwuemeka**

Aspiring SOC Analyst | Threat Detection | SIEM Monitoring | Incident Response | Splunk | pfSense | Linux | Cybersecurity Projects
