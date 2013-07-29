---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Running specs using an in memory database in Rails 3
29/01/2013

This is a simple way to run specs using an in-memory SQLite3 database in Rails 3.

1
2
3
4
test:
  adapter: sqlite3
  encoding: utf8
  database: <%= ENV['in_mem'] ? '":memory:"' : "db/test.sqlite3" %>
view raw01-database.ymlThis Gist brought to you by GitHub.
1
2
3
4
5
6
7
8
9
10
11
12
ENV["RAILS_ENV"] ||= 'test'
require File.expand_path("../../config/environment", __FILE__)
 
load "#{Rails.root.to_s}/db/schema.rb" if ENV['in_mem']
 
require 'rspec/rails'
require 'rspec/autorun'
require 'capybara/rspec'
require 'capybara/rails'
require 'capybara/poltergeist'
 
# etc ...
view raw02-spec_helper.rbThis Gist brought to you by GitHub.
1
env in_mem=1 rspec spec
view raw04-run-it.shThis Gist brought to you by GitHub.
After doing this the time to run our specs reduced, however a couple of specs are failing when using an in memory database. I'm not sure why yet, hence I'm using a ENV var to switch between file and memory SQLite database.

