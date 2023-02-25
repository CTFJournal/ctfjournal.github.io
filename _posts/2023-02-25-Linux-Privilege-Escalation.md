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


### Task2: Service Exploits 

In this task we will be exploiting MySQL service, which is running as root and the "root" user for the service does not have a password assigned.

Navigate to: */home/user/tools/mysql-udf directory*