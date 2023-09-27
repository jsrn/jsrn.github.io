---
layout: post
title: Dealing with JSON log output
description: Some handy tools for handling JSON output on the command line.
category: programming
permalink: /dealing-with-json-log-output
---

I was trying to follow the output of a service running on my machine. Each log line was a JSON string. The nesting was shallow, but there were a large number of key/value pairs in each log line, and it was pretty hard to find the values I was looking for in the sea of white text.

In want of a better way to handle JSON output on the command line, my own research initially turned up [bat](https://github.com/sharkdp/bat). After I shared that in the `#dev_tip_of_the_day` channel on Slack, several of my colleagues chimed in with more excellent suggestions.

- [bat](https://github.com/sharkdp/bat)
  - Pros: Extremely flexible. Handles many more formats than just JSON.
  - Cons: Finding a theme that highlighted JSON the way I wanted it to was a little tough, as many themes displayed object keys and string values in the same colour.
- [jq](https://jqlang.github.io/jq/)
  - Pros: Super powerful. Lets you do cool stuff like displaying and searching at the same time.
  - Cons: Just JSON. Very arguably not a flaw, if you're an adherent of the Unix philosophy.
- [jless](https://jless.io/)
  - Pros: The coolest of the bunch. Lets you interactively whip through JSON data. Very cute mascot.
  - Cons: Not well suited to streaming data, e.g. following log output.
- `python -m json.tool`
  - Pros: Pretty-prints output and doesn't require installing a third party tool (unless you count Python.)
  - Cons: Not well suited to streaming data, pretty-prints but doesn't add colour.

Of the bunch, I think `jless` will become my go-to. Such a cute mascot.
