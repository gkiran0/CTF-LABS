
HSBOX2 – Cybersecurity Lab

Overview
HSBOX2 is a hands-on cybersecurity lab focused on network enumeration, FTP analysis, packet capture investigation, web application vulnerability testing, and Linux privilege escalation.

Objectives
- Identify open ports and running services.
- Enumerate anonymous FTP access.
- Analyze network traffic using Wireshark.
- Identify credentials from captured HTTP traffic.
- Investigate the Drupal web application.
- Obtain access to the target in the authorized lab.
- Explore Linux privilege escalation using SUID permissions.
- Locate the lab flags.

Lab Information
| Detail | Information |
|---|---|
| Machine Name | Feather |
| Lab | HSBOX2 |
| Target IP | `192.168.1.75` |
| Operating System | Linux (reported kernel range: 3.2–4.9) |
| Web Service | HTTP, port 80 |
| FTP Service | Port 21 |
| SSH Service | Port 22 |
| Tools | Nmap, FTP, Wireshark, Metasploit, Hydra |

Tools Used
Nmap – Port and service enumeration
FTP – Anonymous access and file retrieval
Wireshark – PCAP and HTTP traffic analysis
Hydra – Password auditing
Metasploit Framework – Vulnerability testing in the lab
Linux commands – File and user enumeration
Find – SUID file discovery

Methodology

1. Network Enumeration
Scan all TCP ports and identify running services.

bash
nmap -sC -sV -p- 192.168.1.75
<img width="1617" height="786" alt="nmap-service-enumeration" src="https://github.com/user-attachments/assets/4a35af78-6b1a-424c-8d19-693bfef25bd4" />


The scan was used to identify open ports, service versions, and additional service information.

2. FTP Enumeration
The lab report identified anonymous FTP access.

Connect to the authorized lab target and retrieve the network capture file:
<img width="1612" height="429" alt="ftp-anonymous-login" src="https://github.com/user-attachments/assets/4d7c0210-6150-48a8-ab04-ec2602d63139" />

```text
get network_capture.pcap
```

The downloaded PCAP was analyzed using Wireshark.

3. PCAP Analysis with Wireshark
Use the following display filter to inspect HTTP POST requests:

```text
http.request.method == "POST"
```
<img width="1621" height="848" alt="wireshark" src="https://github.com/user-attachments/assets/9da88b27-915d-468f-8966-d7f83e815688" />

The report documents discovering web application credentials in the captured traffic.

4. Web Application Enumeration
Open the target web service:

`http://192.168.1.75`
<img width="1616" height="805" alt="drupal dashboard" src="https://github.com/user-attachments/assets/dd1d904d-e250-4f8a-8384-3314738d8f42" />

The website was identified as running Drupal. The next step was to investigate its version and potential vulnerabilities within the authorized lab.

5. Vulnerability Testing
The lab report describes using Metasploit to investigate a Drupal-related exploit and configure a reverse TCP Meterpreter payload.
<img width="1620" height="788" alt="metasploit drupal search" src="https://github.com/user-attachments/assets/70f2f9d5-191d-44ba-b2ad-25e5ae68480a" />
<img width="1619" height="768" alt="metasploit-exploit-configuration" src="https://github.com/user-attachments/assets/1a4e2627-67e6-45b6-86de-51ce94a007d6" />

The documented configuration included:
RHOSTS: `192.168.1.75`
RPORT: `80`
LHOST: `192.168.1.2`
LPORT: `4444`

The report records obtaining a Meterpreter session and accessing the target.

6. User and Flag Enumeration
After gaining lab access, the report describes inspecting the home directories and locating `flag1.txt` under the `jack` user's directory.
<img width="1614" height="667" alt="jack user flag1" src="https://github.com/user-attachments/assets/140f92d5-1db6-4c85-8a15-0c3bfb6cbf0a" />

The `harry` directory was also investigated using the sudo permissions available in the lab, leading to the discovery of `flag2.txt`.
<img width="1151" height="884" alt="harry user flag2" src="https://github.com/user-attachments/assets/c9564aba-2ead-4529-a70b-7b6103df9205" />


7. Shell Access
The report documents upgrading to an interactive shell using Python PTY:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

This can improve shell interaction when the target environment supports the required Python version.

8. SUID Enumeration
Search for SUID-enabled files:

```bash
find / -perm -4000 -type f 2>/dev/null
```
<img width="1190" height="592" alt="sudo permission enumeration" src="https://github.com/user-attachments/assets/0ea47b00-8d80-4140-bb44-29d4d347b74d" />


The report identified an executable named `iamhere` with the SUID bit set.

9. Privilege Escalation
The lab report documents executing `iamhere` and obtaining root access.

Investigate the executable's ownership, permissions, and behavior to understand the security implications of the SUID configuration.
<img width="1292" height="398" alt="root privilege escalation" src="https://github.com/user-attachments/assets/38717cdb-8156-40a2-91a5-a8480f83c684" />
10. Final Flag Discovery

The report describes accessing `/root/flag.txt` after obtaining root privileges.

Record the flag results in your lab notes and follow the training provider's rules before publishing flag values.

Key Learnings
- Network and service enumeration
- FTP anonymous-access testing
- Packet capture and HTTP traffic analysis
- Web application vulnerability investigation
- Metasploit lab usage
- Linux user and file enumeration
- SUID permission analysis
- Privilege-escalation investigation
- Security evidence documentation
Aspiring Cybersecurity Professional | SOC Analyst / VAPT

