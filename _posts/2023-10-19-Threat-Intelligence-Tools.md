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


##### The IOC 212.192.246.30:5555 is identified under which malware alias name on ThreatFox?

To find the info in the database, use search syntax keyword **ioc:** folllowed by the **ip_address:port**. As we can see in teh screenshot the malware alias is "Katana"

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




![img6](/assets/images/threat_intel_tools/img6.png)
![img7](/assets/images/threat_intel_tools/img7.png)
![img8](/assets/images/threat_intel_tools/img8.png)
![img9](/assets/images/threat_intel_tools/img9.png)
![img10](/assets/images/threat_intel_tools/img10.png)
![img11](/assets/images/threat_intel_tools/img11.png)
