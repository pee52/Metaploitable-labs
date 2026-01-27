## Lab 06 – Linux Persistence & Misconfiguration Abuse

Target Information
Target: Metasploitable 2
Operating System: Linux
Initial Access Level: Root (obtained from Lab 05)
Attack Type: Post-Exploitation / Persistence
Severity: Critical

## Objective
The objective of this lab was to maintain persistent access to the compromised Linux system after successful privilege escalation. This was achieved by abusing system misconfigurations that allow long-term access even after reboot or credential changes.

## Initial Context
After completing Lab 05, full root privileges were obtained on the target system. With unrestricted administrative access, the attacker transitioned from privilege escalation to post-exploitation activities, focusing on persistence mechanisms that would allow continued access to the system.

## Enumeration
Post-exploitation enumeration focused on identifying persistence vectors, including:
-User accounts and group memberships
-Writable system files and directories
-Scheduled tasks (cron jobs)
-SSH configuration and authorized keys
-Service startup scripts
The goal was to identify misconfigurations that could be abused to maintain access without relying on the original exploit.

## Vulnerabilities Identified
Misconfigured System Persistence Mechanisms
Several persistence opportunities were identified due to weak system hardening:
-Ability to modify user accounts
-Writable cron directories
-Lack of monitoring on startup scripts
-Insecure SSH configuration
These weaknesses allow an attacker to establish long-term access without triggering alerts.
Vulnerability Type:
Post-Exploitation Misconfiguration / Persistence Weakness

## Exploitation (Persistence Setup)
With root access, persistence was established by modifying system-level configurations that execute automatically:
-Creation or modification of privileged user accounts
-Persistence via scheduled tasks
-Backdoor access through authorized services
These techniques ensure continued access even if the system is rebooted or if the initial vulnerability is patched.

## Proof of Exploitation
The following evidence confirms successful persistence:
-Screenshot showing creation or modification of persistent access
-Terminal output verifying continued access after session termination or reboot
-Verification of root privileges upon re-entry
Verification Output:

whoami
root

This confirms that persistent root-level access was successfully maintained.

## Result
-Persistent access was successfully established
-Root privileges were retained across sessions
-The system remained fully compromised
Final Access Level: Root (UID 0)

## Impact
With persistence in place, an attacker can:
-Maintain long-term unauthorized access
-Re-enter the system after reboot
-Bypass future patching of the original vulnerability
-Use the host as a pivot point for lateral movement
-Install stealth backdoors or malware
Impact Level: Critical

## Mitigation & Recommendations
To prevent persistence-based attacks, the following measures are recommended:
-Enforce strict access control on user and group management
-Monitor and audit cron jobs regularly
-Restrict write permissions on system and startup files
-Harden SSH configuration and monitor authorized keys
-Implement host-based intrusion detection systems (HIDS)

## Lessons Learned
Privilege escalation is not the final stage of an attack.
Without proper hardening, attackers can maintain access indefinitely using simple misconfigurations rather than complex exploits.

## Conclusion
This lab demonstrated how a compromised Linux system can remain under attacker control even after the initial vulnerability is addressed. 
By abusing weak persistence controls and system misconfigurations, an attacker with root privileges can ensure long-term access, making remediation significantly more difficult.
This highlights the importance of post-exploitation defenses, continuous monitoring, and strict system hardening—especially on legacy or internally exposed systems
