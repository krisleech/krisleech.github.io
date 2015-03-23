---
layout: post
title: "Clojure - arrow"
date: 2015-03-25 09:36
comments: true
published: true
categories: [clojure]
---

The arrow is used as a more readable way of piping a value through a number of forms, usually a function, in Clojure.

<!--more-->

```clojure
(->> 10 (inc) (str)) ; "11"
```

The first argument to `->>`, `10`, is given as the last argument to the first form, `(inc)`. So it expands to `(inc 10)`. The result of the first form is passed as the last argument to the second form and so on for further forms.

It is the same as `(str (inc 10))`.

It is a pipeline.

Another example using a collection as the input and map:

```clojure
(->> [1 2 3] (map inc)) ; [2 3 4]
```

```clojure
(->> [1 2 3] (map inc) (map str)) ; ("2" "3" "4")
```


```clojure
(def times-ten (partial * 10))

(->> [1 2 3] (map inc) (map times-ten) (map str))
; ("20" 30" "40")
```

Note `partial` returns a new function with some of the arguments already applied.

One more example:

```clojure
(->> [1 2 3] (map inc) (map times-ten) (reduce +) (str))
; "90"
```

Note after the `reduce` we have a number value not a collection.

It is expanded to:

```clojure
(str                    ; "90"
  (reduce +             ; 90 
    (map times-ten      ; (20 30 40)
      (map inc [1 2 3]) ; (2 3 4)
    )
  )
)

```
