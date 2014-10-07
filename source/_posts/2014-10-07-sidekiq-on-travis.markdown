---
layout: post
title: "Sidekiq on Travis"
date: 2014-10-07 09:36
comments: true
published: true
categories: [ruby, rails]
---

If you need Sidekiq on Travis CI then you only need to do two things, start
Redis and Sidekiq. Travis has build-in support for installing and starting
Redis. You must add your own script to start Sidekiq.

An example `.travis.yml`

```
language: ruby
before_script:
  - bundle exec sidekiq -d -r ./spec/dummy_app/app.rb -L /tmp/sidekiq.log
  - sleep 1
services:
  - redis-server
rvm:
  - jruby-19mode
  - rbx-2
  - 2.0.0
  - 2.1
  - 2.2
```

If you target JRuby then you can't use `-d` (detach) option to start
sidekiq since JRuby does not support forking. Instead you can run the command
in the background with `&`, i.e.  `bundle exec sidekiq -r ./spec/dummy_app/app.rb -L /tmp/sidekiq.log &`.
