---
layout: post
title: "29c3ctf - Web300 / MEGAIMAGE Writeup"
description: ""
category: security
tags: [29c3ctf, ctf]
---


*This post is mirrored from my old blog.*

This was a fun one. The challenge site provided a file upload control, which allowed us to upload images to the site. Upon uploading an image, the image would be displayed under a new file name, along with the associated Image Description, GPS Longitude and GPS Latitude tags.

My first thought was to insert some PHP into the exif data, so I downloaded the very useful <a href="http://www.sno.phy.queensu.ca/~phil/exiftool/">ExifTool</a> to attempt this.

This attempt failed, so I pulled out the next trick in the bag. I threw an apostrophe into the Image Description tag and uploaded the file. Internal Server Error! SQL injection? I think so!

With some tinkering, I managed to successfully exploit the SQLi to get the flag.

<img src="/images/web300-1.png">

<img src="/images/web300-2.png">