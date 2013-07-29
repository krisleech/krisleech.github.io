---
layout: post
title: "Sendmail/Exim error Mailing to remote domains not supported"
date: 2010-02-24 19:22
comments: true
categories: [linux]
---
If your having problems sending email using sendmail (actually Exim) on Debian
based systems, in my case Ubuntu, then the first port of all call is to check
the files in `/var/mail` for any error messages.

In my instance I was getting `Mailing to remote domains not supported`.

<!--more-->

This is because Exim is by default configured to only send mail locally. You
need to allow Exim to send email to other servers. However do not allow Exim to
relay email as this will allow spammers to use your server to send email.

The easiest way to configure Exim is to use "dpkg-reconfigure". This will guide
you through the configuration in a Q&A style.

```
sudo dpkg-reconfigure exim4-config
```

Answer "internet site; mail is sent and received directly using SMTP" to the
first question and then choose a minimal configuration for the rest.

The questions are fairly self-explanatory, [here is a list](http://pkg-exim4.alioth.debian.org/README/README.Debian.html#id280581).

Remember not to allow relaying of email.

It would also be wise to backup `/etc/exim` first so you can always restore your
current settings, or better yet setup version control using Git.
