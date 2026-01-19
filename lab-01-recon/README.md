# Lab 01: Initial Reconnaissance (Nmap)

## Objective
Perform initial network and service discovery against the Metasploitable 2 machine.

## Environment
- Attacker: Kali Linux
- Target: Metasploitable 2
- Network: Host-only / Internal network

## Reconnaissance Steps

## 1. Identify Target IP
Used basic network scanning to identify the target machine IP address.

## 2. Port Scanning
Performed a full TCP port scan using Nmap to enumerate open ports and services.

## Findings
-Multiple open TCP ports detected
-Legacy sevice exposed (FTP, Telnet, MySQL)
-FTP allows anonymous login

## Impact
- Anonymous FTP access may allow unauthorize users to read or upload files.
- Legacy services increase the attack surface and potential exploitation paths.

## Scan Result (Summary)
-21/tcp open ftp
-22/tcp open ssh
23/tcp open telnet
80/tcp open http
3306/tcp open mysql

## Example command:
```bash
  nmap -sS -p- <target>
  ftp <target>
```
## Conclusion 
The reconnaissance phase successfully mapped the exposed services and attack surface of target machine.
Legacy services and anonymouse access represent common real-world misconfigurations that can be exploited if left unpatched.

This information will guide deeper enumeration and exploitation in the next stages of the assessment.

