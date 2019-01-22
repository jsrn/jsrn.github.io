---
layout: post
title: "29c3ctf - Web100 / regexdb Writeup"
description: ""
category: security
tags: [29c3ctf, ctf]
---

*This post is mirrored from my old blog.*

This challenge required the extraction of the key from the database, using a regex as a search. Knowing from IRC that all keys contained either 29C3 or 29c3, it was relatively simple to narrow down the potential keys to the correct one:

	/^Key: 29C3_Well\.This\/Is\#Not\+The\|Wrong\?Key$/

The final correct regex is slightly misleading, in that it says it matches 2 records, but we strip out the extra bits, and submit it for 100 points:

	29C3_Well.This/Is#Not+The|Wrong?Key