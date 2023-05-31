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
    * [Zoom](#zoom)
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



**Note**:  At the beggining of each CTF challenge, it's easier to create variables such as **IP** & **URL**, as oposed to always copy-pasting the IP adrress.

```powershell
#The syntax is the following:
export IP=10.0.0.1
export URL="http://10.0.0.1"
```
# ![img1](/assets/images/commandlist/img1.png)

------------------------------

### Scanning 

-----------
##### Nmap


```bash
nmap -sC -sV -p- $IP --open
nmap --script=vuln $IP

# -sV -> Version detection
# -sC -> Execute default scripts
# -A  -> OS detection and traceroute + the 2 above(sC,sV)
# -p- -> Scan all 65535 ports (the scan might take a great amount of time)

```


##### Netdiscover


```bash
netdiscover -r $IP/24

# -r IP/MASK: Network range(CIDR) to be scanned
# -l file: scan the list of ranges contained into the given file
# -i device: your network device

```

##### PingSweep

```vim

#!/bin/bash
#script will scan subnet 10.1.1.0/24 - adjust apropriately.

for ip in $(seq 1 254);do
    ping -c 1 10.1.1.$ip | grep "bytes from" | cut -d" " -f4 | cut -d":" -f1 &
done
```


##### Gobuster

```bash
gobuster dir -u $URL -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# -w -> specify wordlist to use
# dir -> directory/file enumeration mode
# -t -> number of concurrent threads
# -x -> file extensions to look for (for ex: txt,php,html,xml)
```

##### Wfuzz






------------------


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
