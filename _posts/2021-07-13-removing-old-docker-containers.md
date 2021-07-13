---
layout: post
title: "Removing Old Docker Containers"
description: Removing my archive of unused docker containers.
category: programming
tags: [docker]
---

```
$ docker container list -a
```

I noticed I had a large number of docker containers that weren't being used. Fortunately docker makes it easy to single out a container from this list and remove all containers that were created before it.

```
docker ps -aq -f "before=widgets_web_run_32cb6f5d2d71" | xargs docker container rm
```

There are a bunch of [handy filters](https://docs.docker.com/engine/reference/commandline/ps/#filtering) that can be applied to `docker ps`. Nice for a bit of spring cleaning.
