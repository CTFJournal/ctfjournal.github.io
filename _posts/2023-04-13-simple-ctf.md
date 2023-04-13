---
layout: post
title: "Simple CTF"
---



***Welcome to "Simple CTF" room on [TryHackMe](https://www.tryhackme.com/room/easyctf). This is a beginner level CTF we will be walking through.***

*security, enumeration, privesc* 

---------------------





*First connect to TryHackMe network and deploy the machine. If you need help, check this [guide](https://ctfjournal.github.io/Connect-to-TryHackMe-VPN/).*

--------------------------


We have the below series of questions to answer, for successfully completing the room. These kind of rooms are easier, given that we are more or less provided with the sequential flow [(cyber kill chain)](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html), to achieve our objective/pwn the machine.

    1 How many services are running under port 1000?
    2 What is running on the higher port?
    3 What's the CVE you're using against the application?
    4 To what kind of vulnerability is the application vulnerable?
    5 What's the password?
    6 Where can you login with the details obtained?
    7 What's the user flag?
    8 Is there any other user in the home directory? What's its name?
    9 What can you leverage to spawn a privileged shell?
    10 What's the root flag?


#### Q1: How many services are running under port 1000?

We can use **nmap** to scan & find out.


![img2](/assets/images/easyctf/img2.png)

#### Q2: What is running on the higher port?
Check the image above from Q1 and you will see what's the service running on port 2222

#### Q3: Which CVE are you using to exploit the application?

After conducting the nmap scan, multiple potential vulnerabilities were discovered on the machine, including:

    The FTP server permitting anonymous login, anyhow this turned out to be a false lead and is not exploitable.
    The webserver containing a robots.txt file that has one forbidden entry, but this won't lead to anything.

Let's do some scanning for hidden directories.


![img3](/assets/images/easyctf/img3.png)

The scan reveals a hidden directory named /simple/, which is utilizing a content management system called CMS Made Simple (CMSMS). The version number 2.2.8 is provided on the homepage.

Looking in [ExploitDB](https://www.exploit-db.com/) we find a higher version vulnerable to **SQLi** which most likely means our **2.2.8** version is also vulnerable to CVE-2019-9053

![img4](/assets/images/easyctf/img4.png)


#### Q5: What's the password?



![img5](/assets/images/easyctf/img5.png)
![img6](/assets/images/easyctf/img6.png)
![img7](/assets/images/easyctf/img7.png)
![img8](/assets/images/easyctf/img8.png)
![img9](/assets/images/easyctf/img9.png)
![img10](/assets/images/easyctf/img10.png)