---
layout: post
title: "Vim tips"
date: 2013-07-28 21:01
comments: true
published: true
categories: [vim]
---

A collection of vim tips

<!-- more -->

Format visual selection to 80 characters

`gq`

Convert tabs to spaces

`:retab`

Substitute string with a period/dot in it

I wanted to replaced all instances of "f." with "sf.", my first stab was `:%s/f./sf./ggg`

However this made a right mess of the text, a quick chat with jamessan& on
freenode#vim reveraled that the first parameter is a regex, and a period in a
regex means any character, oops. The answer he pointed out is to escape the
period with a backslash, so `:%s/f\./sf./ggg`

& Gender assumed based on username containing a male first name.
