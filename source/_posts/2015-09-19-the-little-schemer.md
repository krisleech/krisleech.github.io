---
layout: post
title: "The Little Schemer"
date: 2015-09-19 09:36
comments: true
published: true
categories: [clojure]
---

I've started reading [The Little Schemer](https://mitpress.mit.edu/books/little-schemer), it is a book about LISP, with a unique approach. It is presented in a question / answer format, a topic is presented in a sentence or two and then hit homne with small code examples to which you need to figure out the answer.

I already have Clojure installed so I decided to use that to try out some of the code example.

However some Clojure functions are named differently than in Scheme. Here I will record the differences as I find them.

<!--more-->

## Lists

On one of the first pages we are presented with lists and the question:

```
Is (1 2 3) a list?
```

If you want to type this, as is, in to Clojure REPL you will get an error. This is because LISP is homoiconic, meaning the language syntax is also valid data. LISP uses a list as a container for a function and its inputs. The first element is a function followed by the inputs. So `(1 2 3)` is evaluated by the LISP reader as `1` being a function to be given the inputs `2` and `3`, which raises an error since `1` is not a function.

Instead you need to quote the list:

```clojure
(quote (1 2 3))
```

Or the shorthand:

```clojure
`(1 2 3)
```

This applies everywhere you want to hardcode a list containing data.

## car

`car` in scheme returns the first item in a non-empty list. In Clojure it is `first`.

```clojure
(car `(1 2 3)) ; 1
(first `(1 2 3)) ; 1
```

## cdr

`cdr` in scheme returns the all but the first item of a non-empty list. In clojure it is `rest`.

```clojure
(cdr `(1 2 3)) ; (2 3)
(rest `(1 2 3)) ; (2 3)
```

Both `first` and `rest` accept empty lists, whereas `car` and `cdr` do not.

## cons

`cons` is the same in both Scheme and Clojure.

```clojure
(cons 9 `(1 2 3)) ; (9 1 2 3)
```

## null?

`null?` in Scheme is `empty?` in Clojure.

```clojure
(null? `()) ; #t
(empty? `()) ; true
```

In the Scheme I used `null?` also accepted arguments which where not lists and returned `#f` for everything I tried accept an empty list. Where as `empty?` in Clojure only accepts a list (or other collection) as its argument.

