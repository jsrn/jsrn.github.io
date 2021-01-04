---
layout: post
title: "Storing Proxy Settings in Your SSH Config"
description: Using your SSH configuration to automatically route connections through a jump host.
category: programming
tags: [ssh, productivity]
---

My coworker showed me something cool today. Like a lot of developers, there are certain machines that I find myself SSHing into repeatedly. Not all of them are directly accessible from the network I'm on. Some of them require me to connect via a jump host.

Ordinarily, I manually create a tunnel between a port on my local machine to my final destination via this tunnel. This is cool, but it's a bunch of steps to remember, especially if you're manually creating your SSH tunnels via the command line. _Especially_ if you're trying to remember a bunch of IP addresses.

_Apparently_, you can just add hosts to your `~/.ssh/config`.

```
Host jump-host
    Hostname x.x.x.x
    IdentityFile /path/to/key/proxy.pem
    User ubuntu

Host destination
    Hostname y.y.y.y
    IdentityFile /path/to/key/destination.pem
    User ubuntu
    ProxyJump jump-host
```

With this in place, `ssh destination` gets you there with zero fuss.
