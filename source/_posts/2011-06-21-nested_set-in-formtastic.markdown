---
layout: post
title: "nested_set in a select"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

<!--more-->

```erb
<%= form.input :categories, :collection => website.categories.all.collect { |c| ["#{'  -- ' * c.depth}#{c.name}", c.id ] }, :as => :check_boxes %>
```

This produces a select with the entire tree in it.
