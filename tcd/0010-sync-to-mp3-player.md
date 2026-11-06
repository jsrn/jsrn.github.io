---
layout: tcd
id: "0010"
title: syncing to my mp3 player
authored: "2026-11-06"
updated:  "XXXX-XX-XX"
---

I currently use `robocopy` to copy files between my computer and my MP3 player. I have a Powershell script with the following command:

```ps
robocopy C:\Music F:\Music `
  /MIR <# sync dir recursively, remove deleted files #> `
  /FFT <# allow 2 second drift between file timestamps #> `
  /DST <# ignore exact 1 hour diff between file timestamps #> `
  /R:1 /W:1 <# retry failures once, after one second #> `
  /XJ <# skip junction points to avoid looping #> `
  /XD "*.tmp.*" /XF "*.tmp.*" <# exclude google drive temp files #>
```

