### Lab 05 – Linux Privilege Escalation

Target Information
Target: Metasploitable 2
Operating System: Linux
Initial Access Level: Low-privileged service user
Attack Type: Local Privilege Escalation
Severity: Critical

 ## Objective
The objective of this lab was to escalate privileges from a low-privileged user to root by identifying and exploiting local system misconfigurations on a Linux host.

## Initial Context
After gaining initial access to the target system, the attacker was operating under a non-root service account.
At this privilege level, full administrative control was not available, requiring further local enumeration to identify potential privilege escalation vectors.

## Enumeration
Local enumeration was performed to assess the system for misconfigurations that could lead to privilege escalation.
The enumeration focused on:
-Current user privileges and group memberships
-Operating system and kernel version
-File permissions and ownership
-Presence of SUID (Set User ID) binaries
During this phase, multiple binaries were identified with the SUID bit enabled, indicating potential privilege escalation opportunities.

## Vulnerability Identified
Misconfigured SUID Binaries
Several non-essential binaries were found with the SUID permission set, causing them to execute with the privileges of their file owner, which in this case was root.
This configuration allows a low-privileged user to abuse legitimate program functionality to execute commands with elevated privileges, resulting in a local privilege escalation vulnerability.
-Vulnerability Type:
-Local Privilege Escalation
Misconfiguration (Improper File Permissions)

## Exploitation
A vulnerable SUID binary capable of interacting with the underlying operating system was abused.
Because the binary executed with root privileges due to the SUID permission, it was possible to spawn a shell running as root, effectively bypassing standard permission restrictions and escalating privileges.

##  Attack Flow Summary:
1. Gained low-privileged access to the target system
2. Enumerated local system permissions and SUID binaries
3. Identified misconfigured SUID binary
4. Exploited SUID binary to spawn a root shell


## Proof of Exploitation
The following evidence confirms successful exploitation of a misconfigured SUID binary,
resulting in escalation of privileges to root.
-Screenshot showing enumeration results identifying SUID binaries
-Screenshot demonstrating execution of the vulnerable SUID binary
-Screenshot or terminal output confirming escalation to root privileges
Verification Output:
 
whoami
root

his output confirms that the attacker successfully obtained root-level access.

## Result
-Privilege escalation was successful
-Root access was obtained
-Final privilege level: root (UID 0)
-Full administrative control of the system was achieved

## Impact
With root privileges, an attacker can:
-Read, modify, or delete any files on the system
-Install persistent backdoors or malware
-Disable security mechanisms
-Use the compromised host as a pivot point to attack other systems within the network
Impact Level: Critical

## Mitigation & Recommendations
To prevent similar privilege escalation attacks, the following measures are recommended:
-Remove the SUID bit from non-essential binaries
-Restrict SUID usage to strictly required system components
-Apply the principle of least privilege
-Perform regular audits of file permissions
-Implement system hardening after OS installation

## Lessons Learned
Privilege escalation vulnerabilities are often caused by improper system configuration rather than advanced exploitation techniques.

## Conclusion
This assessment demonstrated that a low-privileged user was able to escalate privileges to root due to misconfigured SUID binaries on the target system.
The attack did not require complex exploits or advanced techniques, highlighting how basic permission misconfigurations can lead to complete system compromise. Once root access was obtained, the attacker gained unrestricted control over the operating system, resulting in severe security impact.
If left unaddressed in a production environment, this issue could allow attackers to fully compromise the system, access sensitive data, and leverage the host for further attacks. Proper permission management, regular audits, and system hardening are essential to mitigate such risks.
This highlights the importance of proper permission management and continuous security
hardening, even on internal or legacy systems.
