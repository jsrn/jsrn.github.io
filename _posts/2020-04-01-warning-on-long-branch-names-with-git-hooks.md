---
layout: post
title: "Warning on long branch names with git hooks"
description: "Long branch names were causing problems in our CI. Here's how to head them off at the pass."
category: programming
tags: [git]
---

My company does some cool stuff with our CI pipeline. When someone submits a merge request, we know that this branch may require some manual testing before it gets merged. We enable this with a button on the merge request that lets people spin up a docker container running the proposed change, whacking it on our kubernetes cluster, and pointing a URL to it.

If your branch is called `new-feature`, you'll get a shiny copy of the app running just for you at `https://new-feature.review.example.com`.

If your branch is called `super-amazing-cool-new-feature-that-i-love-and-you-will-too`, you would expect to be able to test your feature at `https://super-amazing-cool-new-feature-that-i-love-and-you-will-too.review.example.com`

So, we hit the button. Things begin to churn. We ping off a request to AWS Certificate Manager and... it tell us off.

Oh.

*Apparently*, the first domain name on a certificate can't exceed 64 octets in length (including periods). Ours is... somewhat longer than that.

I wish someone had told me that AWS was going to reject my certificate request *before* I created the branch and wrote the code and created the merge request and waited for the automated test suite to run and waited for the docker container to be built.

Enter [git hooks](https://githooks.com/).

git hooks let us execute a script before or after certain git actions. You can do lots of awesome stuff with them, like blocking commits that don't match certain criteria, triggering deploys in different environments, and more.

In this case, we want to warn ourselves whenever we create a branch name that is going to result in a domain name that won't pass AWS Certificate Manager's checks.

We know that the base of our URL is `.review.example.com`, which is 19 characters long. This gives us (assuming that all of our branch names are plain ol' UTF-8) 45 characters to play with.

Let's drop the following into `.git/hooks/post-checkout` and make it executable:

```bash
#!/bin/sh
RED='\033[0;31m'
NC='\033[0m'
branchname=$(git status | cut -d ' ' -f 3 | head -1)
if [ ${#branchname} -gt 45 ]
then
  printf "${RED}💩 This branch name is over 45 characters.\n"
  printf "If you proceed, you will be unable to generate a TLS certificate for your review app.${NC}\n"
fi
```

Now when we try to create a branch with a luxuriously long name, we get:

```
➜  hooks git:(master) git checkout -b super-amazing-cool-new-feature-that-i-love-and-you-will-too
Switched to a new branch 'super-amazing-cool-new-feature-that-i-love-and-you-will-too'
💩 This branch name is over 45 characters.
If you proceed, you will be unable to generate a TLS certificate for your review app.
➜  hooks git:(super-amazing-cool-new-feature-that-i-love-and-you-will-too)
```

Sadly there doesn’t seem to be any pre-checkout hook, so you can’t block a branch from being created (or confirm that it’s really what you want), you can only spit out a warning after the fact. But hey, it’s red!