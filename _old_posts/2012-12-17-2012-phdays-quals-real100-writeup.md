---
layout: post
title: "2012 PHDays Quals: REAL100 Writeup"
description: ""
category: security
tags: [phdays, phdays quals, ctf]
---

*This post is mirrored from my old blog.*

The task was to retrieve the flag from a website. The website was built on an open source framework, and so included a link to the source, hosted on GitHub. There wasn't much to it, apart from the comments sections of news articles, and so it was pretty easy to find some vulnerable code:

{% highlight php %}
<?php
$sql = "INSERT INTO `comments` SET `news_id` = " . (int)$id .
",`username` = " . $this->db->quote($data['username']). 
",`text` = " . $this->db->quote($data['text']). 
",`date_posted` = NOW(), `ip` = INET_ATON('" . $data['ip'] . "')";  
?>
{% endhighlight %}

Sweet. Looks like if we can get our payload into `$data['ip']` we can inject into the query. Luckily, elsewhere in the code, we're shown that if `HTTP_X_FORWARDED_FOR` is set, that is the IP address that is used. I made a test comment, and sent the request on over to Burp Repeater.

We can inject by changing the X-Forwarded-For header.

We can start by using error based injection to find the right table and column. This is the query we get that allows the comment to be posted, rather than spitting out a table/column not found error:

	X-Forwarded-For: 127.0.0.1')+(SELECT flag from flags');--

From here on out, we're going blind. We have to build the flag, letter by letter, going through 0-9a-f until the query fails, and then go back one letter. By this repeated process, we can build the entire 32 character flag:

	X-Forwarded-For: 127.0.0.1')+(SELECT flag from flags WHERE flag &gt; '94bd6136818878b5dd97d3a231a97649');--
