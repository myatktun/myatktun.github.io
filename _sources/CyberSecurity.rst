==============
Cyber Security
==============

1. `Basics`_
2. `Networking`_
3. `Attacks`_
4. `Malware`_
5. `Vulnerabilities`_
6. `Security Systems`_
7. `Cryptography`_
8. `Linux`_
9. `Cisco Packet Tracer`_
10. `Additional Resources`_

`back to top <#cyber-security>`_

Basics
======

* `Levels of Protection`_, `McCumber Cube`_, `AAA`_
* `Hackers`_, `Threats`_, `Digital Forensics`_, `Ethics`_, `Certifications`_
* protecting confidentiality, integrity and availability of computing devices, network,
hardware, software, data and information
* data and information must be secure during transit, processed, and even at rest
* cyber security can be considered a digital form of information security
* need to know attackers and their motivation, and vulnerabilities and security of the system
* attackers can be from both outside and inside of system, and insiders can do greater damage
* penetration testers identify and exploit vulnerabilities, and recommend security measures

Levels of Protection
--------------------
    * each must be protected from digital attacks by protecting networked systems and data from
    unauthorized use or harm
    * **Personal**
        - safeguard own identity, data and computing devices
        - personal data: any information that can be used to identify a person, offline or
        online
        - e.g. choose a username appropriate for the type of account, do not reveal any
        personal information unless necessary
        - cybercriminals can use sensitive information to identify and impersonate
        - example personal data: medical and education records, employment and financial
        records, social security number, driver license number
        - information might be free online, but privacy is the price to pay
        - in identity theft, medical insurance and bank accounts are stolen by the thief to
        use for himself
        - ISP may track activity to sell data to advertisers, or legally require to share it
        with government surveillance agencies
        - social media companies and search engines sell data to advertisers for profit
        - websites use cookies to track activities and sell to advertisers
        - advertisers monitor online activities to send targeted ads
    * **Organizational**
        - everyone is responsible to protect organization's reputation, data and customers
        - transactional data: buy, sell, product activity and basic operations information
        - intellectual property: patents, trademarks, product plans, trade secrets that give
        economic advantage over competitors
        - financial data: income, balance, cash flow, insight company data
        - big data: collected and shared data from IoT devices, including from cloud and
        virtualization
    * **Government**
        - digital information gathered and shared must be protected, more vital as national
        security, economic stability and safety of citizens are at stake

McCumber Cube
-------------
    * model framework to help organizations establish and evaluate information security
    * **Foundational Principles**
        - to protect information systems
        - confidentiality, integrity, and availability
    * **Protect Information in each state**
        - processing: data being used to perform operations
        - storage: data stored in memory or on permanent storage
        - transmission: data travelling between systems
    * **Security Measures to protect data**
        - give awareness, train and educate to users about potential security threats
        - use technology, software/hardware based such as firewalls, to protect systems
        - have policy and procedure to implement information assurance such as incident
        response plans and practice guidelines
    * **Confidentiality**
        - concerns with viewing and privacy in data and information
        - set of rules to prevent sensitive information from unauthorized ones
        - need to restrict certain people, devices or processes from seeing data such as
        username and password combination
        - accomplished through encryption, identity proofing, two factor authentication
    * **Integrity**
        - data need to be safe during transmission, processing and storage
        - need to make sure data is not changed from its original form, even by one bit
        - accomplished through hashing or checksum
    * **Availability**
        - authorized users, devices or processes are able to access systems and data
        - need to be secure from attacks such as DDoS (Distributed Denial-of-Service)
        - need to also have tolerance and load balancing
        - accomplished through maintaining equipment, hardware repair, keep up to date and
        create backup

AAA
---
    * **Authentication**
        - process of proving to be valid
        - claiming as someone is identification, proving it is authentication
        - prove using something you know, such as password, something you have, such as a key,
        and something you are, such as biometrics
        - using multiple forms is called multi-factor authentication
        - using two passwords is not multi-factor, as they are both of same form
        - false positives and false negatives can restrict or allow access in error
        - 2FA or two-factor authentication can make a system more secure
        - but in 2FA, avoid using SMS messages as they can be intercepted and redirected
        - should prefer options like authentication apps or USB keys
    * **Authorization**
        - only allowing a user to do certain things based on the credentials
        - principle of least privilege: users should only be granted enough permissions to do
        required functions, not more
        - never use root account on a system, but use one with limited privileges
        - allowing a user more privileges than required will lead to attacks or the attacker
        can get privileges if he gets the account
    * **Accounting**
        - keeping track/logging of users and their actions
        - by accounting, actions of an attack can be traced, or even predict future attacks
        based on previous actions
        - multiple admins should not share a default account
        - accounting cannot tie actions to an individual

Hackers
-------
    * thinks outside the box and finds unconventional solutions to problems
    * **Script Kiddie**
        - inexperienced ones who use existing scripts and programs to do attacks
        - have no idea of the consequences
        - may use basic tools, but attacks can still have devastating consequences
    * **White Hat**
        - with permission of a company to find and exploit vulnerabilities before a black hat
        hacker does
        - report back to the owner
    * **Gray Hat**
        - may sometimes violate laws, but usually do not have malicious intent like black hat
        hacker
        - only report to the owner if the attack result match with their agenda
        - may publish the vulnerability on the internet for other attackers to exploit
    * **Black Hat**
        - malicious hacker and does damages
        - take advantage of any vulnerability for illegal personal, financial or political
        gain
    * **Organized Hackers**
        - organizations of cyber criminals, terrorists and state-sponsored hackers
        - highly organized and may provide cyber crime as a service
        - hacktivists make political statements to create awareness about issues important to
        them
        - state-sponsored attackers are well-funded and gather intelligence on behalf of their
        government
    * **Security Researcher**
        - find vulnerabilities anywhere
    * **Penetration Tester**
        - find vulnerabilities within a certain company
    * **Security Architect**
        - build systems to establish cyber security presence
    * M.I.C.E: money, ideology, compromise/coercion, ego/extortion
    * M.E.E.C.E.S: money, ego, entertainment, cause, entrance, status
* data breaches can be expensive to fix and threaten customers and employees, but also provide
lessons

Threats
-------
    * cyber attacks can be from within organization or outside
    * **Internal Threats**
        - caused by staffs or trusted partners accidentally or intentionally
        - may mishandle confidential data, invite attacks by using infected USB or clicking
        malicious emails
        - some will threaten operations of internal servers or network infrastructure
    * **External Threats**
        - caused by attackers outside organization
        - may exploit vulnerabilities and gain unauthorized access
        - may use social engineering to gain organizational data

Digital Forensics
-----------------
    * **Forensics**
        - uses science and math to analyse physical evidence
        - inculpate: prove someone did something, exculpate: prove someone did not do something
        - usually investigator will testify as expert witness
    * subcategory of forensics science dealing with digital devices
    * examine systems and networks that have been attacked, or take actions to remediate the
    ongoing incident
    * subcategories of digital forensics: computer forensics, network forensics, mobile device
    forensics
    * **Cybercrime**
        - illegal act involving computing device, its systems or applications
        - fraud, spam, unauthorized access, intellectual property theft, piracy
        - rigging systems, espionage and exfiltration of data, identity theft
        - writing and spreading malware, DoS, creation and distribution of child pornography
    * computing devices can be a tool to commit a crime, target of a crime when hacked, or as
    repository for evidence
    * **Digital Evidence**
        - foundation to identify, capture, and prosecute cyber criminals
        - information stored or transmitted in binary form, comprised of data and metadata
        - gather information about individuals, determine transpired events, identify affected
        systems and networks, construct timeline, and discover tools and exploits used
        - can sometimes reveal motivation of attackers
        - host-based evidence: volatile data from RAM, non-volatile data from HDD, optical
        storage, removable devices
        - network evidence: live traffic, stored communication, and server logs
        - keyword searches can be effective when investigating large data set
        - check common locations such as IDS, IPS, firewall logs, app and server logs, HTTP
        and FTP captures, and email
        - cloud-based evidence can be difficult to acquire, as companies do not give easily
    * **Gather and Image**
        - minimize loss, gather relevant evidence, maintain integrity
        - always create copies and only work on them
        - avoid destroying volatile data, missing critical data, altering data, using
        untrusted commands, and system adjustments
        - evidence must be preserved in its original state
        - hash functions are used to verity files or drives have not been altered, and copies
        are intact during investigation
        - bitstream copy: imaging a hard drive by making bit-for-bit copy of all sectors,
        performed on hard drive level, not file system level
        - slack space: location of the end of a file on hard drive to the end of file cluster
        that the file is stored in
        - in slack space, deleted files, or at least fragments, and hidden data can be found
        - evidence can also be found in unallocated space
    * **Analyse**
        - can confirm or dispel the existence of an incident
        - find kind of incident, directly or indirectly affected systems
        - knowing damage, critical or sensitive systems or data, potential business impact
    * **Report**
        - step by step procedure of imaging, details of each test, tools used, and facts
        uncovered
        - missing item from the report, cannot be used in court
        - error of grammar or spelling can put doubt on the report
        - must document manufacturer, model, serial number of HDD and system components,
        peripherals of system, description of evidence, case number, tag number of evidence,
        hash algorithms and message digests
        - date and time of collection must be proven and clear
        - include full name and signature of people possessing the evidence, location and,
        receipts and transfers of evidence
        - chain of custody: preservation process, from acquisition of evidence until used in
        court
        - any gaps in chain of custody can cause evidence to be inadmissible
    * **Anti-Forensics**
        - to thwart discovery of information related to illegal activities of user
        - making examination difficult, time consuming, or impossible
        - hide data by encryption, compression, through codes to give alternate meaning
        - steganography: hiding files and data inside other files

Ethics
------
    - practice within legal bounds, as most hacks leave tracks
    - check with legal department of organization
    - just because something is not illegal, it is not ethical

