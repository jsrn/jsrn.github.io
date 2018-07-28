---
layout: post
title: "Provisioning Dionaea Honeypot with Vagrant and Salt: A First Attempt"
description: ""
category: security
tags: [salt, dionaea, vagrant]
---

I've recently been talking to my amigos in Sloth Krew about setting up a couple honeypots and comparing field notes. There was a fair amount of discussion in IRC about deployment, and I figured it'd be a really cool opportunity to get to grips with some provisioning tools in a hobby capacity.

To start off with, I'm trying a combination of [Salt](http://saltstack.com/) and [Vagrant](https://www.vagrantup.com/). The idea is to use Vagrant to do just enough to enable salt to talk to it, and then have Salt configure a full instance of Dionaea honeypot, complete with all dependencies. When that's accomplished, I want to look at using these tools to deploy directly into THE CLOUD with my DEVOPS.

The dionaea installation instructions are [here](http://dionaea.carnivore.it/), but there's a [quick install guide](http://andrewmichaelsmith.com/2012/02/quick-install-of-dionaea-on-ubuntu/) for an ubuntu package maintained by honeynet. This makes installation a real breeze.

After spending a while getting to grips with Salt, the final combination of Vagrantfile and Salt state script turned out to be pretty simple.

The result of the evening's tinkering can be found [here](https://github.com/Slothkrew/HoneyDeploy/tree/master/dionaea), and will be built upon next time when I add VPS provisioning to really get this show on the road.

[gist](https://gist.github.com/jsrn/dfd1807dc270a8b2dab4) for posterity, since the files are likely to change a lot.