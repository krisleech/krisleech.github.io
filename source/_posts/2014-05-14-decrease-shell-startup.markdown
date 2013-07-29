---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Increase shell startup time dramatically when using rbenv
04/02/2013

My shell was taking 2 seconds to start, after a bit of commenting out in .zshrc I narrowed it down to the `rbenv init -`. Some Google time later I found that rbenv rehashes all gem during the init process and this can be stopped by changing it to `rbenv init - --no-rehash`.

I also took the time to remove oh-my-zsh plugins I didn't use very often and change my theme to `miloshadzic` which does not display the Ruby version.

Now my shell starts "instantly".