Certifications
--------------
    * great way to verify skills and knowledge, helps boost career
    * **CCST**
        - Cisco Certified Support Technician, entry-level certification
        - does not expire or require periodic recertification
        - aimed at high school and early college students, and those looking for career change
    * **CompTIA Security+**
        - entry-level, meets the U.S. Department of Defense Directive requirements
        - important for looking to work in IT security for federal government
    * **CEH**
        - EC Council Certified Ethical Hacker
        - test to look for weaknesses and vulnerabilities using tools
    * **CISSP**
        - ISC2 Certified Information Systems Security Professional, most recognizable and
        popular
        - need to have at least five years of relevant industry experience to take the exam
    * **Cisco Certified CyberOps Associate**
        - validates the associate-level analysts within security operations centers
    * check [CyberSeek](https://www.cyberseek.org/pathway.html) for career pathways in cyber security market

`back to top <#cyber-security>`_

Networking
==========

* `Addresses`_, `Switch`_, `Communication`_, `Ports`_, `TCP and UDP`_
* `Autonomous System`_, `Static Addressing`_, `Dynamic Addressing`_
* switches connect devices on the same network, routers connect devices on different network
* PC is never connected to a router, only a switch or another router
* devices used at home that are called routers, actually has switch functionality
    * PC connected with cable to that device is actually connected to its switch interface

Addresses
---------
    * **MAC**
        - physical address of network interface card, NIC
        - globally unique, flat and permanent
        - 48 bits long, represented using 12 hexadecimal characters, first 6 characters for
        manufacturer ID, OUI, and last 6 characters for device ID
    * **IPv4**
        - logical addresses, bound to NICs through software
        - 32 bits long, represented with 4 decimal numbers, first section for network ID, and
        second section for host ID
        - 32 bit subnet mask divide the addresses into two sections, 1s represent network
        bits, and 0s represent host bits
    * **Subnet Mask**
        - network bits are shared by all devices on the same network
        - host bits are unique to each host on the network
        - binary AND operation is performed on IP address and subnet mask to determine network
        and host section
        - can span multiple octets, and do not need to be a multiple of eight bits
        - network ID, all host bits set to 0, and network broadcast address, all host bits set
        to 1, cannot be assigned to a host
        - another binary AND operation is performed on the destination IP address and source's
        subnet mask to determine if same network or not
        - destination's subnet mask is unknown and never transmitted, but it's IP address is
        known by the source or resolved using DNS
    * IP addresses can be bound to and associated with MAC addresses for a duration, to keep
    track of which device is using which IP address of the network
    * **DNS**
        - Domain Name System, provide user-friendly way to identify network locations
        - website's IP address may change, but new IP will still be associated with same name
        - FQDN (Fully Qualified Domain Name) are stored in hosts file before DNS
        - DNS uses hierarchical distributed way to resolve names to IPs
        - if client's local DNS server cannot resolve, it will query to one of the 13 root DNS
        servers
        - the root server will give the IP address to the DNS server for top level domain
        - every top level domain, such as .com and .net, is maintained by a registry
        - each top level DNS servers are responsible for knowing the DNS servers for each
        subdomain, e.g. in google.com, .com server is responsible for google subdomain
        - DNS server in home router is not as robust as enterprise level, it sends DNS queries
        to ISP DNS server

Switch
------
    * can connect to other switches and routers
    * checks the destination MAC address to find the interface it is connected to
    * if the switch does not know the interface, it floods the frame out of all other
    interfaces, except the origin
    * the switch can flood unicast, multicast and broadcast
    * as frames are sent into the switch, it builds SAT (Source Address Table)
    * each switch maintains its own SAT
    * switches are transparent and don't change the frame

Communication
-------------
    * **Local**
        - direct communication is possible on same network
        - the source sends broadcast message using ARP (Address Resolution Protocol), which resolve
        a known IP address to MAC address
        - all devices on the network receive the ARP request, and only the destination replies
        - ARP reply is unicast, only sent to the source, since source's MAC address is within the
        ARP request
        - the source now have the destination's MAC address and send the message
        - source and destination MAC addresses are found in layer 2 frame
        - IP addresses are the fields found in IP packet
    * **Remote**
        - direct communication is not possible on different networks
        - the source need to get the traffic to its default gateway, an interface on a router
        connected to its network
        - statically or dynamically configured device also gets an IP address of a router
        interface, which is responsible to take traffic between networks
        - the source send broadcast ARP request, searching for default gateway MAC address
        - the gateway unicat ARP reply back to the source
        - the source send the traffic with gateway MAC address and the actual destination IP
        address
        - routers never forward broadcasts to another network
    * **Packet Routing**
        - routers maintain routing tables, containing destination networks and directions
        to send the traffic
        - routers have a default route to send, if routing tables do not have enough data
        - if no routing table and default route, router will drop the packet and send error
        message back to the source through ICMP (Internet Control Message Protocol)
        - same ARP process takes place, if routers are connected using Ethernet infrastructure
        - different frame type is used for different infrastructure, e.g. wireless NICs use
        802.11 frames instead of Ethernet frames
        - same IP packet can have different encapsulating frame
        - point-to-point directly connected serial interfaces, using PPP or HDLC, do not need
        to deal with MAC addresses and their frames, since the connection is like a tunnel
        - the frame is removed every time a packet passes through an interface, a new frame
        is used if packet still needs another hop
        - IP addresses never change during the packet's path, only MAC addresses of a frame
        change

Ports
-----
    * a web server is just a program or service running in the background
    * the way into and out of a service is through a port
    * a port is logical endpoint of communication that identifies a service, represented by
    a port number
    * MAC addresses are found in Layer 2 frame, IP addresses in Layer 3 packets, and port
    numbers in Layer 4 TCP segments or UDP datagrams
    * based on the port, the machine knows which service to send the data
    * **Well Known Ports**
        - from 0 to 1023 are used by major protocols and services
        - e.g. port 80 by HTTP
    * **Registered Ports**
        - from 1024 to 49151 are assigned by IANA for specific companies that want common port
        to be used for their programs
        - but can be used by any system if not in use
        - locally significant to a system
    * **Dynamic Ports**
        - from 49151 to 65535 are used by client applications as needed
        - e.g. browser might use port 60000 to send a request to a HTTP web server

TCP and UDP
-----------
    * one of the protocols used at Layer 4 to encapsulate data from Layers 5, 6 and 7
    * TCP segments or UDP datagrams will be used
    * **TCP**
        - Transmission Control Protocol
        - connection between source and destination for reliable data transfer and flow
        control
        - send data at acceptable rate, bytes sent are ordered and sequenced
        - guarantee integrity and processed in order
        - segments are acknowleged for sender to know the destination has received
        - will resend unacknowleged bytes
        - used where accuracy is important, such as file transfer, email, websites
        - has much more overhead than UDP, but gives integrity and accuracy
    * **UDP**
        - User Datagram Protocol
        - connection less and no flow control
        - used for real time communication, DNS, DHCP
        - streaming using UDP still need to be processed in correct order
        - RTSP, Real Time Streaming Protocol, at Layer 7, does the ordering for UDP
        - real time communication can slow down when using TCP, as it requires to maintain
        connection, implement flow control and send acknowlegements
        - less overhead than TCP, but has efficiency

Autonomous System
-----------------
    * collection of networks under single administrative control, e.g. ISP, large organization
    * instead of one big flat network, having multiple networks interconnected by routers can
    reduce the size of broadcast domain
    * can control bandwidth and processing of devices on network from broadcast
    * security at router level, in the form of access control list, can be used to filter
    traffic by IP addresses, protocols and ports
    * can design a network hierarchically
    * isolating traffic can make troubleshooting easier
    * **Dynamic Routing**
        - can register ASN (Autonomous System Number) to become independent of ISPs
        - maintain routing tables and exchange routing information with multiple ISPs
        - when traffic leaves the autonomous system, routers choose ISP for most efficient
        packet delivery
        - IGP (Interior Gateway Protocol) allows routers within autonomous system to
        communicate each other
        - with IGP, routers share information about directly or indirectly connected networks
        - metrics are used by routers to determine fastest path between networks
        - OSPF (Open Shortest Path First) and Cisco's EIGRP (Enhanced Interior Gateway Routing
        Protocol) are two popular IGP
        - OSPF and EIGRP use bandwidth as metric, but calculate it differently
        - EGP (Exterior Gateway Protocol) allows routers from different autonomous system to
        communicate and exchange routing information
        - BGP (Border Gateway Protocol) is the only EGP used across the entire internet

Static Addressing
-----------------
    * with 32 bit IPv4 addresses and subnet masks
    * **RARP**
        - Reverse Address Resolution Protocol, reverse of ARP
        - for diskless work stations, which have NIC and MAC address, and get IP address from
        RARP server
        - admins preconfigure a table on the server to match MAC and IP addresses, and
        dynamically give them upon rquest
        - matches MAC address to IP address
        - Layer 2 protocol inside frames, no IP header and cannot send off a network
        - each and every network would need its own RARP server
    * **BOOTP**
        - Bootstrap Protocol, messages are encapsulated in Layer 4 UDP datgrams, which are in
        Layer 3 IP packets
        - messages can be sent off and routed on a network, remove overheads from RARP
        - relay agents: router interfaces on a network, also serve as full gateways, turn
        broadcast BOOTP messages into unicast messages
        - can have two BOOTP servers on a network to give IP addresses for other networks
    * in both RARP and BOOTP, admins need to collect MAC addresses and manually associate with
    IP addresses
    * static addressing is still used for servers and router interfaces that can't rely on
    other server to have dynamic IP addresses
    * static addressing is not scalable for client machines in a network

Dynamic Addressing
------------------
    * DHCP
        - Dynamic Host Configuration Protocol, made to be an extension of BOOTP
        - scope: range of IP addresses for dynamically assigning
        - lease: DHCP server assigning IP to a client machine for a duration, if there are
        addresses left in the dynamic pool
        - the client must renew a lease before time not to lose network connectivity
        - admins do not need to collect MAC addresses
        - there can be excluded and reserved IP addresses in a scope
        - ``Option: 53`` field in Layer 7 distinguishes DHCP from BOOTP
    * **DORA**
        - Discover, Offer, Request and ACK, client getting IP address from DHCP server
        - client broadcast Discover message at both Layer 2, source IP 0.0.0.0 and destination
        IP FF-FF-FF-FF-FF-FF, and Layer 3, source's MAC and destination MAC 255.255.255.255
        - relay agent need to be preconfigured about DHCP server
        - client sends Discover as broadcast, and relay agents turn them into unicast
        - each subnet has its own scope on the DHCP server
        - DHCP server knows which scope, based on IP address of relay agent
        - DHCP server also gives the client information such as subnet mask, default gateway
        IP address, DHCP server address and DNS server address
        - client will be able to send unicast messages after getting ACK from DHCP server,
        using the assigned IP address
    * DHCP server in home router is not as robust as enterprise level

