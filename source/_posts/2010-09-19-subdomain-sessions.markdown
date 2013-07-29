---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

The configuration for setting the session domain between different Rails 2 versions has changed many times, below are some examples from Google:

<!--more-->

```ruby
ActionController::Base.session_options[:domain] = '.domain.com'

ActionController::CgiRequest::DEFAULT_SESSION_OPTIONS.update( :session_domain => '.domain.com')
```

After a bit of guess work the correct format for Rails 2.2.2 is:

```ruby
ActionController::Base.session_options[:session_domain] = '.domain.com'
```

Hope this helps anyone having to add sub-domain's to a Rails 2.2.2 app.
