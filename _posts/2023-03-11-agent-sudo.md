---
layout: post
title: "Agent Sudo"
---



***[TryHackMe](https://tryhackme.com/room/agentsudoctf) room description: You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.***




---------------------





*First connect to TryHackMe network and deploy the machine. For help, check this [guide](https://ctfjournal.github.io/Connect-to-TryHackMe-VPN/).*



----------------------------








### Enumerate   

Let's run the nmap scan & see what we can discover. I add **-p-** for scanning all 65535 ports and **--open** to only perform script scan & version detection on the open ports.


![img1](/assets/images/agent-sudo/img1.png)


We have 3 open ports, and we can start by checking if we can find anything interesting on the **web**. Navigatig to the page we see the message below. Nothing else in the page source


![img2](/assets/images/agent-sudo/img2.png)


Fired up gobuster to search for hidden directories but found nothing. 


![img3](/assets/images/agent-sudo/img3.png)

As you'll see below, i also tried **ftp** but no luck either, anonymous login is disabled. Moreover this is not the direction the room instructions are pointing us to, so let's return to the website.

![img4](/assets/images/agent-sudo/img4.png)

The message from the webpage is pointing us to the fact that each user should use his codename as user-agent to be directed the the site. The codename for this agent seems to be **"R"**. We can check that using **curl** and even change the user-agent if needed. 


![img5](/assets/images/agent-sudo/img5.png)

Tried with user-agent as **"R"** and we see some additional output. 1 Letter, 25 employees, naturally should be set following the alphabetic order, so i did sequentially switched the letter, starting with **"A"**. We do get something at the letter **"C"** and we now know the username and that he also has a weak password.

![img6](/assets/images/agent-sudo/img6.png)

-------

### Hash cracking and brute-force 

First thing we would usually do, is to attempt ftb bruteforcing. For that i'll use **Hydra**, The passwod is found quickly using **rockyou** wordlist

![img7](/assets/images/agent-sudo/img7.png)


I've logged in to **ftp** using the found credentials and we have some files there. I downloaded all of them to the local machine.


![img8](/assets/images/agent-sudo/img8.png)

The content of the file ***"To_agentJ.txt"***

    Dear agent J,

    All these alien like photos are fake! Agent R stored the real picture inside your directory. 
    Your login password is somehow stored in the fake picture. It houldn't be a problem for you.

    From,
    Agent C


Verifying the image with **steghide** we find out it is password protected from extraction, which probably means there's something hidden inside it. I used **stegcracker** to find the password

![img9](/assets/images/agent-sudo/img9.png)

Then we can extract the hidden content which is going to reveal user James and their password

![img10](/assets/images/agent-sudo/img10.png)

Let's login via SSH as James. There we can find the user flag, along with an image for which we'll have to do a bit a revese image search to respond to the last question in this section.

-----------


### Privilege escalation 

![img11](/assets/images/agent-sudo/img11.png)


Looking at the above output: The sudo configuration allows the user "james" to run the "/bin/bash" command with the privileges of any user except for the root user. This restriction is denoted by the "!root" entry in the sudoers configuration. By explicitly specifying the allowed command ("/bin/bash") and the restriction on the target user ("!root"), it seems that the configuration aims to limit the commands that "james" can execute with elevated privileges, preventing potential misuse.

The PrivEsc on this room is about CVE-2019-14287, a vulnerability that allows a user with sudo privileges to bypass intended restrictions and execute arbitrary commands as the target user, even if those commands were not explicitly allowed in the sudoers configuration file. This means that an attacker with limited privileges could escalate their privileges and gain unauthorized access to sensitive resources or execute malicious commands on the system.

The vulnerability arises from an error in the sudo command's policy for user authentication. By using a specially crafted command sequence that includes specifying the user ID "-1" or "4294967295" (equivalent to "ALL" or "ANY"), an attacker can exploit this flaw and execute commands they shouldn't have permission for.

![img12](/assets/images/agent-sudo/img12.png)


-------------------