---
layout: post
title: "Installing ZNC to catch up with IRC"
date: 2014-10-23 09:36
comments: true
published: true
categories: [linux]
---

[ZNC](http://wiki.znc.in/ZNC) is is typically installed on a Virtual Machine and used as a proxy to a public IRC server. The advantage being that since it is permanently connected it can record all messages and play them back to your client when it connects.

![http://wiki.znc.in/images/4/4f/Overview_network_scheme.png](http://wiki.znc.in/images/4/4f/Overview_network_scheme.png)

<!--more-->

## Installation

On a Debian/Ubuntu Virtual Machine you can use `apt-get` to install the znc package:

```
sudo apt-get update
sudo apt-get install znc
```

Create a new linux user for ZNC, should ZNC ever become exploitable at
least it will be limited to a single user on the server.

```
sudo adduser znc-admin
su znc-admin; cd ~
znc --makeconf
```



ZNC will ask you a load of questions and create `~/.znc/configs/znc.conf`. Mostly you can use the default answers by pressing enter.

```
What port would you like ZNC to listen on? (1025 to 65535):
```

This is the port that you will connect your IRC client. The same port is used for the web admin UI.

```
Would you like ZNC to listen using SSL? (yes/no) [no]: no
```

You will be asked if you want to use SSL, if you are already running a web
server you will need to set this to no if the web server listening on the port 443.

The rest of questions will ask about installing modules, install them all.

Now you will create an IRC user. This is the username/password you will end up putting
in to your IRC client.

```
Username (AlphaNumeric): znc-admin
Enter Password:
Would you like this user to be an admin? (yes/no) [yes]: yes
```

Now set your IRC nickname that ZNC will use when connecting to a
channel, I like to use my regular nick.

```
Nick [FirstUser]: your.name
```

Accept the defaults and opt-in to installing all the admin modules.

Now add the public IRC server and channel you wish to proxy too, e.g. irc.freenode.net.

```
Would you like to set up a network? (yes/no) [no]: yes
Network (e.g. 'freenode' or 'efnet'): freenode
IRC server (host only): irc.freenode.net
Does this server use SSL? (yes/no) [no]: no
Would you like to add another server for this IRC network? (yes/no) [no]: no
Would you like to add a channel for ZNC to automatically join? (yes/no) [yes]: yes
Channel name: #microrb
Would you like to set up another network? (yes/no) [no]: no
Would you like to set up another user? (yes/no) [no]: no
```

And start ZNC:

```
Launch ZNC now? (yes/no) [yes]: yes
```

You should now find that port 1025 is open, you can check using `nmap`:

```
nmap -v -A example.com
```

Point your browser at `http://example.com:1025` to access to the web admin UI,
you will need the IRC username/password you setup earlier to login.

Finally grab your favourite IRC client and connection to `example:5000` with
the username and password you set for the IRC user. You will be able to join
new channels and they will be automatically added to ZNC's list of channels.

Now enjoy being able to see messages from people in different timezones!
