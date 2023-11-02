---
layout: post
title:  "Configuring Apt to use a HTTP proxy without proxying HTTPS"
date:   2015-05-26 12:09:35 +1100
categories: ubuntu debian apt proxy
---

This may seem like an odd and extremely niche thing to post about, but I could not find this information ANYWHERE when I was looking, so I wanted to share it.

Have you ever wanted to configure the Ubuntu/Debian Apt system to use an HTTP proxy, but NOT an HTTPS proxy? It's harder than it sounds, because Apt will automatically copy your Acquire::HTTP::Proxy setting over to the Acquire::HTTPS::Proxy setting if it's unset.

I discovered that Apt does not consider an empty string to be a setting, so you cannot override the HTTPS option with it.

However after much grief, I discovered that a string containing whitespace does get counted as a valid setting, and also, doesn't get used as an invalid proxy setting. Phew!

So to sum up, this is the configuration you want in /etc/apt.conf.c/proxy

```
Acquire::HTTP::Proxy "http://my.proxy.com:3142";
Acquire::HTTPS::Proxy " ";
```

Note the space between quote marks!
