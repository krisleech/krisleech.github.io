---
layout: post
title: "Search in history"
date: 2012-04-19 21:01
comments: true
published: true
categories: [shell]
---

In zsh you can type the first letter or two of a previously executed command and press the up arrow to scroll through the history, this is equivalent to an SQL query of "LIKE lipsum%". This works fine if you know the first few letters of the target command are known and unique enough not to have to scroll through lots of hits.

<!--more-->

If I want to search more generally, an SQL query such "LIKE %lipsum%", then I do history | grep lipsum.

But there is a better way I learned of recently, press Ctrl+R and start typing, then use the arrow keys to scroll through matches. 

This works much better in many cases, for example I can press Ctrl+R and type "pull" to scroll through only the "git pull" commands in history, not all the "git" commands. 
