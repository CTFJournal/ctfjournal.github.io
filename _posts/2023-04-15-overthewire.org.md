---
layout: post
title: "OverTheWire Wargames"
---



***[OverTheWire's](https://overthewire.org/wargames/) wargames offer a fun and effective way to learn and practice security concepts. Under this post you'll find the [Bandit](https://overthewire.org/wargames/bandit/) walkthrough, which is aimed at absolute beginners.***


---------------------













[OverTheWire](https://overthewire.org/wargames/) is a website that provides a series of challenges aimed at improving the security skills through practice. The challenges are categorized into different levels, each with a specific focus and difficulty, covering various aspects of hacking and cybersecurity, such as cryptography, web exploitation, reverse engineering, and more.

#### Bandit Level 0

Our objective for this level is to log in to the game using SSH. To establish a connection, we need to connect to the host "bandit.labs.overthewire.org" on port 2220. We should use the username "bandit0" and password "bandit0".



![img1](/assets/images/bandit_overthewire/img1.png)

In the home directory, check the contents of the README file, it should contain the SSH password for bandit1

![img2](/assets/images/bandit_overthewire/img2.png)


#### Bandit Level 1

After loging to level 1, we need the password for level 2 which is stored in a file called **-** located in the home directory. To be able to read a file with such name we need to provide either the current directory path **./-** or the absolute path to the file **/home/bandit1/-**, otherwise the **-** will be considered as an option for the **cat** command and we won't get anything.






![img3](/assets/images/bandit_overthewire/img3.png)


![img4](/assets/images/bandit_overthewire/img5.png)
![img6](/assets/images/bandit_overthewire/img4.png)
![img5](/assets/images/bandit_overthewire/img6.png)
![img7](/assets/images/bandit_overthewire/img7.png)
![img8](/assets/images/bandit_overthewire/img8.png)
![img9](/assets/images/bandit_overthewire/img9.png)
![img10](/assets/images/bandit_overthewire/img10.png)


