---
layout: post
title: "Protecting non-HTTPS connections on a public network"
date: 2012-11-11 21:01
comments: true
published: true
categories: [ssh, shell]
---

If you are using a public network, e.g a cafe, then you can listen to everyones HTTP traffic quite easily and snoop passwords and usernames from logins to non-SSL websites. Just fire up Wireshark in promiscuous mode to find out.

<!--more-->

To protect from this tunnel all HTTP traffic through an SSH connection to some machine on a secure network. The machine on a secure network will typically be a computer on your home/office network or a hosted server. The machine to which you are tunneling all your traffic will need a SSH server running. If you are using a machine on your home/office network you will also need to set your router to forward port 22 to the machine with the SSH server running. You will also need to know the public IP of the router or setup a Dynamic DNS service. For this reason it is typically easier to use a cheap hosted virtual server.

```
ssh -D 5222 interkonect.com -N -f
```

The above will create an SSH connection which will tunnel (i.e proxy) all traffic to the given domain, in my case interkonect.com.

You then just need to tell your browser to use localhost:5222 as its proxy.

To confirm its working for sure; from the machine to which you are tunneling use ngrep to watch the traffic for the name of the website you then visit using the browser:

sudo ngrep | grep rubyflow.com

This same technique can be used to secure any plain text TCP protocol, e.g IMAP or IRC, when on an untrusted network.

Added bonus: Because you are proxying traffic via a server which does not use a domestic ISP it also allows you to access website, such as piratebay, which are blocked by your home ISP, e.g Virgin Media.

Command parameters (see man ssh for full details)

-D = local port to forward

-N = no command to run, i.e just create a tunnel (without this you will get an interactive prompt)

-f  =background the session, so you can close the terminal if required 
