---
layout: post
title: "Configuring a global .gitignore"
description: Because I literally never want to commit .DS_Store
category: programming
tags: [git]
permalink: /configuring-global-gitignore
updated: Jul 16, 2022
---

There are some files that you just pretty much never want to commit to version control. Depending on your needs, this might be something like your `node_modules` folder or it could be the `.DS_Store` file created by MacOS that holds information about the containing folder.

You can configure git to take advantage of a global `.gitignore` file for these cases.

1. Create your global ignore file somewhere on your computer. The name and location aren't actually important, but I call mine `.gitignore_global` and keep it in my home directory.
2. Configure your **global** git configuration to read it with the following command.

```bash
git config --global core.excludesfile ~/.gitignore_globals
```

You should now find that git will ignore any files mentioned in your global gitignore file as well as your project specific ones.

This is good for you, but it's also good for the whole team. With each developer responsible for ignoring their own OS specific files or IDE preferences, the project `.gitignore` can be used to focus exclusively on project specific artefacts.