# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

# Exposed Services

    `nmap -sP 192.168.1.0/24`
Screenshot

    Name of VM: Target 2
    Operating System: Linux
    Purpose: Offensive Red Team
    IP Address: 192.168.1.115

Nmap scan results for each Target 2 reveal the below services and OS details:

    `nmap -sV 192.168.1.115`
Screnshot

This scan identifies the services below as potential points of entry:

- Port 22/tcp open ssh (service) OpenSSH 6.7p1 Debian 5+deb8u4
- Port 80/tcp open http (service) Apache httpd 2.4.10 ((Debian))
- Port 111/tcp open rpcbind (service) 2-4 (RPC #100000)
- Port 139/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X
- Port 445/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X

The following vulnerabilities were identified on each target:
- Remote Code Execution Vulnerability in PHPMailer  (CVE-2016-10033)
- Open SSH (CVE-2021-28041)
- Apache https 2.4.10 (CVE-2017-15710)
- Exploit on open rpcbind port could lead to remote DoS(CVE-2017-8779)
- Samba NetBIOS (CVE-2017-7494)

# Critical Vulnerabilities

The following vulnerabilities were identified on Target 2:

    - Remote Code Execution Vulnerability in PHPMailer, (CVE-2016-10033):
     BGet access to the web services and search for a lot of confidential information. Exploiting PHPMail with back connection (reverse shell) from the target.

    - Network Mapping and User Enumeration (WordPress site):
    Nmap was used to discover open ports. Able to discover open ports and tailor their attacks accordingly.

    - Weak Root Password:
    The root login had a weak password and the attackers were able to discover it by guessing. Able to correctly guess a root's password.

    - Misconfiguration of User Privileges/Privilege Escalation:
    The attackers noticed that the root user has sudo privileges for python. Able to utilize root’s python privileges in order to escalate for privilege to other folders.

# Exploitation

The Red Team was able to penetrate `Target 2` and retrieve the following confidential data:
## `flag1.txt`: SCREENSHOT hash-value: flag1{a2c1f66d2b8051bd3a5874b5b6e43e21
### Exploit Used:
- Enumerated WordPress site with Nikto and Gobuster to create a list of exposed URLs from the Target HTTP server and gather version information.
- Command: `nikto -C all -h 192.168.1.115`
Screenshot:
Determined the website is running on Apache/2.4.10 (Debian). Performed a more in-depth enumeration with Gobuster.
- Command: `sudo apt-get update`
- Command: `sudo apt-get install gobuster`
- Command: `gobuster -w /usr/share/wordlists/dirbuster directory-list-2.3-medium.txt dir -u 192.168.1.115`

Screenshot:

## `flag2.txt`: SCREENSHOT hash-value:
### Exploit Used:
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
## `flag3.txt`: SCREENSHOT `flag2.txt` hash value_
### Exploit Used:
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
## `flag4.txt`:  SCREENSHOT `flag2.txt` hash value_
### Exploit Used:
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_