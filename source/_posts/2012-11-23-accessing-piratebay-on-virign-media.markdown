---
layout: post
title: "Accessing Piratebay on Virgin Media"
date: 2012-11-23 21:01
comments: true
published: true
categories: [ruby]
---

Note: This is not a comment on the right/wrongness of pirating copyright material.

Virgin Media now blocks access to Piratebay, but if you have a server (e.g Rackspace/EC2 etc.) then you can create a SSH tunnel to that server and use it to proxy HTTP.

```
ssh -D 5222 example.com -N -f
```

Just replace example.com with the domain of your server and it will create an SSH connection which proxies all traffic on localhost:5222 to your server.

You then just need to tell your web to use localhost:5222 as its SOCKS proxy, do not set the HTTP Proxy.

Note: You do not need to proxy your Torrent traffic as that is Peer to Peer so you connect to other users who have the file you are downloading, not a central server.
