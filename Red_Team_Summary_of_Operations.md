# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services
Nmap scan results for each machine reveal the below services and OS details:

```
$ nmap 192.168.1.0-255 -sV

```

This scan identifies the services below as potential points of entry:
- Target 1 192.168.1.110
  - 22/tcp  ssh          OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
  - 80/tcp  http         Apache http 2.4.10 ((Debian))
  - 111/tcp rpcbind      2-4 (RPC #100000)
  - 139/tcp netbios-ssn  Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
  - 445/tcp netbios-ssn  Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

The following vulnerabilities were identified on each target:
- Target 1
  - Easy password guess for user, michael, to ssh into.
  - SQL Database visible through michael’s shell.
  - Sudo Python capabilities for user steven



### Exploitation
The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - flag1(b9bbcb33e11b80be759c4e844862482d}
    - Weak Password
      - After guessing michael’s very weak password, I was able to find flag1 within the html 		directory.
      - (while within /var/www/html/ directory) ‘cat service.html | grep flag’



  - flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
    - Weak Password
      - After guessing michael’s very weak password, I was able to also find flag2 within the www 		directory.
      - (while within /var/www/ directory) cat flag2.txt



  - flag3{afc01ab56b50591e7dccf93122770cd2}
    - Easily accessible SQL Database
      - Concatenated a word press configuration file (wp-config.php) revealing the details to login 		to the SQL Database
      - I copied the database in the form of a backup to my kali’s host machine. Knowing I was 		searching for flag3, I used the command ‘cat database-backup.db | grep flag3’ to reveal 		the flag.



   - flag4{715dea6c055b9fe3337544932f2941ce}
    - steven’s Sudo Python Ability
      - ‘sudo -l’ showed that steven was capable of running python at the root level.
      - sudo python -c 'import pty; pty.spawn("/bin/sh")' opened up the root shell and led us to 		flag4.txt in root’s home directory. 
























