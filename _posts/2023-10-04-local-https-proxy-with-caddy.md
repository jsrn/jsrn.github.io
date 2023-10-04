---
layout: post
title: Local HTTPS proxy with Caddy
description: Here's how I set up an HTTPS proxy in front of a local service using a self-signed certificate and three lines of Caddy configuration.
category: programming
permalink: /local-https-proxy-with-caddy
---

Here's how I set up an HTTPS proxy in front of a local service using a self-signed certificate and three lines of Caddy configuration.

I thought this would be a giant hassle, but [Caddy](https://caddyserver.com/) made it easy.

The `/etc/hosts` entry:

```
127.0.0.1       whatever.local
```

The OpenSSL command:

```bash
openssl req  \
  -nodes -new -x509  \
  -keyout whatever.local.key \
  -out whatever.local.cert
```

The Caddyfile:

```
https://whatever.local {
  tls ./whatever.local.cert ./whatever.local.key
}
```

The Caddy command:

```bash
caddy run
```