---
layout: post 
title: "Teaching Ruby to children" 
date: 2013-07-28 21:01
comments: true 
published: true 
categories: [ruby] 
---

I've been playing with using PrinceXML to generate PDF's in my Rails
applications. PrinceXML takes HTML/CSS as input so there is no need to create
seperate HTML and PDF templates which the usual way with other libaries such as
`Prawn_to`. The only downside to PrinceXML is its commercially licensed and can
be quite expensive depending on your usage.

<!--more-->

That aside I used the princely library for Rails which wraps PrinceXML allowing
me to render PDF views with a few lines of code. The only problem was that it
did not work when using Passenger. Mongrel however was fine. The error I was
getting was:

```bash
Cannot find prince command-line app in $PATH
```

The problem as I eventually found out was that Passenger does not run in a
shell context like Mongrel so does not have the usual environment paths. My
solution is not battle tested more of a quick fix. I add the path to prince to
ENV['path'] in environment.rb, right at the bottom of the file.

```bash
shell ENV['PATH'] = ENV['PATH'] + ':/usr/local/bin'
```

Of course I will need to make the path configurable at some point to
accomerdate other setups.
