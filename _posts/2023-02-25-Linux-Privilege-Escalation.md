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
It took a few seconds to crack the password using **john**.

![img8](/assets/images/linuxprivesc/img8.png)


We can now log in as root, executing ***su root*** and providing the password discovered at the previous step.

![img9](/assets/images/linuxprivesc/img9.png)


------------------------------------------------------------------------------

### Task4: Weak File Permissions - Writable /etc/shadow 

As we have learned in the previous task, */etc/shadow* file contains user password hashes and is usually readable only by the root user. Allowing anyone to read the file, will potentially cause having the password cracked as there are multiple tools that can be used to perform hash cracking (**john, hashcat**, or the web)

In this task, we can even write to the */etc/shadow*, meaning that we can create a password hash ourselves and replace it with the root hash from the file, which will allow us to login as root using the new password.

Longlisting the */etc/shadow* file we can see that it is writable by *others*.  So, we generate the hash for the password *password123*.

![img10](/assets/images/linuxprivesc/img10.png)

Now we can use a text editor, **nano** or **vim** to edit the */etc/shadow* file and replace the root password hash with the hash we have generated for *password123*. 

![img11](/assets/images/linuxprivesc/img11.png)

Save the changes & close the editor. Then try logging in as root and the  password you set. You should have a root shell and you've successfuly escalated the privileges.

------------------------------------------------------------------------------

### Task5: Weak File Permissions - Writable /etc/passwd 


The /etc/passwd file contains a list of all the user accounts on the system, including usernames, user IDs, group IDs, home directories, and shell information.

If the /etc/passwd file is made writeable, it would allow anyone with write access to modify the file, which can be a significant security risk. An attacker could potentially modify the file to grant themselves elevated privileges or create new user accounts with administrative access. We will learn how to do it in this task.

We first check that the file is writable then we generate a password hash for *password123* using **openssl**


![img12](/assets/images/linuxprivesc/img12.png)

I copied the entire root line from the beggining of the */etc/passwd* file and added it at the end of the file, replacing the root name with *newroot* and added password hash *UxiDZo1GyzcK.*

![img13](/assets/images/linuxprivesc/img13.png)

Now we can switch user to *newroot* and use the password we have created earlier.

![img14](/assets/images/linuxprivesc/img14.png)

------------------------------------------------------------------------------

### Task6: Sudo - Shell Escape Sequences 

**sudo -l** is a command used in Unix-based systems to list the permissions that a user has to perform administrative tasks using the sudo command. It helps users understand the extent of their administrative capabilities and ensures that they are only running authorized commands with elevated privileges. It is also useful for administrators to manage the permissions granted to users through the sudoers file.

We can look through [GTFOBins](https://gtfobins.github.io) to find various methods for elevating privileges using binaries the user is allowed to run as root.

Let's verify and see what are the programs the current user can run with elevated pivileges.

![img15](/assets/images/linuxprivesc/img15.png)

It looks like we quite a few options. I picked the 1st binay in the list [iftop](https://gtfobins.github.io/gtfobins/iftop/#sudo). Looking at GTFObins we see *the binary is allowed to run as superuser by sudo, and may be used to access the file system, escalate or maintain privileged access* by executing ***sudo iftop*** and then *** ***!/bin/sh***

![img16](/assets/images/linuxprivesc/img16.png)

Following the instructions we quickly get a root shell :) 




![img17](/assets/images/linuxprivesc/img17.png)

------------------------------------------------------------------------------




![img18](/assets/images/linuxprivesc/img18.png)



![img19](/assets/images/linuxprivesc/img19.png)



![img20](/assets/images/linuxprivesc/img20.png)



![img21](/assets/images/linuxprivesc/img21.png)



![img22](/assets/images/linuxprivesc/img22.png)



![img23](/assets/images/linuxprivesc/img23.png)



![img24](/assets/images/linuxprivesc/img24.png)
![img25](/assets/images/linuxprivesc/img25.png)

