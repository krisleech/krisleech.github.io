---
layout: post
title: "Coding 'home away from home' on MacOS"
date: 2013-01-02 21:01
comments: true
published: true
categories: [ruby]
---

I code using the tmux and vim in the shell.

Therefore when away from the office I can SSH back in to my dev box and code
like I'm at home.

This also works really if you put your coding environment on a VPS.

Setup SSH Turn on sshd, System Preferences > Sharing > Remote Login

Add your public key to `~/.ssh/authorized_keys` (create the file if it does not
exist).

Since your are exposing your Mac to the big bad world make sure SSH is secure:

```bash
# /etc/sshd_config PermitRootLogin on PasswordAuthentication no
```

Port forwarding 

Have your router forward port 22 to the IP (or hostname) of your Mac.

If you use an IP also make sure the DHCP in your router is configured to assign
the same IP for your Mac every time (the router will associate the Mac's MAC
address with the IP).

Dynamic DNS service Signup to a Dynamic DNS service (around &pound;17 pa) so you can
use a domain name instead of a (changing) IP address. You'll need to install a
small app which reports your IP to the service, which in turns updates the DNS
record for your chosen domain, e.g example.getmyip.com.

WakeOnLAN When your Mac goes to sleep it will not be able to accept network
connections. To wake up your Mac you can send it magic packet on any open port
(22 in our case) which will wake it up.

There is a nice WakeOnLAN in the AppStore, you will need the Mac's MAC address
and its IP. The MAC address is used as authentication.

You can find the MAC Address in System Preferences > Network (click Wi-Fi or
Ethernet) > Advanced > Hardware.

Try it out...  Put your Mac to sleep, then on another machine, send the
WakeOnLAN packet and SSH in.

```bash
ssh kris@example.getmyip.com
```

Bonus points...  Forward another port, e.g 8080 and start your web server on it
(`rails s -p 8080`).

Check out mosh for a more resilient ssh connection.

Make sure only ports 22 and 8080 are open:

```bash
nmap example.getmyip.com -Pn
```
