---
layout: post
title: "Owning Metasploitable (a bit) via Apache Tomcat"
description: ""
category: security
tags: [pentesting, metasploitable]
---

*This post is mirrored from my old blog.*

While browsing Twitter, I saw a link to a startup going by the name of HackAServer. In their own words, "HackaServer is an online marketplace between PenTesters and companies with a IT&C infrastructure that requires pentesting, and aims to lower the costs and deliver the best quality to price ratio for manual penetration tests using crowdsourcing."

Upon closer inspection, it appears that companies or individuals looking to get their product given a good going over can attach their server(s) to HackAServer's VPN, kick back while freelance pen-testers break things and write reports, then pay according to HackAServer's pricing system for the most promising looking report. I see no mention as to how much of this bounty goes to the hard working pen tester. At any rate, it looked interesting enough to warrant signing up for an account.

Access to the machines of paying customers is not immediately accessible to new members. New members first have to prove themselves in the Training Ground, finding vulnerabilities on the sample targets and filing out a report to prove that they're at least partially skilled. The first on the list of targets? Our good friend <a title="Metasploitable" href="http://www.offensive-security.com/metasploit-unleashed/Metasploitable">Metasploitable</a>. There's not much to Metasploitable, but it's a good way to people to cut their teeth on various tools and techniques.

If you've ever had a go at it, from here on in will be of little interest to you.

