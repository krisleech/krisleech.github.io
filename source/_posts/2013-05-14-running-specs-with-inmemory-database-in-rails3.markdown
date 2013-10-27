---
layout: post
title: "Running specs using an in memory database in Rails 3"
date: 2013-05-14 21:01
comments: true
published: true
categories: [ruby]
---

This is a simple way to run specs using an in-memory SQLite3 database in Rails 3.

```yaml
test:
  adapter: sqlite3
  encoding: utf8
  database: <%= ENV['in_mem'] ? '":memory:"' : "db/test.sqlite3" %>
```

```ruby
ENV["RAILS_ENV"] ||= 'test'
require File.expand_path("../../config/environment", __FILE__)
 
load "#{Rails.root.to_s}/db/schema.rb" if ENV['in_mem']
 
require 'rspec/rails'
require 'rspec/autorun'
require 'capybara/rspec'
require 'capybara/rails'
require 'capybara/poltergeist'
```
 
```bash
env in_mem=1 rspec spec
```

After doing this the time to run our specs reduced, however a couple of specs are failing when using an in memory database. I'm not sure why yet, hence I'm using a ENV var to switch between file and memory SQLite database.
