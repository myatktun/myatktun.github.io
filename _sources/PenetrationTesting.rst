===================
Penetration Testing
===================

1. `Information Gathering`_
2. `Tools`_

`back to top <#penetration-testing>`_

Information Gathering
=====================

* `Passive Info Gathering`_, `Active Info Gathering`_
* information gathered about the target will be used to plan the attack
* look for anything that can expose information about the target, e.g. vulnerable ports,
documents that contain sensitive information

Passive Info Gathering
----------------------
    * OSINT, Open Source Intelligence, use publicly available information
    * collect as much information as possible without alerting the target
    * investigate DNS records for mail server details, subdomains
    * use search operators on search engines, dedicated search engines like Shodan, and OSINT
    analysis tools like Maltego
    * discover internet connected devices, and use tools such as [OSINT framework](https://osintframework.com)
    * **Google Search Operators**
        - ``site``: search from a particular site, e.g. ``site:github.com metasploitable``
        - ``inurl``: search for pages with particular word in the URL, e.g. ``inurl:osint``
        - ``filetype``: search for particular files, e.g. ``filetype:pdf penetration testing``
        - more info on [GHDB](https://www.exploit-db.com/google-hacking-database)

Active Info Gathering
---------------------
    * direct interaction with the target system, can trip alarms that will alert the target
    * some penetration tests intentionally trip alarms to test effectiveness of alerts, logs,
    or response times of the countermeasures

`back to top <#penetration-testing>`_

Tools
=====

* `Nmap`_, `Aircrack-ng`_, `JTR`_, `Hydra`_, `SET`_, `Burp Suite`_, `Shodan`_, `Maltego`_, `OpenVAS`_

Nmap
----
    * one of the most used network mapper tools, has a graphical version called Zenmap
    * host discovery: detect hosts within the network
    * OS detection: determine OS of the target device
    * app version detection: details about app version and name of the target device
    * port scanning: check what ports are exposed to the host
    * vulnerability scanning: Nmap scripting engine (NSE) allows to write custom scripts to
    detect vulnerabilities
    * ``-sS``: TCP SYN scan, gives stealth by not completing TCP connection
    * ``-sT``: TCP connect scan, complete connection to target
    * ``-sU``: UDP scan, can uncover ports related to DHCP, DNS, SNMP
    * ``-p``: define port or port range, default will scan all 65535 ports
    * ``-sC``: scan using default set of scripts
    * ``-sV``: version detection by referencing the port to Nmap services database
    * ``-O``: OS detection by sending packets and comparing to ``nmap-os-db``
    * ``--script``: define scripts using comma-separated list, e.g. ``--script "default,safe"``
    * [SANS Nmap Cheat Sheet](https://assets.contentstack.io/v3/assets/blt36c2e63521272fdc/blte37ba962036d487b/5eb08aae26a7212f2db1c1da/NmapCheatSheetv1.1.pdf) is a good resource
    * check ``/usr/share/nmap/scripts`` for NSE scripts, and can use ``nmap -script-help script_name``

Aircrack-ng
-----------
    * wireless security suite with packet analyser, WPA and WPA2 auditing tools, and others
    * WEP and WPA password decryption
    * packet injection
    * support for WPA and WPA2-PST password decryption
    * export captured data to files
    * replay attacks, de-authentication and more

JTR
---
    * John the Ripper, allows to perform brute force attacks against passwords, offline
    password cracker
    * support many encryption algorithms such as SHA-1, DES, Window's LM/NTLM
    * perform dictionary attacks and brute force capabilities with customisation
    * can run as a cron job

Hydra
-----
    * commonly used alongside JTR, online password cracker with a support of a wide range of
    network protocols
    * perform dictionary attacks and brute force capabilities
    * can add modules to extend functionality

SET
---
    * provide ways to conduct social engineering attacks, based on Python and is open source
    * WiFi AP, email, web and SMS based attacks, and create payloads
    * can integrate with third-party modules
    * support Powershell attack vectors, generate phishing attacks, and more

Burp Suite
----------
    * for web application penetration testing
    * interception proxy: inspect and modify requests and response the browser makes towards
    the targeted web app
    * spider: list all the directories on a web server
    * intruder: create and perform customised attacks
    * repeater: replay requests

Shodan
------
    * search engine for interconnected devices
    * indexes anything that is connected to the internet, such as webcams, database
    servers, medical devices, routers

Maltego
-------
    * link analysis software for OSINT, forensics and other investigations
    * visualise how information on the target is connected
    * transforms: allow to obtain richer results by plugging into various websites such as
    Shodan, VirusTotal and Threatminer

OpenVAS
-------
    * open source vulnerability scanner

`back to top <#penetration-testing>`_