I scanned the host with nmap, and here's what I learned:

	Starting Nmap 5.21 ( http://nmap.org ) at 2012-07-23 17:10 CEST
	NSE: Loaded 36 scripts for scanning.
	Initiating Ping Scan at 17:10
	Scanning 10.1.32.232 [8 ports]
	Completed Ping Scan at 17:10, 0.04s elapsed (1 total hosts)
	Initiating Parallel DNS resolution of 1 host. at 17:10
	Completed Parallel DNS resolution of 1 host. at 17:10, 0.03s elapsed
	Initiating SYN Stealth Scan at 17:10
	Scanning 10.1.32.232 [1000 ports]
	Discovered open port 53/tcp on 10.1.32.232
	Discovered open port 139/tcp on 10.1.32.232
	Discovered open port 80/tcp on 10.1.32.232
	Discovered open port 3306/tcp on 10.1.32.232
	Discovered open port 445/tcp on 10.1.32.232
	Discovered open port 23/tcp on 10.1.32.232
	Discovered open port 21/tcp on 10.1.32.232
	Discovered open port 25/tcp on 10.1.32.232
	Discovered open port 22/tcp on 10.1.32.232
	Discovered open port 5432/tcp on 10.1.32.232
	Discovered open port 8009/tcp on 10.1.32.232
	Discovered open port 8180/tcp on 10.1.32.232
	Completed SYN Stealth Scan at 17:10, 0.95s elapsed (1000 total ports)
	Initiating Service scan at 17:10
	Scanning 12 services on 10.1.32.232
	Completed Service scan at 17:10, 31.16s elapsed (12 services on 1 host)
	Initiating OS detection (try #1) against 10.1.32.232
	Retrying OS detection (try #2) against 10.1.32.232
	Retrying OS detection (try #3) against 10.1.32.232
	Retrying OS detection (try #4) against 10.1.32.232
	Retrying OS detection (try #5) against 10.1.32.232
	Initiating Traceroute at 17:10
	Completed Traceroute at 17:10, 0.03s elapsed
	Initiating Parallel DNS resolution of 2 hosts. at 17:10
	Completed Parallel DNS resolution of 2 hosts. at 17:10, 0.01s elapsed
	NSE: Script scanning 10.1.32.232.
	NSE: Starting runlevel 1 (of 1) scan.
	Initiating NSE at 17:11
	Completed NSE at 17:11, 32.28s elapsed
	NSE: Script Scanning completed.
	Nmap scan report for 10.1.32.232
	Host is up (0.032s latency).
	Not shown: 988 closed ports
	PORT     STATE SERVICE     VERSION
	21/tcp   open  ftp         ProFTPD 1.3.1
	22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
	| ssh-hostkey: 1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
	|_2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
	23/tcp   open  telnet      Linux telnetd
	25/tcp   open  smtp        Postfix smtpd
	|_smtp-commands: EHLO metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
	53/tcp   open  domain      ISC BIND 9.4.2
	80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch)
	|_html-title: Site doesn't have a title (text/html).
	139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
	445/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
	3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
	| mysql-info: Protocol: 10
	| Version: 5.0.51a-3ubuntu5
	| Thread ID: 14
	| Some Capabilities: Connect with DB, Compress, SSL, Transactions, Secure Connection
	| Status: Autocommit
	|_Salt: 9QlM{C|fjd6}5.'t:b\*
	5432/tcp open  postgresql  PostgreSQL DB
	8009/tcp open  ajp13?
	8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
	No exact OS matches for host (If you know what OS is running on it, see http://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=5.21%D=7/23%OT=21%CT=1%CU=38445%PV=Y%DS=2%DC=T%G=Y%TM=500D69A4%P=
	OS:i686-pc-linux-gnu)SEQ(SP=C5%GCD=1%ISR=C9%TI=Z%CI=Z%II=I%TS=7)OPS(O1=M558
	OS:ST11NW5%O2=M558ST11NW5%O3=M558NNT11NW5%O4=M558ST11NW5%O5=M558ST11NW5%O6=
	OS:M558ST11)WIN(W1=16A0%W2=16A0%W3=16A0%W4=16A0%W5=16A0%W6=16A0)ECN(R=Y%DF=
	OS:Y%T=40%W=16D0%O=M558NNSNW5%CC=N%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q
	OS:=)T2(R=N)T3(R=Y%DF=Y%T=40%W=16A0%S=O%A=S+%F=AS%O=M558ST11NW5%RD=0%Q=)T4(
	OS:R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F
	OS:=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T
	OS:=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RI
	OS:D=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

	Uptime guess: 2.043 days (since Sat Jul 21 16:10:00 2012)
	Network Distance: 2 hops
	TCP Sequence Prediction: Difficulty=197 (Good luck!)
	IP ID Sequence Generation: All zeros
	Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux

	Host script results:
	| smb-os-discovery:  
	|   OS: Unix (Samba 3.0.20-Debian)
	|   Name: WORKGROUP\Unknown
	|_  System time: 2012-07-23 17:11:07 UTC-4

	TRACEROUTE (using port 199/tcp)
	HOP RTT      ADDRESS
	1   30.46 ms 10.8.0.1
	2   31.07 ms 10.1.32.232

	Read data files from: /usr/share/nmap
	OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
	Nmap done: 1 IP address (1 host up) scanned in 79.30 seconds
	           Raw packets sent: 1113 (52.750KB) | Rcvd: 1084 (47.018KB)

We can see this machine is broadcasting a lot of information about itself. I felt like pounding on Apache Tomcat, so I pointed my browser at 10.1.32.232:8180 to see what I could see. The standard Apache Tomcat homepage, with a handy link to the (password protected) management panel. Consulting google, I found that the default username and password for Apache Tomcat was tomcat/tomcat. 

Fortunately, (or not so, considering this machine is set up to be deliberately vulnerable) these credentials allowed access to the control panel. From here, the Apache Tomcat world was my oyster. Along with being able to modify the user accounts of assorted Tomcat managers, I was able to deploy any java webapp of my choosing! I chose a backdoor shell, giving me much greater control of the system. Unfortunately though, Tomcat was running as an unprivileged user, tomcat55.

At least we can take a look around.

	$ cat /etc/passwd
	root:x:0:0:root:/root:/bin/bash
	daemon:x:1:1:daemon:/usr/sbin:/bin/sh
	bin:x:2:2:bin:/bin:/bin/sh
	sys:x:3:3:sys:/dev:/bin/sh
	sync:x:4:65534:sync:/bin:/bin/sync
	games:x:5:60:games:/usr/games:/bin/sh
	man:x:6:12:man:/var/cache/man:/bin/sh
	lp:x:7:7:lp:/var/spool/lpd:/bin/sh
	mail:x:8:8:mail:/var/mail:/bin/sh
	news:x:9:9:news:/var/spool/news:/bin/sh
	uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
	proxy:x:13:13:proxy:/bin:/bin/sh
	www-data:x:33:33:www-data:/var/www:/bin/sh
	backup:x:34:34:backup:/var/backups:/bin/sh
	list:x:38:38:Mailing List Manager:/var/list:/bin/sh
	irc:x:39:39:ircd:/var/run/ircd:/bin/sh
	gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
	nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
	libuuid:x:100:101::/var/lib/libuuid:/bin/sh
	dhcp:x:101:102::/nonexistent:/bin/false
	syslog:x:102:103::/home/syslog:/bin/false
	klog:x:103:104::/home/klog:/bin/false
	sshd:x:104:65534::/var/run/sshd:/usr/sbin/nologin
	msfadmin:x:1000:1000:msfadmin,,,:/home/msfadmin:/bin/bash
	bind:x:105:113::/var/cache/bind:/bin/false
	postfix:x:106:115::/var/spool/postfix:/bin/false
	ftp:x:107:65534::/home/ftp:/bin/false
	postgres:x:108:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
	mysql:x:109:118:MySQL Server,,,:/var/lib/mysql:/bin/false
	tomcat55:x:110:65534::/usr/share/tomcat5.5:/bin/false
	distccd:x:111:65534::/:/bin/false
	user:x:1001:1001:just a user,111,,:/home/user:/bin/bash
	service:x:1002:1002:,,,:/home/service:/bin/bash
	telnetd:x:112:120::/nonexistent:/bin/false
	proftpd:x:113:65534::/var/run/proftpd:/bin/false

There's us, at tomcat55, and a list of the other users. I've said as much as I want to say about Metasploitable, but I will say that strong passwords aren't a common trait on this system, and many of these are easily brute-forcible.