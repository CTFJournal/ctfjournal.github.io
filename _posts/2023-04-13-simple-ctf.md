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

The **python** exploit for **CVE-2019-9053** from [ExploitDB](https://www.exploit-db.com/) did not work for me, it seems to need some additional tweaking/adjustments. I found the working version of the exploit on [Github](https://raw.githubusercontent.com/e-renna/CVE-2019-9053/master/exploit.py) and executed it against the machine using the following command:

    python3 exploit.py -u http://$IP/simple --crack -w /usr/share/wordlists/seclists/Passwords/Common-Credentials/best110.txt

Below you'll see the exploit was successfully executed and password found.

 
![img5](/assets/images/easyctf/img5.png)

#### Q6: Where can you login with the details obtained?

With the found credentials let's attempt logging in via **SSH** on port **2222**



![img6](/assets/images/easyctf/img6.png)

The ssh login for **mitch:secret** is successful and we also have the user flag located at */home/mitch/user.txt*, which is the answer for question.

#### Q8: Is there any other user in the home directory?

![img7](/assets/images/easyctf/img7.png)

#### Q9: What can you leverage to spawn a privileged shell?

It looks like **mitch** can run **vim** with root privileges, which makes it simple to escalate privileges by executing **/bin/sh** via **vim**.

![img8](/assets/images/easyctf/img8.png)

This privilege escalation technique and many more can be found at [GTFOBins](https://gtfobins.github.io/gtfobins/vim/#sudo)

![img10](/assets/images/easyctf/img10.png)

#### Q10: What’s the root flag?

And having the root shell we can now read the content of the *root.txt* file located in the */root* directory.

![img9](/assets/images/easyctf/img9.png)

Whith this, the room is now complete! 
