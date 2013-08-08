---
layout: post
title: "Rails Gotchas"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

A list of gotchas

<!--more-->

## Rails 4

Capistrano deploy fails with "There was an error in your Gemfile, and Bundler cannot continue."

However bundling locally is fine.

Even if you have `bundler` in your Gemfile it appears that the system version
is used. I had an older 1.1 rc version installed in my system Ruby. Therefore
simply ssh in to the server and update to the latest version:

```bash
sudo gem install bundler
```


