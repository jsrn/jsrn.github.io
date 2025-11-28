---
layout: tcd
id: "0006"
title: Ripgrep cheat sheet
authored: "2025-11-27"
updated:  "XXXX-XX-XX"
---

Finds occurrences of SEARCHTERM in data.log and gives you the ten characters on
either side. Like grep's `-B` and `-A` but for characters instead of lines.

```bash
rg -oN '.{0,10}SEARCHTERM.{0,10}' data.log
```

-o, --only-matching

-N, --no-line-number
