---
layout: post
title: "Vim, Tmux, Rspec on MacOS"
date: 2011-10-01 21:01
comments: true
published: true
categories: [ruby]
---

My current programming setup is the use of Vim, Tmux and Rspec on MacOS.

<!--more-->

Launchbar

Apple + Space, "it" + Enter to open ITerm2

Apple + Space, "fi" + Enter to open Firefox

ShiftIt

Alt + Apple + Ctrl + Left Arrow Key to make Iterm fill the left half the screen

Alt + Apple + Ctrl + Right Arrow Key to make Firefox fill the right half the screen

iTerm2

$ gitx

$ tmux

Ctrl + a (tmux prefix) then c(reate)

I then open 4 tmux windows, one for vim, one for the server(s), rspec and one for tail or other commands.

$ vim

$ rails s

EDIT: I now automate tmux

I then go about coding.

Using vim-turbux I can run a Spec in one of Tmux splits. I often use JRuby which can be a little slow to warmup so often have the specs running in a seperate window instead of a split.