`back to top <#cyber-security>`_

Malware
=======

* `Virus`_, `Worm`_, `Logic Bomb`_, `Trojan Horse`_, `RAT`_, `Rootkit`_, `Backdoor`_, `Spyware`_
* `Ransomware`_, `Scareware`_, `Symptoms of Malware`_
* malicious software, intended to damage or break computer system

Virus
-----
    * inject itself into other programs' instructions
    * needs a host file to be run, can be a data file or boot sector
    * can spread and replicate by itself locally
    * but need human intervention to spread across network, e.g. forwarding an infected email
    attachment
    * most are spread by USB, network shares or emails
    * always has a malicious payload meant to execute, can be harmless or destructive
    * most require interaction to activate and can be written to act on specific time
    * can mutate to avoid detection

Worm
----
    * does not infect host files, stands alone in its own file
    * spread quickly by itself across networks without human intervention, containing malicious
    payload
    * exploit vulnerabilities in protocols, networks, and configurations
    * can get into email lists, compose an email, attach themselves, and make the subject and
    body of email like it came from trusted source
    * responsible for some of the most devastating attacks

Logic Bomb
----------
    * introduces latency when executed, triggered by date, time or event
    * better if undetected for long time, as it allows malware to spread and remain silent for
    antivirus companies to notice
    * when trigger time is reached, malware on all infected systems will run

Trojan Horse
------------
    * malware with hidden ability used by attacker
    * has social engineering involved, as user must download and install it
    * do not replicate on local machine or across network
    * often found in images, audio or games
    * act as decoy to sneak malicious software

RAT
---
    * Remote Administration Tool, software used to remotely access or control a computer
    * can be used legitimately by system admins to access client computers or servers
    * when maliciously, known as Remote Access Trojan
    * can be used by attacker without the knowledge of user, usually to gain admin access
    * often in pirated software through patches, in cracked games, or email attachments
    * most can log keystroke, capture packet, screen and camera, access file, execute code,
    manage registry, sniff password
    * may perform unauthorized operations and hide their presence

Rootkit
-------
    * set of programs and code that allow persistent undetectable presence on computer
    * can sanitize logs and repair timestamps, hide actions of hackers
    * can modify OS to create backdoor
    * most take advantage of software vulnerabilities to gain privileged access
    * can mask files, processes, network connection
    * conceal installed malware, telling antivirus software that the malicious file is checked
    and skip it
    * computer infected has to be wiped and any required software installed

Backdoor
--------
    * can be installed by rootkit
    * allow hackers to bypass normal authentication through the exploited software
    * gain unauthorized remote access to resources
    * works in the background and difficult to detect

Spyware
-------
    * covertly monitor user activity and report personal user data, usually for financial gain
    * sell personal data, redirect web activity to ad sites, present targeted ads and pop-ups
    through adware
    * modify the security settings on devices
    * often bundle with legitimate software or Trojan horses
    * **Adware**
        - automatically play or display advertisements, mostly often on a browser, or
        download promotional materials
        - often bundled with product or package
        - common in shareware, a free software that might require payment after trial
    * **PUP**
        - Potentially Unwanted Program, term named by McAfee
        - included by companies as extra to a user downloaded program
        - fall under spyware and adware categories, used to be called Trojan horses
        - but as companies mention the extras in EULA (End User License Agreement), name was
        changed to PUP

Ransomware
----------
    * computing devices are locked and encrypted, usually through a clicked link or installed
    malware, often spread through phishing emails
    * users are threatened to pay for a key by a certain time, if not, files will start to be
    deleted
    * paying for decryption key encourages attackers to continue doing
    * a key is not guaranteed to be sent, or not sure to work even if it is sent

Scareware
---------
    * use scare tactics to trick user to take specific action
    * mainly consist of OS style window pop ups that warn system is at risk and need to run
    specific program
    * system is infected with malware when the program is run

Symptoms of Malware
-------------------
    * CPU usage increase, computer freeze or crash often
    * browser speed decrease, unexplainable network connection problems
    * files modified or deleted, can have unknown files, programs or icons
    * unknown processes running, programs turning off or reconfigured
    * emails sent without consent

`back to top <#cyber-security>`_

Attacks
=======

* `Social Engineering`_, `Privilege Escalation`_, `Integrity Attack`_, `DoS`_, `DDoS`_
* `Botnet`_, `On-Path Attacks`_, `Phishing`_, `SEO Poisoning`_, `Password Attacks`_
* `APT`_, `Cyber Warfare`_, `Cryptojacking`_, `KRACK`_
* zero-day attack: attackers exploiting software before creators get a chance to fix

Social Engineering
------------------
    * as computer vulnerabilities get harder to exploit, people become the most obvious target
    * prey on gullible and naive people
    * always the weakest link in cyber security system
    * attackers find any potential weaknesses and vulnerabilities of the victims that can be
    exploited
    * public information online is a great resource for the attacker
    * dumpster diving: technique used to gather information
    * attacker may develop trust and credibility from the victims before attacking
    * mitigate by teaching users and testing to make sure they follow policies
    * pretexting: call an individual and lie to gain access to privileged data
    * tailgating: follow authorized person into secure location
    * quid pro quo: request personal information in exchange for something

Privilege Escalation
--------------------
    * exploit in which attacker elevates their privileges, usually to root level
    * gain control of vulnerable system, identify vulnerable privileged service and exploit it
    * e.g. SUID binary with a security problem, such as sudo in CVE-2019-14287
    * unnecessary running as root in software or containers should be avoided

Integrity Attack
----------------
    * attempt to corrupt data, attacks are not necessarily after specific company, but to
    eliminate any form of trust
    * will throw doubt and confusion into accuracy and reliability of information
    * can affect decisions made in both public and private sectors
    * publicly traded companies have much more to lose
    * attacker may have legitimate, self-created account on the network, undetected and
    monitoring
    * integrity attacks may take years or indefinitely to be detected

DoS
---
    * Denial-of-Service, relatively simple network attack to carry out, even by unskilled ones
    * result in interruption of network service to users
    * considered a major risk as it can easily interrupt communication and cause significant
    loss
    * **Overwhelming Traffic**
        - enormous amount of data is sent to a network which it cannot handle
        - cause slowdown in transmission or response
        - device or service may even crash
    * **Formatted Packets**
        - packet: collection of data that flows between source and receiver on a network
        - receiver is unable to handle the maliciously formatted packet
        - packets with errors or improperly formatted ones cannot be identified by application
        - cause receiver to slow down or crash

DDoS
----
    * Distrusted DoS, similar to DoS but originate from multiple, coordinated sources
    * attacker build a network of infected hosts called zombies, which are controlled by
    handler systems
    * zombie computers constantly scan and infect more hosts, creating more zombies
    * when ready, attacker instruct handler systems to make zombies carry out DDoS attack

Botnet
------
    * a group of bots, connected through internet, controlled by individual or group
    * bot computer is infected by visiting unsafe sites, email attachment or media file
    * can have thousands of bots usually controlled through command and control server
    * can be activated to distribute malware, launch DDoS, spam email or execute brute-force
    password attacks
    * cyber criminals often rent botnets to third parties
    * many organizations force network activities through botnet traffic filters to identify
    botnet locations, e.g. cloud-based Cisco Security Intelligence Operations (SIO) service

On-Path Attacks
---------------
    * attackers intercept or modify communications between devices to collect information or
    impersonate
    * **MITM**
        - Man In The Middle, attacker take control of a device without user's knowledge
        - attacker can intercept and capture user information before sending to the intended
        destination
        - often used to steal financial information
        - there are many types of malware that possess MITM attack capabilities
    * **MITMO**
        - Man In The Mobile, variation of MITM
        - used to take control over user's mobile device
        - mobile device is instructed to exfiltrate user-sensitive information and send to
        attacker
        = e.g. Zeus trojan that quietly capture two-step verification SMS

Phishing
--------
    * email version of social engineering
    * sending out bait to large number of people, hoping some will bite to give information
    * when a link in phishing email clicked, user is brought to attacker's website, that looks
    like a legitimate one
    * visiting the sites can also install a malware on the machine
    * zip files as email attachment: multiple files compressed and can bypass malware scanners
    * MS word document or spreadsheet as email attachment: include a macro, and users are made
    to believe the file is secure, and can only view when macro is enabled
    * **Spear Phishing**
        - target specific users of a company, instead of random emails
    * **Whaling**
        - high profile spear phishing
    * **Pharming**
        - hijacking legitimate website's IP and/or domain
        - redirect traffic to fake website to collect information
    * **Watering Hole**
        - attacking strategy in which victim is in particular group
        - hacker observes which websites the group often uses, infect those websites with
        malware, and some member get infected eventually
        - relying on websites that the group trust makes the strategy efficient
    * 90% of phishing emails are specifically designed to deliver ransomware
    * **Mitigate Phishing**
        - check links before clicking
        - on mobile, do not click, but open a new tab and go to the site manually
        - can have generic greeting instead of user's actual name
        - email address can be spoofed or off
        - URLs with same domain name but wrong location, may use HTTP instead of HTTPS
        - will be asked to fill more information than required
        - emails will always force users to act urgently or threaten
        - always check formatting and design of email and website images
        - poor spelling and grammar are commonly found
        - email often has generic signature without contact information
        - email may have attachments or asking to allow to run scripts

SEO Poisoning
-------------
    * SEO: Search Engine Optimization, improve organization's website to gain visibility in
    search engine results
    * search engines rank web pages according to relevancy of content to user's search query
    * attackers take advantage of popular search terms and use SEO to push malicious sites
    higher up the ranks
    * main goal is to increase traffic to malicious sites that host malware or attempt social
    engineering

Password Attacks
----------------
    * passwords are stored as hashed values, not as plain texts
    * **Password Spraying**
        - spray a few commonly used passwords across a large number of accounts
        - allow attacker to remain undetected as they avoid frequent account lockouts
    * **Dictionary Attacks**
        - attacker systematically try every word in a dictionary or a list of commonly used
        words
    * **Brute-force Attacks**
        - simplest and most commonly used way
        - attacker use all possible combination of letters, numbers and symbols until correct
        - has to calculate each hash on the fly, complex passwords can take much longer
    * **Rainbow Attacks**
        - rainbow table: large dictionary of precomputed hashes and the passwords from  which
        they were calculated
        - compare the hash of a password with those stored in rainbow table
        - do not need to calculate each hash on the fly
    * **Traffic Interception**
        - plain text or unencrypted passwords can be easily read by intercepting communication
        - password stored in readable text can be read by anyone who has access to the account
        or device
    * e.g. Ophcrack, L0phtCrack, THC Hydra, RainbowCrack, Medusa

