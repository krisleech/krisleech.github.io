---
layout: post
title: "Clojure - partial"
date: 2015-03-20 09:36
comments: true
published: true
categories: [clojure]
---

Currying, partially applying arguments to functions, in Clojure.

<!--more-->

The function `partial` accepts a function and some, but not all, of the arguments for that function. It returns a new function which accepts the remaining arguments. This is known as [currying](https://en.wikipedia.org/wiki/Currying).

Using the function `<`, which accepts two arguments, as an example we can use `partial` to apply to the first argument and return a new function which accepts the second.

The function `<` accepts two arguments:

```clojure
(< 25 10) ; false
```

We can create a new function with just `25` applied:

```clojure
(partial < 25) ; fn
```

If we define a var and store the function returned by `partial` we can use it as follows:

```
(def greater-than-25 (partial < 25))

(greater-than-25 10)  ; false
(greater-than-25 100) ; true
```

Since `partial` returns a function it can be used with high-order functions such as `map`.

```clojure
(map (partial * 10) [10 20 30 40]) ; 100 200 300 400
```

The function `map` accepts a function (which accepts one argument) and a collection, the function is applied to each item in the collection.

Docs: https://clojuredocs.org/clojure.core/partial
