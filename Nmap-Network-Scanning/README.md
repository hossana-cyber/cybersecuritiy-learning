# Nmap Network Scanning & Vulnerability Assessment

## Overview

This project documents a practical network reconnaissance, service enumeration, vulnerability assessment, and exploitation exercise performed in an authorized Metasploitable laboratory environment.

The objective was to use Nmap and Metasploit to identify exposed services, determine service versions, verify a known vulnerability, demonstrate its security impact, and document appropriate remediation measures.

---

## Lab Environment

| Component | Details |
|---|---|
| Attacker Machine | Kali Linux |
| Target Machine | Metasploitable |
| Target IP | `192.168.195.131` |
| Kali Linux IP | `192.168.195.136` |
| Lab Network | `192.168.195.0/24` |
| Virtualization | VMware |

---

## Tools Used

- Nmap
- Metasploit Framework
- Kali Linux
- Metasploitable

---

## Assessment Workflow

### 1. Reconnaissance

The initial reconnaissance phase identified the Kali Linux system and the authorized lab target.

**Kali Linux IP:**
`192.168.195.136/24`

**Target IP:**
`192.168.195.131`

**Lab Network:**
`192.168.195.0/24`

---

### 2. Network Discovery

Nmap host discovery was performed to determine whether the target was active and reachable.

```bash
sudo nmap -sn 192.168.195.131
```

**Result:**

The target responded as an active host.

---

### 3. Basic Port Scanning

A standard Nmap scan was performed to identify exposed TCP ports and associated services.

```bash
sudo nmap 192.168.195.131
```

The scan identified **23 open TCP ports** among the 1,000 ports scanned.

Examples of exposed services included:

- FTP
- SSH
- Telnet
- SMTP
- DNS
- HTTP
- SMB
- NFS
- MySQL
- PostgreSQL
- VNC
- IRC
- Apache Tomcat

An open port does not by itself prove that a service is vulnerable, so further service enumeration was performed.

---

### 4. Service & Version Enumeration

Nmap service detection was used to identify the software and versions running on the exposed ports.

```bash
sudo nmap -sV 192.168.195.131
```

Important findings included:

| Port | Service | Detected Version |
|---|---|---|
| 21/tcp | FTP | vsftpd 2.3.4 |
| 22/tcp | SSH | OpenSSH 4.7p1 |
| 80/tcp | HTTP | Apache 2.2.8 |
| 139/tcp | SMB | Samba 3.X - 4.X |
| 445/tcp | SMB | Samba 3.X - 4.X |
| 3306/tcp | MySQL | MySQL 5.0.51a |
| 5432/tcp | PostgreSQL | PostgreSQL 8.3.x |
| 5900/tcp | VNC | VNC 3.3 |
| 8180/tcp | HTTP | Apache Tomcat |

The FTP service running **vsftpd 2.3.4** was selected for further investigation.

---

### 5. Vulnerability Assessment

The vsftpd service was specifically tested for the known vsFTPd 2.3.4 backdoor vulnerability.

```bash
sudo nmap -p 21 --script ftp-vsftpd-backdoor 192.168.195.131
```

The vulnerability check identified:

**Vulnerability:** vsFTPd 2.3.4 Backdoor

**CVE:** CVE-2011-2523

**Affected Service:** FTP — Port 21

The Nmap vulnerability script reported the service as vulnerable and identified the associated backdoor behavior.

This finding was subsequently investigated using Metasploit in the authorized laboratory environment.

---

### 6. Exploitation

Metasploit Framework was used to demonstrate the security impact of the identified vulnerability.

**Module:**

```text
exploit/unix/ftp/vsftpd_234_backdoor
```

**Target:**

```text
192.168.195.131
```

**Port:**

```text
21
```

The exploitation attempt successfully opened a command shell on the Metasploitable target.

The resulting shell was verified with:

```bash
id
```

Result:

```text
uid=0(root) gid=0(root)
```

The current user was also verified:

```bash
whoami
```

Result:

```text
root
```

This demonstrated that exploitation of the vulnerable service could result in **root-level access** to the laboratory target.

---

## Security Impact

The vulnerability presents a significant security risk because successful exploitation can provide unauthorized command execution with root privileges.

Potential impact includes:

- Unauthorized system access
- Execution of commands with elevated privileges
- Compromise of system confidentiality
- Modification or deletion of system data
- Further compromise of services running on the host

The demonstration was conducted exclusively against the authorized Metasploitable laboratory target.

---

## Remediation

Recommended remediation measures include:

1. Remove the vulnerable vsftpd 2.3.4 installation.
2. Upgrade to a secure and supported FTP implementation/version where FTP is required.
3. Disable FTP if it is not necessary.
4. Restrict access to required services using firewall rules and network segmentation.
5. Remove unnecessary or exposed services.
6. Regularly scan systems for outdated software and known vulnerabilities.
7. Monitor network traffic and system logs for suspicious activity.
8. Apply security updates and maintain supported software versions.

---

## Evidence

Practical evidence from the assessment is available in the `screenshots` directory.

The screenshots document:

1. [Basic Nmap Scan](screenshots/1-nmap-basic-scan.png)
2. [Network Discovery](screenshots/2-network-discovery.png)
3. [Basic Port Scan](screenshots/3-nmap-basic-scan.png)
4. [Service & Version Detection](screenshots/4-service-version-scan.png)
5. [Vulnerability Assessment](screenshots/5-vulnerability-assessment.png)
6. [Vulnerability Assessment2](screenshots/6-msfconsole.png)
7. [Exploitation](screenshots/7-msfconsole.png)
8. [Exploitation2](screenshots/8-msfconsole.png)
9. [Exploitation3](screenshots/9-msfconsole.png)

---

## Key Skills Demonstrated

- Network reconnaissance
- Host discovery
- Nmap port scanning
- Service and version enumeration
- Vulnerability identification
- Vulnerability verification
- Metasploit Framework
- Exploitation in a controlled environment
- Root-shell verification
- Security impact assessment
- Vulnerability remediation
- Security documentation

---

## Ethical Considerations

All scanning and exploitation activities documented in this project were performed against a deliberately vulnerable **Metasploitable laboratory system** that was authorized for testing.

The techniques demonstrated here should only be used against systems that you own or have explicit permission to test.

---

## Project Structure

```text
Nmap-Network-Scanning
├── README.md
├── 1. reconnaissance.txt
├── 2. network-discovery.txt
├── 3. nmap-basic-scan.txt
├── 4. nmap-service-version-scan.txt
├── 5. vulnerability-assessment.txt
├── 6. exploitation.txt
├── 7. remediation.txt
└── screenshots
    ├── 1-nmap-basic-scan.png
    ├── 2-network-discovery.png
    ├── 3-nmap-basic-scan.png
    ├── 4-service-version-scan.png
    ├── 5-vulnerability-assessment.png
    └── 6-exploitation.png
```