APT
---
    * Advanced Persistent Threats
    * multi-phase, long term, stealthy and advanced operation against a specific target
    * complex and require levels of skills to carry out
    * individual attacker often lacks the skill set, resources or persistence to perform APT
    * usually well-funded and target organizations or nations for business or political reasons
    * main goal is to deploy malware on target's systems and remain undetected

Cyber Warfare
-------------
    * use technology to penetrate and attack another nation's computer system and networks
    * e.g Stuxnet malware to hijack targeted computers and cause physical damage to equipment
    controlled by computers
    * to steal defense secrets and technology from industries and military
    * compromised data can be used for extortion against foreign government personnel
    * some attack to disrupt nation's infrastructure
    * attackers can destabilize a nation without setting foot in it

Cryptojacking
-------------
    * **Cryptocurrency**
        - use strong encryption techniques to secure online transactions
        - owners keep their money in encrypted, virtual wallets
        - when transaction takes place, details are recorded in a blockchain systen, a
        decentralized, electronic ledger
        - no third party, such as banks, interference
        - special computers collect data about latest transactions, turn them into
        mathematical problems to maintain confidentiality
        - mining: verifying transactions through technical and highly complex process
        - miners work on high-end PCs to solve mathematical problems and authenticate
        transactions
        - after verifying, the ledger is updated and copied to anyone belonging to the
        blockchain network
    * cryptojacking is a technique to hide on user computing devices and use the resources to
    mine cryptocurrency
    * many victims do not know they had been hacked until too late

KRACK
-----
    * Key Reinstallation Attack
    * attacker break the encryption between wireless router and the device, gain access to
    network data

`back to top <#cyber-security>`_

Vulnerabilities
===============

* `Hardware Vulnerabilities`_, `Software Vulnerabilities`_, `Software Updates`_
* any way a hacker can breach a system, a weakness or a flaw, is vulnerability
* exploit: program written to take advantage of a known vulnerability
* think like an attacker, how things can be made to fail, to notice most security problems
* bug bounty programs reward hackers for finding and exploiting security vulnerabilities
* even IoT devices, with no valuable information, can be hacked and instructed to attack
other devices and systems
* default username and password, outdated or flawed software can be an entry point
* always examine personal cyber security habits and practices

Hardware Vulnerabilities
------------------------
    * mostly occurred because of hardware design flaws
    * specific to device models and not exploited through random attempts
    * more common in highly targeted attacks
    * traditional malware protection and good physical security are sufficient protection for
    everyday user
    * **Rowhammer Exploit**
        - in RAM, changes in a capacitor can influence to those very close to it
        - Rowhammer uses the design flaw to trigger electrical interference to corrupt data
        stored inside RAM, by repeatedly accessing a row of memory
    * **Meltdown & Spectre**
        - two hardware vulnerabilities that affect almost all CPUs
        - Meltdown: read all memory from given system
        - Spectre: read data handled by applications
        - side-channel attack: attack enabled by leakage of information from computer system
        - can compromise large amount of memory data as the attacks can be run multiple times
        with minimum crash or errors

Software Vulnerabilities
------------------------
    * usually introduced by errors in OS or application code
    * **SYNful Knock Vulnerability**
        - discovered in Cisco IOS (Internetwork Operation System) in 2015
        - attacker can gain control of enterprise-grade routers, and monitor all network
        communication and infect other network devices
        - to avoid, always verify the downloaded IOS image and limit physical access to
        authorized personnel only
    * **Buffer Overflow**
        - buffer: memory area allocated to an application
        - by writing data beyond the limits of a buffer, application can access memory
        allocated to other processes
        - can lead to system crash, data compromise or provide escalation of privileges
    * **Non-validated Input**
        - input data can have malicious content to force the program to do unintended way
        - e.g. maliciously created image with invalid dimensions can force the program to
        allocate buffers of incorrect sizes
    * **Race Conditions**
        - situation where output of an event depends on ordered or timed outputs
        - race condition can become source of vulnerability when required ordered events do
        not occur in correct order or proper time
    * **Weak Security Practice**
        - developers should use security techniques and libraries created, tested and verified
        - should not use own security algorithms, as they can create new vulnerabilities
        - e.g. using well-known and widely-used authentication, authorization and encryption
    * **Access Control Problems**
        - Access Control: process of controlling who does what, ranging from accessing
        physical equipment to resources such as files
        - improper use of access controls create security vulnerabilities
        - all access controls and security practices can be overcome if attacker has physical
        access to equipment

Software Updates
----------------
    * to stay current and avoid exploitation of vulnerabilities
    * OS and software companies release patches and updates almost every day
    * even with finding and patching vulnerabilities, new ones are discovered regularly
    * some organizations use third party security researchers or own penetration testing teams
    * e.g. Google's Project Zero

`back to top <#cyber-security>`_

Cryptography
============

* `Cryptanalysis`_, `Cryptology`_, `Encryption`_, `Kerckhoffs' Principle`_, `Hashing`_
* `Certificate Authority`_
* used to secure communication techniques and data
* allows confidentiality, integrity, authentication and non-repudiation

Cryptanalysis
-------------
    * finding weakness and breaking cryptographic systems
    * cryptographic systems must be analysed before declaring secure

Cryptology
----------
    * scientific and mathematical study of cryptography and cryptanalysis

Encryption
----------
    * used to secure data in motion and at rest to protect confidentiality of data
    * plain text, human readable form, is inserted into encryption algorithm to convert into
    cipher text, unreadable form
    * encryption does not prevent interception, only viewing the data
    * a secret key is also used in algorithm to convert the text
    * if the key is not used during encryption, the cipher text can be fed into the algorithm
    to get the original text
    * with using a key during encryption, attacker cannot decrypt the cipher text without it
    * since algorithms are well known, the confidentiality of data depends on the secret key
    * **Symmetric Encryption**
        - use only one key for encrytping and decrypting
        - fast but have key distribution problem
        - attacker can get the key and use it
        - e.g DES, 3DES, RC4, AES
    * **Asymmetric Encryption**
        - use two keys, public and private keys
        - data encrypted with public key can only be decrypted with private key, vice versa
        - slower than symmetric encryption, but more secure and no key distribution problem
        - public key is transmitted through insecure medium, hence the name, to only encrypt
        - since slower, it is only used for encrypting a shared secret, such as symmetric key,
        not an actual message
        - e.g. RSA is widely used in areas such as SSL/TLS

Kerckhoffs' Principle
---------------------
    * only secrecy of the key provides security
    * security of cryptographic system should not rely on the secrecy of algorithm
    * it is easy to replace a key than an algorithm
    * if a key is compromised, only just replace the key
    * keys can be changed with time interval to limit any potential risk
    * switching encryption algorithms, security through obscurity, is not practical

Hashing
-------
    * one-way mathematical algorithms to transform data
    * guarantee the integrity of data during transmission or storage
    * **Characteristics**
        - variable length input results in fixed length output hash, message digest
        - if one bit in input changes, the result is completely different
        - cannot revert back from hashed message into original
    * also used to protect confidentiality of data, such as password databases
    * passwords should always be stored in hashed format
    * brute force, dictionary and rainbow table attacks can be used to attack stolen hashes
    * hashing algorithm should be deprecated when it becomes easy to find multiple inputs that
    produce same message digest, e.g. MD5, SHA-1
    * SHA-2, SHA-256, SHA-512 and SHA-3 variants are not appropriate for passwords, as they are
    too quick when using brute force attacks with GPUs, ASICs and FPGAs
    * PBKDF2, bcrypt, scrypt and Argon2 should be used for hashing passwords
    * **Checking File Integrity**
        -

Certificate Authority
---------------------
    * a trusted public third party digital notary
    * companies give the CA their public key, but keep the private key in secret
    * the CA constructs digital certificate, signs it, and give it to companies to put in
    the root of their web server
    * it authenticates that the company can be trusted by customers
    * **Example**
        - browser submit a list of supported algorithms to the web server
        - the web server selects an algorithm and sends its digital certificate
        - browser verify that the certificate is not revoked or expired
        - if certificate is valid, browser extracts the server's public key from it
        - browser generates a random value, pre-master secret, encrypts it with the server's
        public key and send it to the server
        - the server decrypts with its private key to get the pre-master secret
        - browser and server use same pre-master secret to create a same master secret
        - the master secret is used by both to create symmetric session keys for encrypting,
        decrypting and hashing
    * **Verifying Certificate**
        - digital signature: hashed public key of a company, encrypted with the CA's private
        key
        - browser gets the CA's digital certificate from trusted root certificate store
        - browser decrypts the encrypted hash with CA's public key, found on the certificate
        - browser also hashes the company's public key
        - the two hashes should match

`back to top <#cyber-security>`_

Security Systems
================

* `Threat Agents`_, `Mitigation`_, `Firewall`_, `IDS and IPS`_, `Decoy System`_, `Wireless Security`_
* `Password Guideline`_, `Data Security`_, `Port Scanning`_, `Penetration Testing`_
* `Behaviour-Based Security`_, `Impact Reduction`_, `Risk Management`_
* `Security Playbook`_, `Best Practices`_, `Other Appliances`_
* security and convenience should be balanced, if one side is too high, other suffers
* on personal devices, at least turn on firewall, install antivirus and antispyware, manage OS
and browser and setup password protection
* if using IoT devices, set them up on isolated network

Threat Agents
-------------
    * assets: valuable things of a company, can be hardware, software, data, information or
    employees
    * a threat can change or damage the assets
    * threat agents/actors carry out the threats, e.g. natural disaster, malware, hackers
    * threat agents exploit the vulnerability
    * exploits are named after the exploited vulnerability, e.g. CVE-2022-22965
    * **Risk**
        - combination of probability of event or loss and its consequence or impact
        - risk can be reduced or mitigated, not eliminated
        - some vulnerabilities can be eliminated, but that does not eliminate the risk 100%
        - encryption, hashing, VPN, firewall, intrusion detection and prevention can reduce
        risk
        - risk can be transferred, e.g. purchasing cyber security insurance, cloud computing
        - accept the risk if the cost to protect a resource outweigh the cost of losing
    * identify critical assets, what business processes require them
    * find out the interference and risks to the operations, and the most cost-effective way
    of reducing them
    * check the ones with the highest and most negative outcomes, and prioritize as necessary

