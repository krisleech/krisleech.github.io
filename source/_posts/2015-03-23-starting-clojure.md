---
layout: post
title: "Getting started with Clojure"
date: 2015-03-23 09:36
comments: true
published: true
categories: [clojure]
---

My first steps in learning Clojure and setting things up.

<!--more-->

## Get inspired

* [Making Games at Runtime with Clojure](https://www.youtube.com/watch?v=0GzzFeS5cMc)
* [Actors in Clojure](http://clojure.org/state#actors)
* [Clojure Resources](https://github.com/matthiasn/Clojure-Resources)

## Start with Leiningen

Since Clojure is a [Jar file](https://en.wikipedia.org/wiki/JAR_%28file_format%29) there is no Clojure binary to install. You just need a Java JVM and [Leiningen](http://leiningen.org/).

Leiningen is like Gemfile and Rakefile in Ruby, plus much more.

```
brew install leiningen
```

(or use your preferred package manager)

## Start a new project

```
lien project new playground
```

By default a command-line app is generated. You can get a different starting point by specifying a template, for example [Figwheel](https://github.com/bhauman/lein-figwheel) for ClojureScript, [Play](https://github.com/oakes/play-clj) for dekstop and mobile games, or [Luminus](http://www.luminusweb.net/) for websites.

A commandline app is packaged as an [uberjar](https://stackoverflow.com/questions/11947037/what-is-an-uber-jar), that is a jar file which includes all it's dependencies.

You can build the uberjar:

```
lein uberjar
```

and run it using `java -jar playground-0.1.0-standalone.jar`.

## REPL

The REPL (Read Eval Print Loop), like Irb in Ruby, can be started with `lein`:

```
lien repl
```

In the REPL we can requie and run our newly created app:

```
(require 'playground.core) ; require the app
(playground.core/-main)    ; run the app
```

In the source `playground.core` is a namespace and `-main` is a function. The `-main` function is used as an entry point for the uberjar, it is passed the commandline arguments as its arguments.

The REPL is the best place to start experimenting with Clojure syntax.

* [BraveClojure](http://www.braveclojure.com)
* [Clojure Doc Essentials](http://clojure-doc.org/articles/content.html#essentials)
* [Clojure Community Docs](http://conj.io/)
* [Clojure Distilled](https://yogthos.github.io/ClojureDistilled.html)

## Tests

This is just here for compleness, I don't intend to write any tests for quite a while.

```
lien test
```

## Run

```
lien run
```	

For more detailed information about Lein see the [leiningen tutorial](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#leiningen-projects).
