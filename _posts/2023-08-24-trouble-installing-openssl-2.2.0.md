---
layout: post
title: Trouble installing the openssl 2.2.0 gem
description: I was trying to install the openssl@2.2.0 gem today with Ruby 3.0.5 and received a very long build error.
category: programming
---

I was trying to install the `openssl@2.2.0` gem today with Ruby 3.0.5 and received a very long build error.

I have OpenSSL 1.1 and OpenSSL 3 installed via Homebrew, and it turned out to be some sort of compatibility issue with OpenSSL 3.

The workaround was to briefly uninstall OpenSSL 3, install the gem, and re-install OpenSSL 3.

```bash
# A bunch of installed packages depend on this,
# but we don't want to mess with them.
brew uninstall openssl@3 --ignore-dependencies

# Do what we've gotta do.
bundle install

# Put OpenSSL 3 back.
brew install openssl@3
```