Mitigation
----------
    * mitigation reduce but do not eliminate the potential for attacks
    * e.g. command injection vulnerability in ``/bin/sh``
        - mitigated by dropping privileges to rUID if ``/bin/sh`` is run as SUID with eUID as 0,
        but rUID not 0
        - use ``sh -p`` to disable it
    * e.g. wireshark split into two programs to reduce attack surface
        - one that dumps traffic, which needs root privileges
        - one that analyses traffic
    * sandbox programs from sensitive data and capabilities

Firewall
--------
    * filter traffic based on rules, can be physical hardware or software
    * can permit or deny traffic both inbound and outbound, but packets can sneak by firewall
    * can filter by criteria such as source IP, destination IP, protocols, ports
    * one side of the firewall is trusted and under admin control
    * helps prevent unauthorized access to or from a system or network
    * network activities in both directions are logged
    * e.g. Cisco 4100 has capabilities of ISR router and advanced network management and
    analytics
    * **Hardware Based**
        - also called network-based firewall
        - protect trusted inside from untrusted outside
        - can be placed between company edge router to the ISP and autonomous system
        - can also place internally where router does not connect to ISP
    * **Software Based**
        - also called host-based firewall
        - only protect single system, can mitigate the risk of attack spreading from one
        machine to another
        - most OS have software based firewall built in
        - anything other than the host can be considered unsafe
    * **Stateless Packet Filtering**
        - sessionless, each packet is isolated piece of communication
        - use less memory and time, low overhead and high throughput
        - but cannot make complex decisions based only communication stage
        - only base on access control lists, referencing, IP addresses, protocols and ports
        - can be fooled if attacker spoofs an IP address
    * **Stateful Packet Filtering**
        - use sessions, understand stages of TCP connection
        - can be aware of spoofed IP addresses
        - after TCP connection is established, packets can flow without further checking
    * **ALG**
        - Application Layer Gateway, apply based on applications like HTTP, SSL/TLS, FTP, DNS
        and VoIP
        - check deeper into protocols, understand how a protocol should work at Layer 7
        - stateful and can filter commands in the data stream
        - DPI (Deep Packet Inspection) looks into protocols and their behavior
        - some ISP use DPI to scan contents of packet to reroute or drop
        - DCI (Deep Content Inspection) examine entire file, such as email attachment, and
        can decode and decompress files
    * **Network Layer Firewall**
        - filter based on source and destination IP addresses
    * **Transport Layer Firewall**
        - filter based on source and destination data ports and connection states
    * **Application Layer Firewall**
        - filter based on application, program or service
    * **Context Aware Layer Firewall**
        - filter based on user, device, role, application type and threat profile
    * **Proxy Server**
        - filter web content requests such as URLs, domain names and media types
    * **Reverse Proxy Server**
        - placed in front of web servers
        - protect, hide, offload and distribute access to web servers
    * **NAT Firewall**
        - Network Address Translation firewall hide the private addresses of network hosts
* **IDS and IPS** <a id="ids-and-ips"></a>
    * protect from malicious traffic that originates within the network
    * have more logic and learning than firewall
    * **IDS**
        - Intrusion Detection System, out of band and get copies of network traffic
        - read and compare packets against known threat signatures database
        - traffic can still flow even if IDS sensor goes down
        - can alert admin and automatically tell firewall to block traffic after observation
        - only to detect, log and report, will not take action and not prevent attacks from
        happening
        - can be considered as visibility device, dedicated network device or one of several
        tools in a server
        - packets collected can be analysed
        - scanning performed can slow down network, and usually placed offline
        - data is copied by a switch and forwarded to IDS for offline detection
    * **IPS**
        - Intrusion Prevention System, inline and original traffic must pass through
        - can block or deny traffic based on positive rules or signature match
        - has latency since traffic is processed live
        - traffic will stop if IPS goes down
        - can alert admin, automatically tell firewall to block traffic after observation, and
        stop traffic in its tracks
        - can be considered as control device
    * e.g. Snort, Cisco Sourcefire
    * vulnerable to false positives, normal activity flagged as malicious, and false
    negatives
    * need to be constantly tuned, can be host based or network based
    * host based can deal with encrypted traffic that will be decrypted for processing, can
    detect attacks that evade the network based, but slow down the system
    * host based IDS look for network activity, and host based IPS for system activity
    * signature based act like anti-virus software, detect attacks by looking for patterns
    * anomaly based compare and establish baseline to malicious things
    * signature based act like anti-virus software, detect attacks by looking for patterns

Decoy System
------------
    * deployed on network to lure potential attackers away from critical systems
    * honeypot: server with no security features by design
    * honeynet: network with no security features by design
    * allow security specialists to collect information about attacker activities
    * encourage attackers to stay long enough for admins to document and respond to attack, and
    refine firewall rules based on behaviours
    * deception software: new type of honeypot, can be centrally managed, made to work with
    other security software, and run through virtualization
    * intruders can be fooled at many layers, e.g. network, endpoint, application, fake data
    * endpoint can be setup to look like it is running particular version of OS
    * decoy documents can be embedded with a tracking capability

Wireless Security
-----------------
    * making a router to not broadcast SSID (Service Set Identifier) is not adequate security
    * should change preset SSID and default password
    * encrypt wireless communication by using at least WPA2 feature
    * update all wireless devices as soon as security updates are available
    * use wired connection if possible, use a trusted VPN service
    * **Public Wi-Fi**
        - best not to send personal data on public wifi
        - make sure device cannot share file and media, or require user authentication if
        sharing is needed
        - use VPN to prevent interception
    * turn off bluetooth if not using

Password Guideline
------------------
    * NIST (National Institute of Standards and Technology) has published improved password
    requirements
    * passwords should be at least eight characters, but less than 64
    * do not use common and easy to guess ones
    * do not impose composition rules, e.g. to include lower and uppercase letters
    * user should be able to see the password when typing
    * allow all printing characters and spaces
    * no password hints and expiration period
    * no knowledge-based authentication

Data Security
-------------
    * always encrypt data when transmitting, or even when stored, if data is sensitive
    * **Data Deletion**
        - data deleted, e.g. in recycle bin, can still be recovered with forensic tools, as there
        is a magnetic trace left on the hard drive
        - to be unrecoverable, data must be overwritten with ones and zeros multiple times
        - e.g. SDelete, Shred, Secure Empty Trash
        - the most certain way is to physically destroy the storage device
        - always check if data is stored online in the cloud or not
    * **Terms of Service**
        - legally binding contract with rules between user, service provider and other users
        - Data Use Policy: how service provider will collect, use and share user data
        - Privacy Settings: allow user to control who can see and access information
        - Security Policy: show what the company is doing to secure user data
    * always read the Terms of Service when registering for a new service
    * change privacy settings rather than the default one
    * limit how data is shared and with whom
    * review service provider's security policy to understand how data is protected
    * change password often, use complex one and two factor authentication
    * **2FA**
        - two factor authentication require second token to verify identity
        - e.g. physical object, biometric scan or verification code
        - OAuth: Open Authorization, standard protocol to use credentials to access
        third-party applications without exposing password

Port Scanning
-------------
    * each application is assigned an identifier called port number
    * the port is used on both ends of transmission to get data to correct application
    * port scanning is probing a host for open ports
    * can be used maliciously to identify OS and services running on a host
    * can be used by network admin to verify network security policies
    * port scanning should be seen as precursor to a network attack
    * never port scan on public servers or organization's network without permission
    * e.g. Nmap, Zenmap (GUI Nmap)

Best Practices
--------------
    * perform a risk assessment, create security policies that outline organization
    * apply necessary physical and human resources security measures, use network security
    devices
    * regularly backup data and test recovery, maintain security patches and updates
    * configure access controls, test incident response regularly
    * use network monitoring, analytics and management tools
    * implement comprehensive endpoint security solution, e.g. using enterprise level
    antimalware
    * educate users and employees, e.g. SANS Institute
    * always encrypt sensitive data

Behaviour-Based Security
------------------------
    * capture and analyse flow of communication between user and destination
    * any changes in normal patterns are regarded as anomalies
    * **Honeypot**
        - lure attacker by appealing to their predicted pattern of malicious behaviour
        - once attacker is inside, network admin can capture, log and analyse to build better
        defense
    * **Cisco Cyber Threat Defense Solution Architecture**
        - provide greater visibility, context and control
        - aim is to know who the attacker is, what type of attack and where, when and how the
        attack is taking place
        - the architecture use many security technologies
    * **NetFlow**
        - gather information about the flowing data
        - include who and what devices, when and how users and devices access the network
        - important component in behaviour-based detection and analysis
        - switches, routers and firewalls with NetFlow can report information about data
        entering, leaving and travelling
        - NetFlow collectors collect, store and analyse data, which can be used to establish
        baseline behaviours on more than 90 attributes

Penetration Testing
-------------------
    * assess a computer system, network or organization
    * try to breach systems, people, processes and code to uncover vulnerabilities which could
    be exploited
    * information gathered during testing is used to improve system defense
    * **Planning**
        - pen tester gather information about target system or network
        - find for potential vulnerabilities and exploits to use against
        - conduct passive or active reconnaissance and vulnerability research
    * **Scanning**
        - do active reconnaissance to probe target system and identify potential weakness
        - include port scanning, vulnerability scanning and establishing active connection to
        the target
    * **Gaining Access**
        - attempt to gain access to target system and sniff network traffic
        - launch exploit with payload, breach physical barriers
        - social engineer, breach access controls security
        - exploit website, software and hardware vulnerabilities or misconfigurations
        - crack weak encrypted wifi
    * **Maintaining Access**
        - maintain access to the target to find out vulnerable data and systems
        - need to remain undetected, such as using backdoors, Trojans, rootkits
        - after setting the infrastructure, pen tester will gather data that are considered
        valuable
    * **Analyse and Report**
        - pen tester provide feedback with reports
        - recommend updates to products, policies and training to improve security

