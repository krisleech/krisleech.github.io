---
layout: post
title: "Erlang for Rubyists"
date: 2014-10-27 09:36
comments: true
published: true
categories: [ruby, erlang]
---

This is part 1 of a series on Erlang for the Rubyist.

<!--more-->

Disclaimer: I'm in the process of learning [Erlang](http://www.erlang.org/).

## Installing

`brew install erlang`

## Hello, world

```erlang
-module(hello).
-export([hello/0]).

hello() ->
  io:write("Hello, world!~n", []).
```

`module` says that everything in this file belong to the `hello` module. The module provides a namespace for a group of functions much like a `Module` in Ruby.

Erlang modules, unlike in Ruby, must start with a lowercase letter.

`hello()` is a function, this is the equivalent of a method in Ruby.

The `export` describes which functions can be called externally, it is the same idea as `public` methods in a Ruby class. Therefore by default functions in a Erlang modules are private and must be explicitly exported to be visible from outside the module.

The square brackets in the `export` indicate a list, like an Array in Ruby. The one and only element in there `hello/0` exports the `hello` function with zero arguments. In Erlang functions of the same name can be declared multiple times but different number of arguments know as the [arity](https://en.wikipedia.org/wiki/Arity) of the function.

The actual function `hello()` starts with `->`, like `def` in Ruby and ends with a period, like `end` in Ruby. Note that every line ends in a period.

In the body of the function we call the `write` function in the `io` module. The `io` module is part of the Erlang standard library known for historical reasons as OTP (Open Telecom Platform).

`io.write` is the same as `puts "hello, word\n"`. In Ruby `put` is defined in the `Kernel` module (which is mixed in to every object) so it is shorthand for `Kernel.puts` in Ruby.

The equivalent, verbosely written, in Ruby is:

```ruby
module Hello
  public

  def self.hello
    Kernel.puts "hello, world\n"
  end
end
```

The difference with Erlang modules is that they only provide namespacing, they can't be mixed in to an object, since there are no objects.

Like in Ruby Erlang will return the value of the last line in a method/function.

Variables in Erlang are scoped to the function in which they are declared. There is no such thing as an instance variable (like `@firstname`) in Erlang. There is no global state either.

Once assigned a variable in Erlang can never be reassigned. It, unlike in Ruby, is immutable. The equivalent in Ruby would be to `freeze` a variable after assigning it:

```ruby
name = 'Kris'.freeze
name = 'Chris' # error
```

In Erlang variables must start with an uppercase letter. So modules lowercase and variables uppercase, the reverse of Ruby.

```erlang
X = 1.
X = 2.  # => error
```

You can try the above out in the Erlang console, `erl`.

When a variable is assigned it is said to be bound.

The lack of mutation and global state is part of the reason Erlang can scale so well. Nothing is shared.

## Pattern matching

Pattern matching is one of the key features of Erlang (and other functional languages).

Pattern matching is a form of message dispatch based on the name and arity functions. It is similar to the idea of overloading a method. Overloading a method not possible in Ruby but can be faked using a splated argument.

```ruby
def run(*args)
  if args[0].is_a?(Hash)
    # ...
  else
    # ...
  end
end
```

However pattern matching takes overloading one step further and allows us to select a function not just on its name, not just the number of arguments, but also the _values_ of those arguments.

If Ruby had pattern matching we could do this:

```ruby
def fibo(0)
  0
end

def fibo(1)
  1
end

def fibo(n)
  n > 0 ? fibo(n-1) + fibo(n-2) : n
end
```

And depending on the _value_ passed to `fib` it would dispatch to the matching method.

In reality we have to do something like this:

```ruby
def fibo(n)
  return 0 if n == 0
  return 1 if n == 1
  fibo(n-1) + fibo(n+1)
end
```

The previous hypothetical version is more readable to my eyes. Testability is also potentially improved as our code is broken down in to smaller units.

In Erlang, with pattern matching, we can do this:

```erlang
-module(fib). 
-export([fib/1]).

fib(0) -> 0; 
fib(1) -> 1; 
fib(N) -> fib(N-1) + fib(N-2).
```

This looks similar to our hypothetical Ruby version.

Notice that in the `export` we have `fib/1`, that exports the `fib` function which has an arity of 1.

Pattern matching is also used when using the equals sign. Erlang treats the equals sign in two different ways depending on if the var has already been assigned or not. When the variable has not yet been assigned it is assigned, or bound. But if you use the equals sign on a bound var it acts more like an assertion, i.e. `==` in Ruby, but is raises an error if the the result is not `true`. This is due to pattern matching.

```
X = 1. # binds X to 1
X = 1. # no error
X = X. # no error
X = 3. # error
```

As we can see var’s once assigned are immutable, this is a side effect of pattern matching. The last line `X=3` does not match since `X` equals `1`, thus an error is raised. Here is another example. 

```erlang
Me = { person, 'Kris', 'Leech' }.

{ human, Firstname, Lastname } = Me.  # error

{ person, Firstname, Lastname } = Me.

FirstName. # => 'Kris'
```

On line two the pattern does not match and an error is raised.

Notice that in the third line, because the tuples match, assignment occurs for the `Firstname` and `Lastname` variables.

In our case we wanted to make sure that we only assigned if the first element of `Me` is the  atom `person`. If we did not care about the first element we could have done this `{ _, Firstname, Lastname } = Me.`. This would assign `Firstname` and `Lastname` regardless of the first arguments value.

The underscore is used when we don’t care about an argument, this is the same in Ruby when using blocks.

```ruby
{..}.each do |k,_|
  # ...
end
```

## Lists, Tuples and Atoms

Tuples, `{1,2,3}` are like fixed size arrays, there is no real equivalent in Ruby.

Lists, `[1,2,3]`, are like dynamically sized arrays, the same as `Array` in Ruby.

A List has a a tail and head, like `[1,2,3].first` and `[1,2,3].last` in Ruby. Except `Tail` and `Head` can also be used in pattern matching, for example:

```erlang
[Head|Tail] = [1,2,3,4].

Head. # => 1

Tail. # => [2,3,4]
```

Note that `Head` and `Tail` are variables and could have been named something else, e.g. `[First|Rest]`.

In Ruby we _can_ do something similar with a splat:

```ruby
head, *tail = [1,2,3,4]
head # => 1
tail # => [2,3,4]
```

In Erlang lists can also be concatenated with `++`:

```erlang
[1,2,3] ++ [4,5,6]. # => [1,2,3,4,5,6]
```
And in Ruby:

```ruby
[1,2,3] + [4,5,6]
```

An atom is like a symbol in Ruby. An atom must start with a lowercase letter. Since there are no objects or types it is common to use a tuple where the first element is an atom such as: `{ person, ‘Kris’, ‘Leech’ }` or `{point, 100, 240 }`.

Unlike in recent versions of Ruby atoms are _not_ garbage collected so should never be generated from user input to avoid using up all memory.

## Guards

Guards are clauses that can be added to a function to restrict pattern matching. For a match to occur the pattern must match and the guard must pass.

Guards are implemented with the `when` keyword. In this example we use a guard clause to check the input is within a range. In regular pattern matching ranges are not supported so we must use a guard.

```erlang
is_in_range(X) when X >= 22 ->
  true;
is_in_range(_) ->
  false.
```

Given `is_in_range(3)` the first matching function will be checked, the guard fails, and the next matching function is tried. In this case we don't care what the given value is because we know if it didn't pass the guard in the first function it must fail.

Note that in this case we need to use `;` after the first function instead of `.`. You can think of `;` a bit like `else` in Ruby.

You can add multiple guards which are comma separated, e.g. `when X >= 22, X <= 44`.

There is no equivalent in Ruby since we do not have pattern matching, the closest would be to raise if a condition on the given arguments is not met:

```ruby
def is_in_range(input)
  raise ArgumentError unless input >= 22
  # ...
end
```

## if statements

```erlang
is_it_okay(X) ->
  if X == 1 -> true;
     true   -> false
  end.
```

The syntax is a little odd and similar to `if/elsif/else` in Ruby. In Erlang every expression has to return something, but unlike in Ruby you have to explicitly return a value from an `if`. This is the `true` part. If none of the `if` clauses match the `true` (like `else` in a Ruby `case` statement) will be called.

You only need `true` if it is possible that `X` might not match one of your `if` statements. For example:

```erlang
is_it_okay(X) ->
  if X > 1 -> true;
     X < 1 -> false
  end.
```

In Part 2 (forthcoming) I will look Recursion, Concurrency and Servers. Subscribe to the [Atom feed](http://teamcoding.com/atom.xml) to be notified.

References: 

* https://www.scribd.com/doc/44221/Thinking-in-Erlang
* http://learnyousomeerlang.com/content
