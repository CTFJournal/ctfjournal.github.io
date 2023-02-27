---
layout: post
title: "Linux Privilege Escalation"
---




***This post will be containing the entire walkthrough of the Linux PrivEsc room on [TryHackMe](https://www.tryhackme.com/room/linuxprivesc).***

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

-------------------------------------------------------------------------------

### Task7: Sudo - Environment Variables 

This section is explaining how can we use "sudo" to modify the environment variables that the programs run with. Environment variables are pieces of information that programs use to find things like libraries they need to run. By changing these variables, it's possible to run programs in a way that might not normally be allowed.

We have two examples of modifying environment variables using "sudo". Following the examples, we will modifiy the LD_PRELOAD and LD_LIBRARY_PATH variables, which control how shared libraries are loaded when programs run. Will compile a new shared library, then use "sudo" to run a program with that library preloaded.

For the first example i chose **nano** and spawned a root shell using the comands provided.

![img18](/assets/images/linuxprivesc/img18.png)

The second task is also easy to accomplish, since we have the commands available. 

![img19](/assets/images/linuxprivesc/img19.png)

-------------------------------------------------------------------------------

### Task8: Cron Jobs - File Permissions 

We'll learn some intersting stuff in this task, specifically how to expoit crontab. Crontab allows users to schedule automated tasks to run at specified times. It can be exploited if the crontab file is not properly secured or if the tasks it runs are vulnerable to attacks, allowing to execute malicious code or gain unauthorized access to the system.

Checking the **crontab** we see there's a cronjob being run periodicaly as **root** and the source is the file *overwrite.sh*. If we look at the file permissions, we see it allows **others** to write to it. 
So, we can replace the script content with our own code which will create a reverse tcp connection.

![img20](/assets/images/linuxprivesc/img20.png)

Let's replace the contents of the *overwrite.sh* file. Make sure you're changing the IP address to that of your Kali box.


![img21](/assets/images/linuxprivesc/img21.png)

Save the changes and close the text editor. Start a netcat listener on your local **Kali** machine and set the port to match the one from the script. Wait for about a minute and once the script is executed via crontab, you will get a reverse shell via netcat, as root.

![img22](/assets/images/linuxprivesc/img22.png)

-------------------------------------------------------------------------------

### Task9: Cron Jobs - PATH Environment Variable 


To break this down in the most simple terms, the task will be consisting of creating a script that's being executed as root via crontab.  The script will  copy the ***/bin/bash*** binary  to ***/tmp*** folder and rename it ***rootbash***

Checking the **crontab** to confirm we have ***/home/user/*** in the **PATH** and also we can see there's an entry called *overwrite.sh* being executed every minute.

![img23](/assets/images/linuxprivesc/img23.png)

Creating the script and making it executable with ***chmod +x overwrite.sh*** command.

![img24](/assets/images/linuxprivesc/img24.png)

We wait for about a minute, check ***/tmp*** directory to see if the *rootbash* is there and execute it to get a root shell.
![img25](/assets/images/linuxprivesc/img25.png)

-------------------------------------------------------------------------------

### Task10: Cron Jobs - Wildcards 

This task moves the focus on the other cron job script *compress.sh*. Taking  a look at its content we can the command "tar czf /tmp/backup.tar.gz *" being executed. This creates a compressed tar archive of all the files and directories in the current working directory and saves it to the file "/tmp/backup.tar.gz". Here is what each option in the command does:

    "tar" is a program used to create and manipulate tar archives.
    "c" option tells tar to create a new archive.
    "z" option tells tar to compress the archive using gzip.
    "f" option specifies the filename of the archive to create.
    "/tmp/backup.tar.gz" is the filename and path where the compressed tar archive will be saved.
    "*" represents all files and directories in the current working directory that will be included in the archive.

![img26](/assets/images/linuxprivesc/img26.png)

On [GTFOBins](https://gtfobins.github.io/gtfobins/tar/) we see that **tar** has command line options that let us run other commands, so we wil use this to execute a reverse shell.

![img27](/assets/images/linuxprivesc/img27.png)

Let us first create the payload for the reverse shell using **msfvenom**. Make sure to replace **LHOST** IP with your **Kali** machine's IP address

![img28](/assets/images/linuxprivesc/img28.png)
![img29](/assets/images/linuxprivesc/img29.png)
![img30](/assets/images/linuxprivesc/img30.png)
![img31](/assets/images/linuxprivesc/img31.png)
![img32](/assets/images/linuxprivesc/img32.png)