Impact Reduction
----------------
    * when security breach occurs, must act fast to contain damage
    * **Communicate**
        - create transparency, which is critical in the situation
        - inform employees and clients through direct and official announcements
    * **Be accountable**
        - respond in honest and genuine way
        - take responsibility where the organization is at fault
    * **Provide details**
        - explain why the breach took place and compromised information
        - take care of client costs associated with the result of the breach
    * **Find the cause**
        - take steps to understand what caused and facilitated the breach
        - hire forensics experts if needed
    * **Apply lessons learned**
        - apply lessons learned from forensic investigations to prevent similar breaches
    * **Check again**
        - attackers will often attempt to leave a backdoor
        - make sure all systems are clean and nothing else has been compromised
    * **Educate**
        - raise awareness, train and educate employees, partners and clients on how to prevent
        future breaches

Risk Management
---------------
    * formal process to identify and assess risk to reduce impact
    * cannot eliminate risk completely, but determine acceptable levels of impacts
    * the cost of control should never be more than the value of protected asset
    * **Frame**
        - identify threats that increase risk
        - processes, products, attacks, potential failure or disruption
        - negative >perception of organization reputation, potential legal liability or loss of
        intellectual property
    * **Assess**
        - determine the severity of each threat
        - prioritise risk by assessing financial impact, quantitative, or scaled impact on
        operation, qualitative
    * **Respond**
        - develop plan to reduce overall risk exposure
        - detail where risk can be eliminated, mitigated, transferred or accepted
    * **Monitor**
        - continuously review any reduced risk
        - as not all risks can be eliminated, closely monitor any accepted threats

Security Playbook
-----------------
    * a collection of repeatable reports that outline standardized process for incident
    detection and response
    * provide guidance to identify risk, implement safeguards and training
    * make flexible response plan, automate if possible, to minimise impact
    * have security measures to do after security breach
    * describe and clearly define inbound and outbound traffic
    * provide trends, statistics, counts, usable and quick access to key statistics and metrics
    * correlate events across all relevant data sources

Other Appliances
----------------
    * **Router**
        - primarily used to interconnect various network segments and route traffic
        - provide basic traffic filtering capabilities
        - e.g. Cisco ISR 4000 can run IPS, encryption and VPN
    * **VPN**
        - Virtual Private Network
        - let remote employees use a secure encrypted tunnel to connect to organization's
        network
        - can securely interconnect branch offices with central office network
        - e.g. Cisco AnyConnect Secure Mobility Client
    * **Antimalware**
        - use signatures or behavioral analysis of applications to identify and block
        malicious code
        - e.g. Cisco Advanced Malware Protection (AMP)
    * web and email security appliances, decryption devices, client access control servers
    and security management systems are also available
    * **Cisco AMP Threat Grid**
        - allow Cisco Secure Operations Center team to gather more accurate, actionable data
        - Incidence Response team has access to valid information to quickly analyse
        - Threat Intelligence team can use the analysis to improve organization's security
        infrastructure
        - Security Infrastructure Engineering team can consume and act on threat information
        faster, usually automated
    * **Cisco CSIRT**
        - Computer Security Incident Response Team, provide assessment, mitigation planning,
        incident trend analysis and security architecture review
        - collaborate with FIRST (Forum of Incident Response and Security Teams), NSIE
        (National Safety Information Exchange), DSIE (Defense Security Information Exchange),
        and DNS-OARC (DNS Operations Analysis and Research Center)
    * there are other several national and public CSIRT organizations
    * **SIEM**
        - Security Information and Event Management, facilitate early detection of attacks
        - collect and analyse security alerts, logs and real-time and historical data
    * **DLP**
        - Data Loss Prevention, stop sensitive data from being stolen or escaping a network
        - monitor and protect data in use, in motion and at rest
    * **Cisco ISE and TrustSec**
        - Identity Services Engine and TrustSec
        - enforce user access to resources by creating role-based access control policies


`back to top <#cyber-security>`_

Linux
=====

* `Command Line`_, `File System`_, `Environment Variables`_, `Pipes`_, `Permissions`_
* `ELF`_, `Processes`_, `System Calls`_, `Signals`_
* open-source and can configure as user desires
* various tools and utilities available

Command Line
------------
    * also called shell, takes commands, executes and outputs the result, e.g. cat, ls, grep
    * most convenient way to interact with the computer
    * use man pages to view command manual pages, e.g. ``man cat``

File System
-----------
    * Linux organizes files into unified file system, starting at the root ``/``
    * ``/usr``: Unix System Resource, containing all system files
    * ``/usr/bin``: executables, ``/usr/lib``: shared libraries, ``/usr/share``: program resources
    * ``/etc``: system configs, ``/var``: logs, caches, ``/home``: user data
    * ``/proc``: runtime process data, ``/tmp``: temporary data
    * **Directories**
        - each directory can have several files
        - each process has a current working directory
    * **Paths**
        - absolute paths: start with ``/``, e.g. ``/usr/bin``, ``/home/hello/world``
        - relative paths: relative to current working directory, don't start with ``/``,
        e.g. ``.``: current directory, ``..``: previous directory
    * **Type of files**
        - ``-``: regular file, ``d``: directory, ``l``: symbolic link, ``p``: named pipe
         ``c``: character device file, ``b``: block device file, ``s``: Unix socket
    * **Symbolic/Soft Links**
        - file that reference another file
        - created with ``ln -s`` command
        - links to relative paths are also relative
    * **Hard Links**
        - reference original file directly using inode
        - create with ``ln`` command without flags

Environment Variables
---------------------
    * set of key/value pairs passed into processes when launched
    * ``PATH``: directories to search for programs
    * ``PWD``: current working directory
    * ``HOME``: home directory path
    * ``HOSTNAME``: name of the system
    * can print environment variables with ``env`` command

Pipes
-----
    * unidirectional flow of information
    * **Unnamed Pipes**
        - commonly used to direct data from one command to another
    * **Named Pipes**
        - also called FIFOs, created with ``mkfifo`` command
        - to help facilitate data flow

Permissions
-----------
    * every process has user ID and GID, every file and directory is owned by user and group
    * child processes inherit from parent processes
    * UID 0: admin user/root, required for installing software, loading drivers, etc.
    * run SUID binaries to elevate privileges, e.g. sudo, su, newgrp
    * SUID: execute with eUID of file owner rather than parent process
    * SGID: execute with eGID of file owner rather than parent process
    * Sticky: used for shared directories to limit file removal to file owners
    * effective eUID/eGID are used for most access checks
    * real UID/GID are of process owner, used for things such as signal checks
    * saved UID/GID can be switched by process as its eUID/eGID, used for temporarily dropping
    privileges

ELF
---
    * Executable and Linkable Format, a binary file format
    * defines a program address on disk, how it should be loaded and executed
    * stores data about a program such as compiled architecture
    * **Program Headers**
        - specify information needed to prepare for program execution
        - source of information used when loading a file
        - ``INTERP``: define library to be used to load ELF into memory
        - ``LOAD``: define part of the file to be loaded into memory
        - segments defined by the program header are the only true information to load it
        - can build a file that only have program headers
    * **Section Headers**
        - define what the segments have inside them
        - not strictly necessary part of ELF, as they are only metadata for introspection,
        debugging, etc.
        - ``.text``: executable code of program
        - ``.plt``, ``.got``: to resolve and dispatch library calls
        - ``.data``: pre-initialized global writable data, e.g. global array with initial values
        - ``.rodata``: global read-only data, e.g. string constants
        - ``.bss``: uninitialized global writable data, e.g. global array without initial values
    * **Symbols**
        - to find libraries, resolve function calls into the libraries, etc.
        - used by binaries and libraries that use dynamically loaded libraries
    * **Tools for ELF**
        - gcc: create ELF
        - readelf: parse ELF header
        - objdump: parse ELF header and disassemble source code
        - nm: view ELF symbols
        - patchelf: change ELF properties
        - objcopy: swap out ELF sections
        - strip: remove information such as symbols
        - kaitai struct: web-based tool to view ELF interactively

Processes
---------
    * a running program, which is an executable file
    * e.g. the shell is just a file on the file system and becomes process when executed
    * every process has state, priority, parent, siblings, children, shared resources, virtual
    memory space and security context such as eUID
    * **Life Cycle**
        - create, load, initialize
        - launch, read arguments and environment
        - do task, terminate
    * **Creation**
        - parent and child: ``fork`` and ``clone`` syscalls create nearly exact copy of calling process
        - child process uses ``execve`` syscall to replace itself with another process
        - e.g. when using ``/bin/cat`` in bash, bash forks itself into old parent process and
        child process, and the child ``execve`` and replace itself with ``/bin/cat``
    * **Loading**
        - kernel checks for executable permissions, and if file is not executable, ``execve``
        will fail
        - kernel reads the beginning of the file to make a decision
        - ``#!``: kernel extract the interpreter from the line and execute it with original file
        as argument
        - one of ``/proc/sys/fs/binfmt_misc/`` formats: kernel execute the interpreter specified
        for that format with original file as argument
        - dynamically-linked ELF: kernel read the defined interpreter/loader, load the
        interpreter and original file, and give control to the interpreter
        - statically-linked ELF: kernel will load it
    * **Dynamically-Linked ELFs Loading**
        - kernel load program and its interpreter
        - interpreter locate libraries in ``LD_PRELOAD``, ``/etc/ld.so.preload``, ``LD_LIBRARY_PATH``,
        ``DT_RUNPATH`` or ``DT_RPATH``, system-wide configuration ``/etc/ld.so.conf``, and ``/lib``
        and ``/usr/lib``
        - interpreter load the libraries, which can cause to load other libraries, and update
        relocations
    * **Virtual Memory**
        - dedicated to each process, physical memory is shared among whole system
        - contains binary, libraries, heap, stack, memory mapped by the program, helper
        regions, kernel code in top half of memory which is inaccessible to the process
        - can view the memory at ``/proc/self/maps``
    * ``libc.so``: standard C library, linked by almost every process, provides functions such as
    printf, scanf, malloc, free
    * **Initialization**
        - ELF can specify constructors, functions that run before the program is launched
        - e.g. libc can initialize memory regions for dynamic allocations
        - can specify own constructor with
        ``__attribute__((constructor)) void foo() { puts("bar"); }``
        - specific constructors are especially used for ``LD_PRELOAD``
    * some exposed functionalities allow to inject library into a process
    * **Launch**
        - ELF calls ``__libc_start_main()``, which calls the program's ``main()``
        - can override ``__libc_start_main()`` with custom using ``LD_PRELOAD``
    * **Reading Environment**
        - loaded objects such as binaries and libraries, arguments, and environment are the
        only input from outside at launch
        - e.g. ``int main(int argc, void** argv, void** envp);``
    * the binary's import symbols are resolved using the libraries' export symbols, done when
    the binary is loaded
    * **Interaction**
        - processes interact with outside via system calls
        - can trace a process system calls using ``strace``
        - processes also interact by sharing memory with other processes
        - shared memory require system calls to establish, but after that, communicate without
        syscalls
        - e.g. use a shared memory-mapped file in ``/dev/shm``
    * **Termination**
        - processes only terminate by receiving unhandled signal or calling ``exit()`` syscall
        - after termination, a process remain in zombie state
        - when ``wait()`` is called by its parent, the exit code is returned to the parent and
        the process is freed
        - if parent dies without ``wait()``, a process parent is changed to PID 1 until it is
        cleaned up

