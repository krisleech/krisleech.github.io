---
layout: post
title: "Map Reduce"
date: 2012-03-30 21:01
comments: true
published: true
categories: [ruby]
---

This is based on an example I saw while watching the Aaron Patterson Play by Play from Peepcode. The syntax is really nice.

```ruby
records = [ 
            { :id => 1, :score => 1 }, 
            { :id => 2, :score => 4 }, 
            { :id => 3, :score => 9 } 
          ]
 
records.map { |record| record[:score] }.reduce(:+) # => 14
```

This will work will an collection such as an Array of ActiveRecord objects.
