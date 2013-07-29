---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

I'm using Jeweler to package my Ruby libraries/plugins as Gems. Jeweler uses a file called VERSION which contains the current version for the gem. It is also common practice to have a constant called VERSION in the plugin's root module which also returns the current version. This leads to duplication and the tendancy to forget to update one of the two versions. 

Here is how to keep your version numbers DRY (Dont Repeat Yourself) by reading directly from the VERSION file.

```
module Qcms
  VERSION = File.read(File.join(File.dirname(__FILE__), '..', 'VERSION'))
end
```

A little bit of DRY goes a long way!

UPDATE: It is recommended to keep the `VERSION` in a separate file, so in the gemspec you can require just that file to get the gem version without having to require the entire gem. 