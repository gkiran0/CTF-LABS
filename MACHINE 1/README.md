Machine 1 – Cybersecurity Lab

Overview

Machine 1 – hands-on Linux cybersecurity lab focused on network enumeration, SMB user discovery, MySQL security assessment, password hash cracking, SSH access, restricted shell analysis, and Linux privilege escalation.

The objective was to identify exposed services, discover valid user credentials, obtain authorized access, investigate misconfigurations, and achieve root access within the lab environment.

Objectives

- Perform network scanning and identify open ports.
- Enumerate SMB and NetBIOS services.
- Discover usernames and shared resources.
- Assess MySQL authentication security.
- Extract and analyze password hashes.
- Perform password auditing.
- Obtain SSH access to the target.
- Investigate restricted Bash configurations.
- Identify SUID-enabled executables.
- Analyze privilege-escalation vulnerabilities.
- Obtain root access and locate the final flag.

Lab Information

| Detail | Information |
|---|---|
| Machine Name | Unfold |
| Hostname | Unfold |
| Operating System | Linux |
| Target IP | `10.0.0.168` |
| Domain | N/A |
| Network Response | ICMP enabled |
| Firewall | Reported as enabled |
| Main Services | SSH, HTTP, SMB, MySQL |

Tools Used

Nmap – Network and port scanning
enum4linux – SMB and NetBIOS enumeration
Medusa – Password auditing
MySQL Client – Database enumeration
CrackStation – Password hash analysis
SSH – Remote system access
Linux Commands – User and file enumeration
Wget – File retrieval and SUID misconfiguration analysis

Methodology

1. Network Enumeration – Nmap

Performed network scanning to identify open ports and running services.

```bash
nmap 10.0.0.168
```
<img width="734" height="349" alt="Screenshot 2026-10-09 165611" src="https://github.com/user-attachments/assets/57acbf10-fb89-42ac-8ba4-6f9b78878bb1" />

2. SMB Enumeration – enum4linux

Used enum4linux to enumerate SMB and NetBIOS information.

```bash
enum4linux -a 10.0.0.168
```
<img width="974" height="453" alt="Screenshot 2026-10-09 165813" src="https://github.com/user-attachments/assets/abeafe2c-924d-4595-9ff7-96bdcf40fb9e" />

Discovered the username `harry`.
<img width="968" height="607" alt="Screenshot 2026-10-09 165836" src="https://github.com/user-attachments/assets/6611ef02-7b1a-4df5-8776-4106465e21a8" />


3. MySQL Enumeration

Identified MySQL running on port 3306.

Performed password auditing against the MySQL service using Medusa.

```bash
medusa -h 10.0.0.168 -u root -P <password-list> -M mysql
```

<img width="1918" height="688" alt="Screenshot 2026-10-12 180805" src="https://github.com/user-attachments/assets/5fd887bb-ae5c-4911-9fae-a5bfa9b37929" />


4. Database and Password Hash Analysis

Accessed the MySQL database and investigated the user table.

```sql
SELECT * FROM user;
```

Identified a password hash associated with the Harry user account.
<img width="950" height="271" alt="Screenshot 2026-10-07 185629" src="https://github.com/user-attachments/assets/f9d0b6f3-24f2-4ee9-8172-22f7e9f780b6" />


Used CrackStation to analyze the hash and recover the corresponding password in the lab.

<img width="985" height="563" alt="Screenshot 2026-10-07 175235" src="https://github.com/user-attachments/assets/92b85761-2d62-4d1a-8ec2-e557b2a98bd8" />

5. SSH Access

Used the recovered lab credentials to establish an SSH connection.

```bash
ssh harry@10.0.0.168
```
<img width="1075" height="444" alt="Screenshot 2026-10-12 180848" src="https://github.com/user-attachments/assets/d8b5e75e-bc12-407f-8347-09096f4b7e10" />

Successfully obtained a shell as the Harry user.

6. Restricted Bash Analysis

The Harry account initially used a restricted Bash environment.

The Harry user have restricted rbash shell .
<img width="1919" height="719" alt="image" src="https://github.com/user-attachments/assets/a66049d7-abf1-45c5-87a5-4c96ab65ad7f" />



```bash
chsh -s /bin/bash
```

After restarting the machine, verified the updated shell configuration.
<img width="1096" height="724" alt="Screenshot 2026-10-09 155150" src="https://github.com/user-attachments/assets/9ae9d289-e0ed-470e-9262-56eec8ab366d" />


7. SUID Enumeration

Searched for SUID-enabled files to identify potential privilege-escalation opportunities.

```bash
find / -perm -u=s -type f 2>/dev/null
```

<img width="751" height="483" alt="Screenshot 2026-10-13 175138" src="https://github.com/user-attachments/assets/a4f74efc-9fa3-4641-98c5-ab1fcb7a7a87" />


identified the `wget` executable with SUID permissions and root ownership.

8. Privilege Escalation Analysis

Investigated the security implications of the misconfigured SUID-enabled Wget executable.
<img width="925" height="268" alt="Screenshot 2026-10-09 162434" src="https://github.com/user-attachments/assets/1b2f4f29-04cd-435a-b59e-94fad8ef0ca3" />


The demonstrated how insecure file permissions and privileged file operations could allow modification of the system's `/etc/passwd` file.

<img width="1784" height="644" alt="Screenshot 2026-10-13 175425" src="https://github.com/user-attachments/assets/d7251552-f758-4d47-82a8-cba701443723" />

This configuration resulted in unauthorized root-level access within the controlled lab environment.

9. Root Access and Flag Discovery

Successfully obtained root access and navigated to the root user's directory.

The final lab flag was located in:

```text
/root/flag.txt
```
<img width="709" height="162" alt="Screenshot 2026-10-09 162517" src="https://github.com/user-attachments/assets/3b32d21f-cbf2-4fef-8e8a-d1f7480aefa2" />

The flag was verified as part of the lab completion process.

Key Skills Demonstrated

- Network Enumeration
- Port and Service Discovery
- SMB / NetBIOS Enumeration
- MySQL Security Assessment
- Password Auditing
- Password Hash Analysis
- SSH Authentication
- Linux User Enumeration
- Restricted Shell Analysis
- SUID Permission Enumeration
- Linux Privilege Escalation
- Security Documentation
