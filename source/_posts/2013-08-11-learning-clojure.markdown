---
layout: post
title: "Learning Clojure"
date: 2013-08-11 21:01
comments: true
published: true
categories: [ruby, clojure]
---

Some notes on learing Clojure

<!--more-->

http://jrheard.tumblr.com/post/40024238467/getting-started-with-clojure

* Clojure -> Ruby
* Leiningen -> Bundler, IRB
* Rubygem -> JAR
* Rubygems.org -> ?
* Ruby Toolbox -> ?

Side by side Ruby against Clojure.

```ruby
require 'http'
```

```
(require '[clj-http.client :as client])')
```

```ruby
HTTPClient.get('http://yelp.com')
```

```
(client/get "http://www.yelp.com")
```

```ruby
puts HTTPClient.get('http://google.com').body[0..49].join
```

```
(println (apply str (take 50 (:body (client/get "http://google.com")))))
```
