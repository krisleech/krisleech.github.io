---
layout: post
title: "String#split takes a second argument"
date: 2013-10-27 21:01
comments: true
published: true
categories: [ruby, mini]
---

Reading [Rebuilding Rails](https://rebuilding-rails.com) I noticed `split` can be used as such:

```ruby
'1/2/3/4/5/6/7/8'.split('/', 3)
# => ['1', '2', '3/4/5/6/7/8']
```
