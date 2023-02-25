---
layout: post
title: "Linux Privilege Escalation"
---




***This post will be containing the entire walkthrough of the Linux PrivEsc room on [TryHackMe](https://www.tryhackme.com).***

----------------------------------------------------





















### Task1: Deploy the Vulnerable Debian VM 
   Nothing difficult in to do in this task, just click "Start Machine" and connect to it via ssh. Make sure you are connected to **tryhackme vpn**

   Use the following command to connect to the deployed machine: ***ssh tryhackme@MACHINE_IP*** and enter the password provided in the task description when prompted. You should now have access to the deployed machine's terminal.

![img1](/assets/images/linuxprivesc/img1.png)

To answer the question in this section, we simply need to run the ***"id"*** command
 
![img2](/assets/images/linuxprivesc/img2.png)


------------------------------------------------------------------------------

### Task2: Service Exploits 

In this task we will be exploiting MySQL service, which is running as root and the "root" user for the service does not have a password assigned. I have to admit the task is not so friendly and i had to use ChatGPT to understand what's going on behind the scenes of the **gcc** compiler. 

Navigate to: */home/user/tools/mysql-udf directory* and you'll see a file called *raptor_udf2.c*

![img3](/assets/images/linuxprivesc/img3.png)
This is uncompiled code and we need to compile it using **gcc** before being able to execute it

![img4](/assets/images/linuxprivesc/img4.png)

Let's connect to MySQL as root without a password usin command ***sudo mysql -u root***. Then execute some commands on the MySQL shell to create a User Defined Function wich we will use to copy */bin/bash* to */tmp/rootbash* and set the SUID permission. This will allow us gain a root shell when executing the binary from */tmp/rootbash*.  The output of the ***sql*** commands should look as below:

![img5](/assets/images/linuxprivesc/img5.png)


 Let's exit MySQL shell (type exit and press Enter). Then we can execute the /*tmp/rootbash* binary with ***-p*** and we get a shell running as root.


![img6](/assets/images/linuxprivesc/img6.png)



------------------------------------------------------------------------------

### Task3: Weak File Permissions - Readable /etc/shadow 

This task will teach us how can we exploit */etc/shadow* file, if we have read permissions on it. This file contains user password hashes and is usually readable only by the root user.

Looking at the file contents we can see the root hashed password in between the first 2 columns ::

![img7](/assets/images/linuxprivesc/img7.png)

I copied the hash and created a *hash.txt* file on my local machine then used **john** to crack the hash. You can also explore online tools to determine out the hashing algorythm. I used **hashid** utility.

![img8](/assets/images/linuxprivesc/img8.png)



![img9](/assets/images/linuxprivesc/img9.png)


![img10](/assets/images/linuxprivesc/img10.png)
![img11](/assets/images/linuxprivesc/img11.png)
![img12](/assets/images/linuxprivesc/img12.png)
![img13](/assets/images/linuxprivesc/img13.png)
![img14](/assets/images/linuxprivesc/img14.png)
![img15](/assets/images/linuxprivesc/img15.png)
![img16](/assets/images/linuxprivesc/img16.png)
![img17](/assets/images/linuxprivesc/img17.png)
![img18](/assets/images/linuxprivesc/img18.png)
![img19](/assets/images/linuxprivesc/img19.png)
![img20](/assets/images/linuxprivesc/img20.png)
![img21](/assets/images/linuxprivesc/img21.png)
![img22](/assets/images/linuxprivesc/img22.png)
![img23](/assets/images/linuxprivesc/img23.png)
![img24](/assets/images/linuxprivesc/img24.png)
![img25](/assets/images/linuxprivesc/img25.png)

