---
layout: post
title: "Setting up an interactive ClojureScript project"
date: 2015-03-10 09:36
comments: true
published: true
categories: [clojure clojurescript]
---

This is a really short guide to get an interactive development environment
where ClojureScript code is automatically reloaded in the browser and a command
line REPL which evaluates in the browser.

<!--more-->

## Install Clojure

The easiest way to get [Clojure](http://clojure.org/) is to install
[Leiningen](http://leiningen.org) which is a project automation tool for
Clojure. It is somewhat like a combination of Bundler and Rake in Ruby. Among
other things it allows you to:

* Create new projects
* Manage dependencies
* Run tests
* Start a REPL
* Run the project
* Publish libraries to Clojars
* Run tasks

```
brew install leiningen
```

Substitute the above for your prefered package manager if you are not using
`brew`.

## Create a new project

We are going to use a Lein plugin called
[Figwheel](https://github.com/bhauman/lein-figwheel) which gives us a starting
point for a ClojureScript application which hot-loads your code in the browser
as you make changes. FigWheel includes:

* Live Code Reloading
* Static File Server
* Live CSS Reloading
* A ClojureScript REPL

I'm going to use the [Om
tutorial](https://github.com/omcljs/om/wiki/Basic-Tutorial) as a starting
point. [Om](https://github.com/omcljs/om) is an interface to
[React](http://facebook.github.io/react/) in ClojureScript.

```
lein new figwheel om-tut -- --om
cd om-tut
```

Start the FigWheel static server / autobuilder:

```
lein figwheel 
```

The first time the project is compiled it will take a little while since it has
to fetch and build dependencies.

You should end up in a REPL which is connected to your ClojureScript
application running in the browser. It uses websockets to communicate.

Visit `localhost:3449` in your browser and open up the web console. 

You should see `Edits to this text should show up in your developer console.`.

Open `src/om_tut/core.cljs` in your editor and change the string given to the
`println` function. As soon as you save the code in the browser will be
reloaded and the new text will be displayed in the console.

Pretty neat!

Now try this, go to the REPL and type `(println "Hello from the REPL")` and you
will see this message in the browser console.

This is a truly interactive environment for ClojureScript and can be used as a
basis for developing a declarative web UI.
