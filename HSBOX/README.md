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

The scan was used to identify open ports, service versions, and additional service information.

2. FTP Enumeration
The lab report identified anonymous FTP access.

Connect to the authorized lab target and retrieve the network capture file:

```text
get network_capture.pcap
```

The downloaded PCAP was analyzed using Wireshark.

3. PCAP Analysis with Wireshark
Use the following display filter to inspect HTTP POST requests:

```text
http.request.method == "POST"
```

The report documents discovering web application credentials in the captured traffic.

4. Web Application Enumeration
Open the target web service:

`http://192.168.1.75`

The website was identified as running Drupal. The next step was to investigate its version and potential vulnerabilities within the authorized lab.

5. Vulnerability Testing
The lab report describes using Metasploit to investigate a Drupal-related exploit and configure a reverse TCP Meterpreter payload.

The documented configuration included:
RHOSTS: `192.168.1.75`
RPORT: `80`
LHOST: `192.168.1.2`
LPORT: `4444`

The report records obtaining a Meterpreter session and accessing the target.

6. User and Flag Enumeration
After gaining lab access, the report describes inspecting the home directories and locating `flag1.txt` under the `jack` user's directory.

The `harry` directory was also investigated using the sudo permissions available in the lab, leading to the discovery of `flag2.txt`.

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

The report identified an executable named `iamhere` with the SUID bit set.

9. Privilege Escalation
The lab report documents executing `iamhere` and obtaining root access.

Investigate the executable's ownership, permissions, and behavior to understand the security implications of the SUID configuration.

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

