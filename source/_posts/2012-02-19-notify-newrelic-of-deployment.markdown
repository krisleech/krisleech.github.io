---
layout: post
title: "Notify NewRelic of deployment"
date: 2012-02-19 21:01
comments: true
published: true
categories: [ruby]
---

Notify NewRelic of a deployment from a Ruby script.

```ruby
# config(:branch) returns the branch being deployed.
# The newrelic command can accept an appname or appid, its undocumented, but it works.
# The appid can be found in the URL after logging in to NewRelic.

def notify_newrelic
  sha1 = (`git rev-parse #{config(:branch)}`).chomp
  description = (`git log #{config(:branch)} -1 --format="%s"`).chomp
  command = "bundle exec newrelic deployments --appname=#{config(:application_id)} --revision=#{sha1} '#{description}'"
  puts command
  system command
end
```
