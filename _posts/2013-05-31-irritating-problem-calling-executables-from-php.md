---
layout: post
title: "Irritating problem calling executables from PHP"
description: ""
category: programming
tags: [php, bug, absolute path]
---

While chucking together my LaTeX web front end, I ran into an interesting problem. In concept, the code is pretty simple.

	$texPath = "C:/Program Files (x86)/Miktex 2.9/miktex/bin/pdflatex.exe";
	$texFile = time() . '.tex';
	exec($texPath . ' ' . $texFile);

However, this results in a fatal error each time.

	'C:\Program' is not recognized as an internal or external command, operable program or batch file.

Hm. At first, I assumed it was because of the spaces in the path. In fact, it is, but the odd part is, escaping the spaces didn't seem to help. The same error occured.

In this case, pdflatex is already in my system path, so in fact, the use of an absolute path is unecessary. But what about the hypothetical situation where I have to run some non-standard software from PHP, yet do not have the ability to add it to the path? Should I be constrained to using a file structure with no spaces? (Yes, if you ask me, but that's a different matter.)

Short path names were the answer. A quick and easy way to find them, in this case:

	C:\> cd "C:\Program Files (x86)\MiKTeX 2.9\miktex\bin"
	C:\Program Files (x86)\MiKTeX 2.9\miktex\bin> for %I in (*) do @echo %~sI
		....
		C:\PROGRA~2\MIKTEX~1.9\miktex\bin\pdflatex.exe
		....

Problem solved, although I'm sure there's a better way.