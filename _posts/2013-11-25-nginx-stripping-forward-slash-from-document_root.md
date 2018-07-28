---
layout: post
title: "nginx stripping forward slash from DOCUMENT_ROOT"
description: ""
category: misc
tags: [programming, nginx, document root, DOCUMENT_ROOT]
---

Today, I had to deploy some code onto an nginx server. I like nginx because while it's not as easy to use straight out of the box as apache, it's not too hard to configure and it's very fast.

I set up my virtual host:

	server {
		server_name sub.domain.net;
		...
		root /var/www/sub.domain.net/public/;
		...
	}

... and I checked the site. Server error?!

	Failed opening required '/var/www/sub.domain.net/public../app/...

I know I put the trailing slash in the document root, so why is nginx stripping it?

The solution is in `/etc/nginx/fastcgi_params`. Just change:

	fastcgi_param	DOCUMENT_ROOT		$document_root;

into

	fastcgi_param	DOCUMENT_ROOT		$document_root/;

and restart nginx. It feels like a bit of a hack, but you now have the trailing slash you need.

<a href="http://forum.nginx.org/read.php?2,220811,220811#msg-220811">Source</a>