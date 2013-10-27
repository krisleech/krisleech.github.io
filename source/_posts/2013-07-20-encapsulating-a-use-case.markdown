---
layout: post
title: "Encapsulating a use case"
date: 2013-07-20 21:01
comments: true
published: false
categories: [ruby]
---

In this post I am going to describe a technique I have been using recently to
encapsulate a multi-step use case.

I haven't fully groked DCI, but this strikes me as quite similar, at least on
the surface.

<!--more-->

## Background

```ruby
class DeliverPackage
  def initiate(user)
  end

  def cancel(order)
  end

  def start(order)
  end
end
```
