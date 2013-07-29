---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Automate tmux and get to coding quicker
30/03/2012

Here is an example of how to automate startup of your development environment with tmux. I use Ruby/Rails, Rspec and Vim so this shows their use, but it would equally apply to any terminal based setup.

 

Update: I wrote a rubygem to automate the creating of tmux configurations like the one below: tmuxinator. 

Create a function which starts a tmux session, sends key strokes to create windows and run commands, then attach to the session. I put this file in ~/bin/zsh.

 

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
13
14
15
16
17
18
19
20
21
#!/bin/sh
 
function matripa
{
  BASE="$HOME/code/matripa"
  cd $BASE
 
  tmux start-server
  tmux new-session -d -s matripa -n vim
  tmux new-window -t matripa:1 -n tests
  tmux new-window -t matripa:2 -n servers
  tmux new-window -t matripa:3 -n logs
 
  tmux send-keys -t matripa:0 "cd $BASE; vim" C-m
  tmux send-keys -t matripa:1 "cd $BASE; guard -c" C-m
  tmux send-keys -t matripa:2 "cd $BASE; foreman start" C-m
  tmux send-keys -t matripa:3 "cd $BASE; tail -f log/*.log" C-m
 
  tmux select-window -t matripa:0
  tmux attach-session -t matripa
}
view rawmatripaThis Gist brought to you by GitHub.
Source the function so its always available and has autocomplete (tested with zsh).

1
2
3
# ~/.zshrc
 
source ~/bin/zsh/matripa
view rawzshrcThis Gist brought to you by GitHub.
Now start a new terminal and type matripa. Or even ma and press tab.

This works well with RVM if you have a .rvmrc file in your project root.

<iframe border="0" height="524" id="shelr_record_4f8691a69660807979000003" scrolling="no" src="http://shelr.tv/records/4f8691a69660807979000003/embed" style="border: 0" width="654"></iframe>

Note that this has been tested in zsh, not bash. However I would expect, apart from the autocomplete, it would work just fine.

Alternative options: Tmuxinator and Teamocil, of the two I prefer Teamocil. Update: And now I must include my own Tmuxinator.