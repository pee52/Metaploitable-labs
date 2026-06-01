# Lab 02: FTP Anonymous Login

## Objective
Identify and assess anonymous FTP access on the Metasploitable 2 machine.

## Environment
- Attacker: Kali Linux
- Target: Metasploitable 2
- Network: Host-only / Internal network

## Enumeration Steps

### 1. FTP Service Discovery
FTP service was identified running on port 21 during reconnaissance.

### 2. Anonymous Login Test
Tested anonymous login access to the FTP service.

## Findings
- FTP service allows anonymous authentication.
- Files and directories are accessible without credentials.

## Impact
- Anonymous FTP access may allow unauthorized users to read or upload files.
- This misconfiguration can be leveraged for further attacks.

## Example Commands
```bash
ftp <target-ip>
ls pwd cd
```
## Conclusion
The presence of anonymous FTP login represents a common misconfiguration that can expose sensitive information or enable further exploitation paths.
