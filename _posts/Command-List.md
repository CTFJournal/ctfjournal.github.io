---
layout: post
title: "Useful Commands"
---


-----

## A list of most used commands & tools across the CTF cycle


----

### Table of Contents

* **[Scanning](#scanning)**
    * [Nmap](#nmap)
    * [NetDiscover](#netdiscover)
    * [PingSweep](#pingsweep)
    * [Gobuster](#gobuster)
    * [Wfuzz](#wfuzz)
    * [WpScan](#wpscan)
    * [Nikto](#nikto)


* **[Gaining Access](#gaining-access)**
    * [Metasploit](#metasploit)
    * [Sqlmap](#sqlmap)
    * [Hydra](#hydra)


* **[Enumeration/PrivEsc](#enumeration/privesc)**
    * [LinEnum](#linenum)
    * [WinPEAS](#winpeas)
    * [PowerUp](#powerup)

* **[Cracking](#cracking)**
    * [Hashcat](#hashcat)
    * [John](#john)
    * [Stegcracker](#stegcracker)

* **[Extra](#extra)**
    * [Shells](#shells)
    * [SMB](#smb)
    * [FTP](#ftp)
    * 



------------------------------

### Scanning 

-----------
##### Nmap


```bash
nmap -sC -sV -p- $IP --open

# -sV -> Version detection
# -sC -> Execute default scripts
# -A  -> OS detection and traceroute + the 2 above(sC,sV)
```


##### PingSweep

```vim

#!/bin/bash
#script will scan subnet 10.1.1.0/24 - adjust apropriately.

for ip in $(seq 1 254);do
    ping -c 1 10.1.1.$ip | grep "bytes from" | cut -d" " -f4 | cut -d":" -f1 &
done
```

### References
* Nmap. https://nmap.org/
* DirBuster. https://sourceforge.net/projects/dirbuster/
* Nikto. https://cirt.net/Nikto2
* Metasploit Framework. https://www.metasploit.com/
* Hydra. https://github.com/vanhauser*thc/thc*hydra
* John the Ripper. https://www.openwall.com/john/
* LinEnum. https://github.com/rebootuser/LinEnum
* Linux Exploit Suggester. https://github.com/mzet*/linux*exploit*suggester
* Windows Exploit Suggester. https://github.com/GDSSecurity/Windows*Exploit*Suggester