System Calls
------------
    * well-define interfaces that rarely change
    * over 300 system calls in Linux
    * **Example**
        - ``open()``: return file/new file descriptor
        - ``read()``: read data from file descriptor
        - ``write()``: write data to file descriptor
        - ``fork()``: fork an identical child process, return 0 if child and PID of child if
        parent
        - ``execve()``: replace the process
        - ``wait()``: wait child termination, return its PID and write its status into ``*wstatus``
        - ``syscall()``: invoke specific syscall
    * program such as ``cat`` combine open, read and write syscalls

Signals
-------
    * pause execution and invoke handlers, functions that take signal number as argument
    * default action, usually kill, is used if no handler
    * ``SIGKILL``, signal 9, and ``SIGSTOP``, signal 19, cannot be handled
    * ``SIGSTOP``, signal 20 sent with ``CTRL+Z``, can be caught
    * check ``man 7 signal`` and ``kill -l``
    * there are some system calls that are not interruptible by signals

`back to top <#cyber-security>`_

Cisco Packet Tracer
===================

* `Tabs`_, `File/Assessment Types`_
* helps practice network configuration and troubleshooting skills
* can simulate networks without having access to physical equipment
* can add devices and connect via cables or wireless, select, delete, inspect, label and group
components, manage network
* can open existing or sample network, save current network  and modify user profile or
preferences

Tabs
----
    * **Physical**
        - provide interface for devices
        - can manage power and installing different modules
    * **Config**
        - does not simulate functionality of a device
        - provide a way to use Packet Tracer-only GUI to configure basic settings
        - helps learn CLI commands and Cisco Internetwork Operating System (IOS)
        - can save, load, erase and export configurations
    * **CLI**
        - access to command line interface of Cisco device
        - require knowledge of device configuration with IOS
        - CLI is necessary for advanced networking implementations
        - any command entered from Config tab are shown in CLI tab
    * **Desktop**
        - desktop interface for some end devices
        - can access IP and wireless configuration, command prompt, web browser and other
        applications
    * **Services**
        - allow to configure a server with HTTP, DHCP, DNS, or other services
* besides Ethernet and console cables, USB console cables can be used to connect devices
* rack: mount heavy devices, such as servers and network devices
* table: place personal devices such as PC, laptop
* shelf: collection of unused devices
* logical mode: provides a high level view of network topology, and ignores physical aspect
* physical mode: takes account of the physical scale and placement of the devices

File/Assessment Types
---------------------
    * pka: activity file, has instructions window, scores, initial network and answer network,
    which cannot be accessed
    * pkt: for simulated network, can embed background images
    * pksz: specific to PTTA, bundle pka file, media and scripting file
    * pkz: deprecated, used to embed images and other files
    * PTMO: Packet Tracer Media Objects, launch pkt or pka file, and configure the network to
    determine answer to the question
    * PTSA: Packet Tracer Skills Assessments, summative skill assessments, standalone and have
    own grading engine

`back to top <#cyber-security>`_

Additional Resources
====================

