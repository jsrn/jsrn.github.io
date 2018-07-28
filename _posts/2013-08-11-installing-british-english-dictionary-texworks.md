---
layout: post
title: "Installing the British English Dictionary in TeXworks"
description: ""
category: misc
tags: [spellcheck, latex, texworks, british english, en-GB]
---

TeXworks is great, but it's even better when you enable the in-built spellchecking. It was suggested that I post this step-by-step in the hopes that it helps anyone else trying to enable the en-GB dictionary for their spellchecker.

First, you'll need to find a `.dic` file and a `.aff` file for download. These can be found anywhere, but I swiped mine from <a href="http://dictionaries.mozdev.org/installation.html">Mozilla's Thunderbird Extensions library</a>. I've rehosted it <a href="/files/spell-en-GB.zip">here</a> for convenience.

If you choose to download from the Thunderbird Extensions library, follow these steps:

 1. Right-click on the British English link
 2. Save Link as
 3. Navigate to your downloads folder
 4. You will find a folder called spell-en-GB.xpi
 5. Change the file extension to .zip

Now that you have the zip file, locate your Texworks folder. It should be in `C:\Documents and Settings\Your Name\` by default. Within this folder, create a folder called "dictionaries" and extract `en-GB.dic` and `en-GB.aff` to it.

To enable the dictionary:

 1. Edit -> Preferences
 2. Set **Spell-check language** to British English

Now you have functional spellchecking inside your LaTeX editor.

 *Big thanks to A for the helpful tip.*