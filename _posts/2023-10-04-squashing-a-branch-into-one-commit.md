---
layout: post
title: Squashing a branch into one commit
description: When you want to tidy your commit history, but can't be bothered reminding yourself how rebase works.
category: programming
permalink: /squashing-a-branch-into-one-commit
---

Assuming you're currently on the `feature` branch:

```bash
git checkout -b temp main
git merge --squash feature
git commit
git checkout feature
git reset --hard temp
git branch -d temp
```

Source: [StackOverflow](https://stackoverflow.com/a/69827502)
