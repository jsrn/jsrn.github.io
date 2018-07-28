---
layout: post
title: "EasyPreload"
tagline: "a tool for intercepting calls with LD_PRELOAD"
description: ""
category: security
tags: [python, tools, LD_PRELOAD]
---

After trying my hand at some OverTheWire challenges, I was reflecting on the solution to the 2nd Semtex challenge, where one needs to feed a binary the wrong result from `geteuid()`. I decided to spend an evening whipping up a little tool so that if I encounter a similar problem in the future, I can hopefully solve it much more easily.

Intercepting calls with `LD_PRELOAD` is pretty easy once you understand what's going on. The gist of it is that the `LD_PRELOAD` environment variable is checked for libraries to load **before anything else**. This makes it perfect for modifying the behaviour of existing system calls.

I wanted the project to make it possible to go from requirement to result as quickly as possible, and so overriding a system call is as "simple" as writing your replacement c function and passing it through EasyPreload. Options for stealth and persistence are available, though by default, the effects of EasyPreload last only so long as the terminal session in which the program was invoked.

As of writing this, the only module included is the one I used to solve the OverTheWire puzzle. I may expand it in the future, but on the off chance anyone finds this tool useful, they're more than welcome to contribute their modules.

Here is an example of its use:

<img src="/images/easypreload.png">

The project code is available on <a href="https://github.com/jsrn/EasyPreload" title="GitHub">GitHub</a>. I hope someone out there enjoys it.

**Recommended reading:**  
http://www.linuxforu.com/2011/08/lets-hook-a-library-function/  
http://fluxius.handgrep.se/2011/10/31/the-magic-of-ld_preload-for-userland-rootkits/