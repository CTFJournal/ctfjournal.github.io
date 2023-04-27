---
layout: post
title: "OverTheWire Wargames"
---



***[OverTheWire's](https://overthewire.org/wargames/) wargames offer a fun and effective way to learn and practice security concepts. Under this post you'll find the [Bandit](https://overthewire.org/wargames/bandit/) walkthrough, which is aimed at absolute beginners.***


---------------------













[OverTheWire](https://overthewire.org/wargames/) is a website that provides a series of challenges aimed at improving the security skills through practice. The challenges are categorized into different levels, each with a specific focus and difficulty, covering various aspects of hacking and cybersecurity, such as cryptography, web exploitation, reverse engineering, and more.

----------

### Bandit Level 0

Our objective for this level is to log in to the game using SSH. To establish a connection, we need to connect to the host "bandit.labs.overthewire.org" on port 2220. We should use the username "bandit0" and password "bandit0".



![img1](/assets/images/bandit_overthewire/img1.png)

In the home directory, check the contents of the README file, it should contain the SSH password for bandit1

![img2](/assets/images/bandit_overthewire/img2.png)

----------

### Bandit Level 1

After loging to level 1, we need the password for level 2 which is stored in a file called **"-"(dash)** located in the home directory. To be able to read a file with such name we need to provide either the current directory path **./-** or the absolute path to the file **/home/bandit1/-**, otherwise the **-** will be considered as an option for the **cat** command and we won't get anything.






![img3](/assets/images/bandit_overthewire/img3.png)

------------

### Bandit Level 2

Level 2 → Level 3 task is to find the password in a file which has spaces in it's name. To read the contents of such file we can use the **" \ "** before each space, or double **TAB** to autocomplete.

![img4](/assets/images/bandit_overthewire/img4.png)

--------

### Bandit Level 3

Now that we know the password for level 3, we can work on finding the one for the next level, which is stored in a hidden file in the **inhere** directory. To list the hidden files in a folder use the **ls -la** command.

![img5](/assets/images/bandit_overthewire/img5.png)

-------

### Bandit Level 4

We know that the password for the next level is stored in the only human-readable file in the **inhere** directory. To find out the content type of a file, we can use the **file** command. Keep in mind the filenames in the **inhere** directory start with a dash **-** so you need to use relative/absolute path to the files, when executing the command.


![img6](/assets/images/bandit_overthewire/img6.png)

From the command output we see that the only file containing ASCII text is **-file07** and there's our password for level 5.


--------
### Bandit Level 5


For the next level, the password is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable

So, to return a list of all readable files in the current directory and its subdirectories that are not executable and have a size of 1033 bytes, we can use **find** command. The complete coommand will look as as follows: **find ./ -type f -readable ! -executable -size 1033c**

    ./: The search starts from the current directory.
    -type f: Only files (as opposed to directories, links, etc.) will be considered.
    -readable: The files must be readable by the current user.
    ! -executable: The files must not be executable (i.e., they cannot be run as programs).
    -size 1033c: The files must have a size of exactly 1033 bytes.



![img7](/assets/images/bandit_overthewire/img7.png)


-------

For level 7, the password is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

The find command will look as follows: **find / -user bandit7 -group bandit6 -size 33c 2>/dev/null**

    Command: find - used to search for files and directories.
    Starting point: / - the root directory, from where the search starts.
    User: bandit7 - specifies the owner of the files to be searched for.
    Group: bandit6 - specifies the group of the files to be searched for.
    Size: 33c - specifies the size of the files to be searched for.
    2>/dev/null redirects error messages to a black hole, i.e., discards them and prevents them from being displayed on the terminal.




![img8](/assets/images/bandit_overthewire/img8.png)


---------

### Bandit Level 6

For level 7, the password is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

The find command will look as follows: **find / -user bandit7 -group bandit6 -size 33c 2>/dev/null**

    Command: find - used to search for files and directories.
    Starting point: / - the root directory, from where the search starts.
    User: bandit7 - specifies the owner of the files to be searched for.
    Group: bandit6 - specifies the group of the files to be searched for.
    Size: 33c - specifies the size of the files to be searched for.
    2>/dev/null redirects error messages to a black hole, i.e., discards them and prevents them from being displayed on the terminal.




![img8](/assets/images/bandit_overthewire/img8.png)


---------

### Bandit Level 7


The password for the next level is stored in the file data.txt next to the word **millionth**


We can find it either using **cat** or **strings** + **grep**. Note this is not the only way to do it, there are multiple other methods.

![img9](/assets/images/bandit_overthewire/img9.png)




-------------

### Bandit Level 8


We have the file data.txt which contains the next level password. That would be the only line of text occuring only once

This is quite easy to solve, using the **sort** & **uniq** commands. **sort data.txt | uniq -u**

![img10](/assets/images/bandit_overthewire/img10.png)

To break it down, the command performs the following operations:

    Reads the contents of the file called "data.txt"
    Sorts the contents of "data.txt" alphabetically.
    The sorted output is then piped to the next command in the pipeline.
    The next command is "uniq", which filters adjacent matching lines from the sorted output and only outputs the lines that appear exactly once.
    The "-u" option in "uniq" stands for "unique" and ensures that only lines that appear exactly once are output.

So, in summary, the command sorts the lines in "data.txt" alphabetically and filters out duplicate lines, only outputting lines that appear exactly once.

------

### Bandit Level 9

Based on challenge description, the password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

We will be using the **strings** & **grep** commands: **strings data.txt | grep ===**


    "strings data.txt"  Prints all printable character sequences in the input
    "grep ==="  Searches for and prints lines in the output that contain the string "==="

So, the command essentially prints out any sequences of printable characters in "data.txt" and then filters and prints out only the lines that contain the string "===" from the output.

![img11](/assets/images/bandit_overthewire/img11.png)

--------

### Bandit Level 10  - work in progress...

![img12](/assets/images/bandit_overthewire/img12.png)



![img13](/assets/images/bandit_overthewire/img13.png)
![img14](/assets/images/bandit_overthewire/img14.png)
![img15](/assets/images/bandit_overthewire/img15.png)
![img16](/assets/images/bandit_overthewire/img16.png)
![img17](/assets/images/bandit_overthewire/img17.png)
![img18](/assets/images/bandit_overthewire/img18.png)
![img19](/assets/images/bandit_overthewire/img19.png)
![img20](/assets/images/bandit_overthewire/img20.png)

