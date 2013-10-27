---
layout: post
title: "Switching git branches"
date: 2013-07-28 21:01
comments: true
published: true
categories: [git]
---

When switching branches make sure to restart the application server to ensure that the environment is reloaded. Branches may have different gem dependencies and configurations which can lead to frustration.

<!--more-->

```bash
$ git checkout feature_65

$ rake test:integration

19 tests, 13 assertions, 1 failures, 0 errors

1) Failure: uninitialized constant Project::Grit
```

environment.rb

```ruby
config.gem 'grit'
```

Yep its including the Grit gem!

```ruby
10.times do
  puts "Frustrated!"
  sleep(1)
end 
```

```bash
$ touch tmp/restart.txt

$ rake test:integration

19 tests, 13 assertions, 0 failures, 0 errors
```
