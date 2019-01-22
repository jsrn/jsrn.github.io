---
layout: post
title: "2012 PHDays Quals: MISC100 Writeup"
description: ""
category: security
tags: [phdays, phdays quals, ctf]
---

*This post is mirrored from my old blog.*

This was an odd one. We were given a binary, and two hints: "limbo" and "inferno". After a quick google, we find out that "limbo" is a programming language intended to be run on the "inferno" operating system. Inferno can either be run as a stand-alone OS, usually on embedded systems, or as an application within a parent OS. I elected the latter, and before long had a working install of Inferno.

The binary still would not execute though. Some more googling lead me to understand that the usual extension for compiled limbo applications is ".dis". After adding that extension to the binary, we can finally execute it, and get the output:

	98f6bcd 4621d373 -3521b17d 2627b4f6

This is... almost an md5 hash! The first quarter is only 7 characters, so we pad it with a 0 to get the full 32.

	098f6bcd 4621d373 -3521b17d 2627b4f6

Something is odd about the third, but by googling the first, we quickly discover that it is the beginning of the md5 of the word 'test'. However, the third quadrant is wrong, so we swap it out for the real one.

	98f6bcd 4621d373 +cade4e83 2627b4f6
	98f6bcd4621d373cade4e832627b4f6

Which is the md5 of 'test' and the correct flag for the challenge.