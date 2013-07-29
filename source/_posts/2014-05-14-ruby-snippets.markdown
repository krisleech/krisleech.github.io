---
layout: post
title: "Map Reduce"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Map Reduce
This is based on an example I saw while watching the Aaron Patterson Play by Play from Peepcode. The syntax is really nice.


1
2
3
4
5
6
7
records = [ 
            { :id => 1, :score => 1 }, 
            { :id => 2, :score => 4 }, 
            { :id => 3, :score => 9 } 
          ]
 
records.map { |record| record[:score] }.reduce(:+) # => 14
view rawmap_reduce.rbThis Gist brought to you by GitHub.
This will work will an collection such as an Array of ActiveRecord objects.

2012-03-30