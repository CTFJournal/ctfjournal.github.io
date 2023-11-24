---
layout: post
title: "Threat Intelligence Tools"
---



***A [TryHackMe](https://tryhackme.com/room/threatinteltools) room focused on the concepts of Threat Intelligence and various OSINT tools used to conduct security threat assessments and investigations.***

  Practice: *Threat Intel, Urlscan.io, Cisco Thalos, Abuse.ch, URLhaus, SSLblacklist, MalwareBazaar, FeodoTracker, ThreatFox, PhishTool* 

---------------------



To put it briefly in simple terms, Threat Intelligence is information and analysis about potential cybersecurity threats and risks that could harm computer systems, networks, organizations, etc. It involves collecting, analyzing, and understanding data related to cyber threats, including the tactics, techniques, and procedures (TTPs) used by attackers. Threat intel helps in making informed decisions to enhance cybersecurity measures and protect against potential cyberattacks.

One by one we'll get to exercise and resolve some challenges including the following: 

- Use UrlScan.io to scan for malicious URLs.
- Use Abuse.ch to track malware and botnet indicators.
- Investigate phishing emails using PhishTool
- Use Cisco's Talos Intelligence platform for intel gathering.

-----
#### UrlScan
Urlscan.io is a free website analysis tool that automates the process of checking websites. It records web interactions, including connected websites, requested resources, and technology used. The website has two sections for recent and real-time scans, making it a handy tool for monitoring website activity.

We are provided with a urlscan.io snapshot of tryhackme.com website, which we can use to answer the challenge questions.

![img1](/assets/images/threat_intel_tools/img1.png)
Image source: https://tryhackme.com/room/threatinteltools


---------------
#### Abuse.ch

This is a research project developed to identify and track malware and botnets through several operational platforms, such as:


    Malware Bazaar:  Used for sharing malware samples.
    Feodo Tracker:  Used to track botnet command and control (C2) infrastructure.
    SSL Blacklist:  Used to collect and provide a blocklist for malicious SSL certificates and JA3/JA3s fingerprints.
    URL Haus:  Used for sharing malware distribution sites.
    Threat Fox:  Used for sharing indicators of compromise (IOCs).


##### The IOC 212.192.246.30:5555 is identified under which malware alias name on ThreatFox?

To find the info in the database, use search syntax keyword **ioc:** folllowed by the **ip_address:port**. As we can see in the screenshot the malware alias is "Katana"

![img2](/assets/images/threat_intel_tools/img2.png)

##### Which malware is associated with the JA3 Fingerprint 51c64c77e60f3980eea90869b68c58a8 on SSL Blacklist?
A quick search for fingerprints brings the result.


![img3](/assets/images/threat_intel_tools/img3.png)

##### From the statistics page on URLHaus, what malware-hosting network has the ASN number AS14061? 
Go under "Statistics" and use **Ctrl+F** to search text within the page content.

![img4](/assets/images/threat_intel_tools/img4.png)

##### Which country is the botnet IP address 178.134.47.166 associated with according to FeodoTracker?
Search by IP.

![img5](/assets/images/threat_intel_tools/img5.png)

----------

#### PhishTool 

This is an email analysis tool. We ar going to use the community edition for completing the tasks under this section.

Once we sign up for an account and log in, we encounter an upload file screen within the Analysis tab. At this interface, we provide our email for analysis in the specified file formats.

![img6](/assets/images/threat_intel_tools/img6.png)

Upon uploading, the email details are presented across various tabs:

    Headers: Routing info, source/destination addresses, IP, DNS, timestamp.
    Received Lines: Traversal details through SMTP servers.
    X-Headers: Additional info added by recipient mailbox.
    Security: SPF, DKIM, DMARC details.
    Attachments: List of file attachments.
    Message URLs: External URLs in the email.



In this simulation, we assume the role of a SOC Analyst assigned to analyze a suspicious email - *Email1.eml*. located on the Desktop of the AttackBox



##### What social media platform is the attacker trying to pose as in the email? Recipient and sender address?

For the first 3 questions we get the response if oppening the email in Thunderbird

![img7](/assets/images/threat_intel_tools/img7.png)

##### What is the Originating IP address? Defang the IP address.

To find the source IP address we can click on "More" and select "View Source" in Thunderbird upper right side corner. 

![img8](/assets/images/threat_intel_tools/img8.png)

Keep in mind, for answering correctly we need to Defang the IP - meaning to alter its format to make it non-functional while preserving its visual representation. This is commonly done for security purposes, especially when sharing information in a non-executable context. For example, an IP address like "192.168.1.1" might be defanged as "192[.]168[.]1[.]1" to prevent accidental clicks or interpretations by systems that could potentially interact with the IP. 



![img9](/assets/images/threat_intel_tools/img9.png)
![img10](/assets/images/threat_intel_tools/img10.png)
![img11](/assets/images/threat_intel_tools/img11.png)







During high school i would rate my performance level for mathematics and native language to have been somewhere somewhere in the average range, if we are to put it into a more generic statement.


I have graduated university in 2011 and i cannot say i have used Linux a lot at that time. I have started interacting with it more after i have graduated, especially in the recent past few years(5-6). Initially the impression was of a quite difficult OS, however the more i learned and used it, the more i started to like it. I am still considering Linux a quite challenging Operating System, yet that is only a motivation to keep learning it's complexities. With the great support from Linux/AskUbuntu community, mastering the OS becomes an easier goal to achieve.  For almost a year now i am using Ubuntu as my main OS, keeping windows as the 2nd boot option(usually needed for attending certification exams). Besides the desktop experience, the CLI opens endless possibilities and i enjoy practicing it. I am passionate about cybersecurity and all my activities are done in Linux, either Kali or Ubuntu, as a result i get to use the Terminal for a significat amount of time.

I am aware of Landscape and Kubernetes however i haven't worked with any of them.

https://www.linkedin.com/in/sergiu-onuta/