* `World's Biggest Data Breaches`_
* `Inside the Cunning, Unprecedented Hack of Ukraine’s Power Grid`_
* `Ransomware ‘Remediation’ Firm Exposed: Researchers Weigh in on Paying`_
* `Baltimore Ransomware Attack Takes Strange Twist`_
* `Cybersecurity experts warn Baltimore to stop 'playing' with ransomware attacks`_
* `The Security Mindset`_
* `Cybersecurity Unemployment Rate Drops To Zero Percent`_
* `Network live IP video cameras directory`_
* `Jeep hackers at it again, this time taking control of steering and braking systems`_
* `With 'recall,' Fiat Chrysler makes its car hack worse`_
* `Florida man wins over 1 million miles for hacking United Airlines`_
* `Computer hackers can now hijack toilets`_
* `Baby monitor hacker delivers creepy message to child`_
* `It’s Insanely Easy to Hack Hospital Equipmen`_
* `It’s Way Too Easy to Hack the Hospital`_
* `Here's What We Know About the Massive Cyber Attack That Took Down the Internet on Friday`_
* `How the Dyn DDoS attack unfolded`_
* `MEECES to pieces`_
* `Forget Russia, China And Iran, Up To 80% Of Cybersecurity Threats Are Closer To Home`_
* `Is Your Biggest Security Threat Already Inside Your Organization?`_
* `Insider threat vs. inside threat: Redefining the term`_
* `The 15 biggest data breaches of the 21st century`_
* `Anatomy of the Target data breach: Missed opportunities and lessons learned`_
* `Two-Factor Snafu Opened Door to JPMorgan Breach`_
* `Home Depot: Massive Breach Happened Via Third-Party Vendor Credentials`_
* `Home Depot hackers used vendor log-on to steal data, e-mails`_
* `The Truth About Home Depot's Security Breach: Hacking Was Easy`_
* `53 Million Email Addresses Stolen in Home Depot Hack`_
* `Sony Pictures hack: the whole story`_
* `The OPM hack explained: Bad security practices meet China's Captain America`_
* `What to know about the Ashley Madison hack`_
* `'I was sent a video of my wife having sex': Ashley Madison members and their heartbroken spouses reveal the devastating impact last year's hack had on their lives`_
* `This basic security mistake led to the Houston Astros hack that shook baseball`_
* `Kaspersky Lab cybersecurity firm is hacked`_
* `The LastPass security breach: What you need to know, do, and watch out for`_
* `How Hacking Team got hacked`_
* `This big U.S. health insurer just got hacked`_
* `Massive breach at health care company Anthem Inc.`_
* `Premera health insurance hack hits 11 million people`_
* `Inside the Cunning, Unprecedented Hack of Ukraine’s Power Grid`_
* `Ukraine’s Power Grid Gets Hacked Again, a Worrying Sign for Infrastructure Attacks`_
* `How a Typo Stopped Hackers from Stealing $1 Billion from Bank`_
* `SWIFT Banking System Was Hacked at Least Three times This Summer`_
* `Hackers are trading millions of Gmail, Hotmail, Yahoo logins`_
* `Hack Brief: Your Old Myspace Account Just Came Back to Haunt You`_
* `LinkedIn Urges Users To Change Passwords: Hacker Puts 117 Million Accounts Up For Sale`_
* `Yahoo says 500 million accounts stolen`_
* `The Download on the DNC Hack`_
* `‘Guccifer 2.0’ Releases Documents From DCCC Hack`_
* `Hillary Clinton's campaign got hacked by falling for the oldest trick in the book`_
* `DNC chief Podesta led to phishing link ‘thanks to a typo’`_
* `Why Clinton’s Private Email Server Was Such a Security Fail`_
* `U.K. Hospitals Hit in Widespread Ransomware Attack`_
* `Massive ransomware attack hits UK hospitals, Spanish banks`_
* `WannaCry ransomware attack`_
* `WannaCry ransomware: Everything you need to know`_
* `WannaCry ransomware is still spreading fast, but ‘kill switch’ defenses hold for now`_
* `WannaCry hackers still trying to revive attack says accidental hero`_
* `WannaCry: Smaller businesses are at great risk`_
* `WannaCry Hit Windows 7 Machines Most`_
* `Wana Decrypt0r Ransomware Outbreak Temporarily Stopped By "Accidental Hero"`_
* `French researchers find way to unlock WannaCry without ransom`_
* `WannaCry Ransomware Decryption Tool Released; Unlock Files Without Paying Ransom`_
* `Why WannaCry won’t change anything`_
* `Digital Attack Map`_
* `Kaspersky Cyberthreat Real-Time Map`_
* `Deteque Botnet Threat Map`_
* `Fortinet Real-Time Attack Map`_
* `FireEye Cyber Threat Map`_
* `Bitdefender Cyberthreat Real-Time Map`_
* `Threatbutt Internet Hacking Attribution Map`_
* `Threat Cloud`_
* `Akamai Real-Time Web Attack Monitor`_
* `Talos Cyber Attack Map: Top Spam and Malware Senders`_
* `'Bob' outsources tech job to China; watches cat videos at work`_
* `10 Reasons Why Biometrics Won’t Replace Passwords Anytime Soon`_
* `NIST declares the age of SMS-based 2-factor authentication over`_
* `NIST Denounces SMS 2FA - What are the Alternatives?`_
* `Standards body warned SMS 2FA is insecure and nobody listened`_
* `MS08-067: Vulnerability in Server service could allow remote code execution`_
* `The Inside Story Behind MS08-067`_
* `Power grid cyber security 'in chaos' | State ponders ways to guard against attacks by humans as well as Mother Nature`_
* `What hackers inside your company are after: Convenience, Doug Wick, Help Net Security`_
* `Read the Verizon Data Breach Investigations Report`_
* `Everything you need to know about the Heartbleed SSL bug`_
* `DV, OV or EV? How to offer the right SSL Certificate`_
* `Extended Validation Certificates are Dead`_
* `Extended Validation Certificates are (Really, Really) Dead`_
* `Public Key Cryptography: RSA Encryption Algorithm`_
* `Why some cryptographic keys are much smaller than others`_
* `Data in Use Is the Point of Least Resistance`_
* `Heartbleed Explanation`_
* `Heartbleed`_
* `What should you do about “HeartBleed?”`_
* `Don't want to trust Lenovo, Dell or the Chinese government? You don't have to`_
* `Post-quantum cryptography`_
* `China set to launch an 'unhackable' internet communication`_
* `China holds world's first 'unhackable' quantum videoconference in secure communication breakthrough`_
* `Symantec cannot handle SHA-2 and breaks Windows 7 and Server 2008 R2`_
* `Final Report on DigiNotar Hack Shows Total Compromise of CA Servers`_
* `Google is fighting with Symantec over encrypting the internet`_
* `Google takes Symantec to the woodshed for mis-issuing 30,000 HTTPS certs`_
* `Google slaps Symantec for sloppy certs, slow show of SNAFUs`_
* `Google Chrome to Distrust Symantec SSLs for Mis-issuing 30,000 EV Certificates`_
* `Bang! SHA-1 collides at 38762cf7¬f55934b3¬4d179ae6¬a4c80cad¬ccbb7f0a`_
* `Cyberespionage groups are stealing digital certificates to sign malware`_
* `Stolen Foxconn certs used to sign malware used in Kaspersky Lab attack`_
* `How Attackers Steal Private Keys from Digital Certificates`_
* `Malware is being signed with multiple digital certificates to evade detection`_
* `Google Starts Labeling All HTTP Sites as ‘Not Secure’`_
* `3DES is Officially Being Retired`_
* `ICANN’s internet DNS security upgrade apparently goes off without a glitch`_
* `Microsoft Edge and IE Browsers Dropping Symantec Security Certificate Support`_
* `Final Warning: Last chance to replace Symantec SSL certificates`_
* `Mozilla Pushes Back Symantec Distrust Date`_
* `Windows 7 users: You need SHA-2 support or no Windows updates after July 2019`_
* `Cryptography failure leads to easy hacking for PlayStation Classic`_
* `Adobe security team posts public key – together with private key`_
* `Silicon Valley and the FBI Take Their Encryption Fight Behind Closed Doors`_
* `Roger Stone allegedly wanted to use Facebook’s WhatsApp to ‘talk on a secure line’ — here’s what that means`_
* `Solving a blockchain conundrum: Biometrics could recover lost encryption keys`_
* `India authorizes 10 agencies to intercept, monitor, and decrypt citizens' data`_
* `Deciphering the Encryption Paradox`_
* `Encryption backdoors open a Pandora’s Box for cybersecurity`_
* `Encrypted messaging app Signal uses Google to bypass censorship`_
* `Signal Secure Messaging App Now Encrypts Sender's Identity As Well`_
* `Signal Has A Clever New Way To Shield Your Identity`_
* `Windows Server 2008 Requires KB4493730 to Get Future Updates`_
* `Online Thief Cracks Private Keys to Steal $54m in ETH`_
* `Security flaw lets attackers recover private keys from Qualcomm chips`_
* `Hackers Breached a Programming Tool Used By Big Tech and Stole Private Keys and Tokens`_
* `FBI–Apple encryption dispute`_
* `Apple vs FBI: All you need to know`_
* `Facebook, Google, WhatsApp in the firing line as Australia reveals encryption laws`_
* `Apple says 'dangerous' Australian encryption laws put 'everyone at risk'`_
* `Australia passes bill to force tech firms to hand over encrypted data`_
* `Australia data encryption laws explained`_
* `Australia’s encryption-busting law is ‘deeply flawed,’ says tech industry`_
* `What's actually in Australia's encryption laws? Everything you need to know`_
* `Australia's Encryption-Busting Law Could Impact Global Privacy`_
* `Cisco IOS Switching Services Configuration Guide, Routing Between VLANs Overview`_
* `AlliedWare Plus™ OS, Overview of VLANs (Virtual LANs)`_
* `Firewll.cx, The VLAN Concept - Introduction to VLANs`_
* `Cisco ONS 15454 SONET/SDH ML-Series Multilayer Ethernet Card Software Feature and Configuration Guide, Release 4.1.x, Chapter 6, Configuring STP and RSTP`_
* `Cisco, Understanding Rapid Spanning Tree Protocol (802.1w)`_
* `How to Find Any Device’s IP Address, MAC Address, and Other Network Connection Details`_
* `7 Ways to find the MAC address in Windows`_
* `How to Ping a Computer or a Web Site`_
* `How to Ping an IP Address`_
* `How to run a ping test`_
* `OUI Lookup Tool`_
* `Cisco Networking Academy's Introduction to Switched Networks`_
* `Switch Operation for the CCNP BCMSN Exam`_
* `ARP cache: What is it and how can it help you?`_
* `Network Administration: ARP Command`_
* `How to See What Web Sites Your Computer is Secretly Connecting To`_
* `Using the Netstat Command to Monitor Network Traffic`_
* `Netstat tips and tricks for Windows Server admins`_
* `Netstat`_
* `Why There Are Only 13 DNS Root Name Servers`_
* `Why 13 DNS root servers?`_
* `Hosts file hijacks`_
* `6 Surprising Uses for the Windows Hosts File`_
* `how to make the internet not suck (as much)`_
* `The Hosts File and what it can do for you`_
* `What are these 127.0.0.1 entries in my system hosts file?`_
* `Google Public DNS: Get Started`_
* `How to Switch to OpenDNS or Google DNS to Speed Up Web Browsing`_
* `Google DNS: 8.8.8.8 and 8.8.4.4. Benefits and how to use`_
* `Windows - Displaying, Releasing and Renewing a DHCP Lease`_
* `Ipconfig`_
* `How do I find the DNS server being used by my PC?`_
* `Get All DHCP Info with ipconfig Quickly`_
* `Linux and Unix nslookup command`_
* `2016 Dyn cyberattack`_
* `Lessons From the Dyn DDoS Attack`_
* `Distributed denial-of-service attacks on root nameservers`_
* `Someone Just Tried to Take Down Internet's Backbone with 5 Million Queries/Sec`_
* `Attack floods Internet root servers with 5 million queries a second`_
* `Internet's root servers take hit in DDoS attack`_
* `DNS root server attack was not aimed at root servers – infosec bods`_
* `Internet DNS servers withstand huge DDoS attack`_
* `Switcher hacks Wi-Fi routers, switches DNS`_
* `How to detect and fix a machine infected with DNSChanger`_
* `FBI Shuts Down DNSChanger Servers`_
* `Fileless PowerShell malware uses DNS as covert channel`_
* `Covert Channels and Poor Decisions: The Tale of DNSMessenger`_
* `Microsoft Azure data deleted because of DNS outage`_
* `BIND DNS software vulnerability which could lead to DoS attacks exposed`_
* `DNS lookups can reveal every web page you visit, says German boffin`_
* `Protecting networks from DNS exfiltration`_
* `Most companies are unprepared for DNS attacks`_
* `Iranian hackers suspected in worldwide DNS hijacking campaign`_
* `DNS Infrastructure Hijacking Campaign`_
* `DDoS Amped Up: DNS, Memcached Attacks Rise`_
* `U.S. Gov Issues Urgent Warning of DNS Hijacking Attacks`_
* `Emergency Directive 19-01`_
* `Hacker group has been hijacking DNS traffic on D-Link routers for three months`_
* `Network Security First-Step: Firewalls`_
* `DMZ (computing)`_
* `Virtual DMZs in the Cloud`_
* `Firewall DMZ Zone`_
* `Using IDS Sensors in Switched Networks`_
* `SPAN Port or TAP? CSO Beware`_
* `Implementing Networks Taps with Network Intrusion Detection Systems`_
* `SPAN Port Or TAP? White Paper`_
* `Port Mirror vs Network Tap`_
* `How to capture traffic? (SPAN vs TAP)`_
* `Snort: Port Mirroring`_
* `Switch Port Mirroring`_
* `Understanding Social Engineering Attacks`_
* `Top 5 Social Engineering Exploit Techniques`_
* `Phone scammers call the wrong guy, get mad and trash PC`_
* `Amazing mind reader reveals his 'gift'`_
* `Anti Social Engineering Training Video`_
* `DefCon 15 - T112 - No-Tech Hacking`_
* `Walmart Prank`_
* `Tiger Team - The Car Dealer Takedown`_
* `Man Steals People's Banking Information`_
* `The Verizon Data Breach Investigations Report`_
* `Using Windows Firewall with Advanced Security`_
* `Compilation of PandaLabs Reports`_
* `Verizon Data Breach Investigations Report`_
* `Fileless Malware Takes 2016 By Storm`_
* `Fileless Malware Execution with PowerShell is Easier than You May Realize`_
* `A rash of invisible, fileless malware is infecting banks around the globe`_
* `How malware works: Anatomy of a drive-by download web attack`_
* `Steganography: Hiding Data Within Data`_
* `Steganography`_
* `Terrorists and steganography`_
* `Programmer from hell plants logic bombs to guarantee future work`_
* `Phished credentials caused twice as many breaches than malware in the past year`_
* `The Nasty List Phishing Scam is Sweeping Through Instagram`_
* `25% of Phishing Emails Bypass Office 365 Default Security`_
* `It’s not your imagination — tax extortion scams are skyrocketing`_
* `Cybercriminals Spoof Major Accounting and Payroll Firms in Tax Season Malware Campaigns`_
* `Phishing attacks are a worse security nightmare than ransomware or hacking`_
* `Mobile-First Phishing Kit Targets Verizon Customers`_
* `March Madness Scams Give Attackers Fast Break`_
* `Google and Facebook got tricked out of $123 million by a scam that costs small businesses billions every year — here’s how to avoid it`_
* `6 Real Black Friday Phishing Lures`_
* `BBB: Top Valentine's Day scams`_
* `Watch out for these Valentine’s Day scams`_
* `Popular tax software may expose users to phishing attacks`_
* `Attackers Sending Out Fake CDC Flu Warnings to Distribute GandCrab`_
* `Spam Campaign Uses Recent Boeing 737 Max Crashes to Push Malware`_
* `New Sextortion Scam Tries to Scare Users with Fake CIA Investigation`_
* `‘Bad Tidings’ Phishing Campaign Targeting Saudi Government Agencies`_
* `Manipulation tactics that you fall for in phishing attacks`_
* `How these fake Facebook and LinkedIn profiles tricked people into friending state-backed hackers`_
* `Phishers’ techniques and behaviours, and what to do if you’ve been phished`_
* `Cyber spies use fake profile as a ‘honey pot’ to trap male workers`_
* `Someone Hijacks A Popular Chrome Extension to Push Malware`_
* `Russia’s hack into the US election was surprisingly inexpensive, Mueller report shows`_
* `Why Every Employee in Your Organization Should Learn Social Engineering Vocabulary`_
* `SonicWall Phishing IQ Test`_
* `Phishing Quiz: Think you can Outsmart Internet Scammers?`_
* `Can you spot a fake email? Take our phishing IQ test`_
* `Can you spot when you’re being phished?`_


`back to top <#cyber-security>`_
