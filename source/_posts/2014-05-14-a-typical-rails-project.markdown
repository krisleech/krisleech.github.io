---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

A Rails tool chain from development to production
30/03/2012

MacOS or Ubuntu
I like to stick to POSIX / GNU compatible operating systems.

Rails / Bundler / RVM
Rails 3 on Ruby 1.9.2 is standard, bundle makes life so much easier.

I have lots of projects on my hard drive which use a variety of Ruby versions so I use RVM to manage these.

I don't strictly need an RVM gemset any more since bundler can install gems to vendor, but I like to use gemsets anyway in most cases.

Foreman
Foreman is great for starting servers in development. I often have more than just an app server, for example Solr or some job queue, to start.

Foreman allows me to create a file in the root of my project with all the commands to start the project.

For legacy projects without foreman I have some aliases for opening the browser.

RSpec/Capybara/Factory Girl
If I have a choice I don't bother with cucumber just plain Capybara request specs.

Guard
Guard for continuous integration. I have guard open in its own window.

Tmux, Vim and Turbux
For actually coding I use vim on the terminal inside a tmux session, with turbux to run isolated tests when Guard gets too noisy.

	<iframe border="0" height="459" id="shelr_record_4f884bc296608003ba000001" scrolling="no" src="http://shelr.tv/records/4f884bc296608003ba000001/embed" style="border: 0" width="885"></iframe></p>


Chrome
It might seem odd to include this but I think it makes a difference.

I used Firefox for so many years, but nowdays it crashes too often and seems slower.

It could be the plugins and to be fair I should remove all but the essential ones.

Automate all the above
I use a shell script to automate starting of development environment including starting foreman, guard and vim, all inside tmux windows.

I even have it open a Chrome browser at http://localhost:8080

Unicorn
Until very recently I used mongrel for development and often passenger in production. But I'm really liking Unicorn, as late as I am to the party.

It seems quicker than mongrel and in production, like passenger, there is no need to manage a cluster of processes.

Git
What else?

I usually host the repo on Github.

Capistrano
I swayed to and from cap over the years. I have projects using vlad, custom shells scipts and even ( ! ) manual processes (ssh and git pull).

I started using capistrano again recently. Its kinda heavy weight and it take time to work out exactly how it works (I don't like to rely on magic), but its solid and has nice integrations for bundler, rvm and the asset pipeline.

Sprinkle
A bit like cap I've used many techniques for provisioning from by hand installs to shell scripts. I've tried on a few occasions to use chef solo and puppet solo with little joy. Sprinkle on the other hand is more accessible. I have comfortably provisied multi-server systems with it.

Pivotal tracker
I have a paid account and follow a scrum-like process.

Whats missing?
I don't use a formal documentation tool such as RDoc, TomDoc or YARD, I should.