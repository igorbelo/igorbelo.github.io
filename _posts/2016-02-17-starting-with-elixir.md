---
layout: post
title: Starting with Elixir
---
Recently, I decided to start the journey of learning a new programming language.

I've been my whole professional life working with the OO paradigm and I was looking for a new approach (challenge). Elixir was my choice:

<blockquote>Elixir is a dynamic, functional language designed for building scalable and maintainable applications.</blockquote>

Elixir is built on top of the [Erlang](https://www.erlang.org/):

<blockquote>Erlang is a programming language used to build massively scalable soft real-time systems with requirements on high availability. Some of its uses are in telecoms, banking, e-commerce, computer telephony and instant messaging. Erlang's runtime system has built-in support for concurrency, distribution and fault tolerance.</blockquote>

Although Erlang is a great language its not friendly syntax may drive off many developers.
Elixir, in turn, provides a great syntax (sometimes confused with Ruby) and access to Erlang implementations as well. Some of these characteristics are listed (and explained) below.

## Access to Erlang
Elixir provides access to Erlang functions, take the `tl` function as the example, which just returns the tail of a list:
{% highlight elixir %}
tl([1,2,3]) #[2,3]
{% endhighlight %}
Actually, if you check the `tl` [implementation at GitHub](https://github.com/elixir-lang/elixir/blob/v1.0.5/lib/elixir/lib/kernel.ex#L715-L717) you will see it just wraps the Erlang implementation of `tl`:
{% highlight elixir %}
def tl(list) do
  :erlang.tl(list) # :erlang is a module
end
{% endhighlight %}

## Pattern Matching
The pattern matching is one of the most important features of Elixir you should know. It will be present when you write your functions, evaluate results of operations and assign them to variables.

{% highlight elixir %}
{:ok, ok_result} = XptoWebService.request
{% endhighlight %}
What's happening in the code above? We are saying that the left side matches to the right side, which means that Elixir will assign a value to `ok_result` variable only if the right side returns a tuple with exact two elements and the first element must be `:ok`. Thus, the second element will be assigned to `ok_result` variable. Let's analyze some of possible results of `XptoWebService.request` and see either if it matches:
{% highlight elixir linenos %}
{:ok, %{}} # matches and assigns %{} to ok_result
{:error, "error reason"} # does not match
{:ok, %{}, :complete} # does not match since there's a third element
{% endhighlight %}

Pattern matching is also present when you invoke functions. Functions can have the same name in the module (functions are distinguished by its name and by its arguments). Let's see an example:
{% highlight elixir linenos %}
defmodule Xpto do
  def handle({:ok, result}) do
    # success implementation
  end

  def handle({:error, result}) do
    # log the error
  end
end
{% endhighlight %}
We just defined the same function name into the module, but when you can the `handle` function with `{:ok, ...}` Elixir matches to the first implementation.

We can also combine the pattern matching with the pin operator, which will give us the ability to expect patterns set in variables.
What would happen in code below?
{% highlight elixir %}
a = 1
{a, b} = {2, 3}
{% endhighlight %}
The value `2` was assgined to `a`. If we wanted to just use the `a` value in the matching instead of redeclaring it, we had to use the pin operator:
{% highlight ruby %}
a = 1
{^a, b} = {2, 3} # MatchError is raised as it only will match tuples with 1 as their first element
{% endhighlight %}

## No loops, recursion instead
It changes how things are being done underlying. The [Starting Guide](http://elixir-lang.org/getting-started/recursion.html) gives an excellent explanation about recursion.

At the end, you'll see yourself using Elixir abstractions present in modules that implement the [Enumerable protocol](http://elixir-lang.org/docs/v1.0/elixir/Enumerable.html) and [Comprehensions](http://elixir-lang.org/getting-started/comprehensions.html) instead of implementing `map` and `reduce` by your own.

## State
The operations in Elixir will preserve the state of your data. For example:
{% highlight elixir linenos %}
a = {} # after this, whenever you call the `a` variable an empty tuple is returned unless you redeclare it
Tuple.append(a, 1) # {1}
a # {}
{% endhighlight %}
Notice that line 2 produced the desired result but didn't change the `a` content in place.

In Ruby we have the similar behaviour for some data types such as strings and integers:
{% highlight ruby linenos %}
a = "hello world"
a.upcase # "HELLO WORLD"
a # "hello world"
{% endhighlight %}
However, it doesn't work the same way with arrays and hashes:
{% highlight ruby linenos %}
a = []
a.push(1) # [1]
a # [1]
{% endhighlight %}

The main reason for keeping state is concurrency. Imagine a lot of processes and flows operating over `a` and changing its content. We would probably produce wrong results (due race condition) if we ignored sync the access to `a`.

## Pipe Operator |>
The pipe operator in Elixir provides data transformation flow in a more readable way. It takes the result of the left side and pass as the first argument to the function on right side.
So, instead of doing this:
{% highlight elixir %}
String.split(String.upcase("hello world"), " ")
{% endhighlight %}
We could do this:
{% highlight elixir %}
"hello world"
  |> String.upcase #String.upcase("hello world")
  |> String.split(" ") #String.split("HELLO WORLD", " ")
{% endhighlight %}

## Processes
<blockquote>In Elixir, all code runs inside processes. Processes are isolated from each other, run concurrent to one another and communicate via message passing. Processes are not only the basis for concurrency in Elixir, but they also provide the means for building distributed and fault-tolerant programs.</blockquote>

Processes have their own mailbox of received messages and their own heap. Processes are different from OS processes and threads as they are lightweight in terms of memory and CPU.

Processes are huge and some of their concepts and abstractions such as agents and genservers (also processes) to handle messages and state changing deserve their own posts.

## Conclusion
I've just introduced some of Elixir concepts and implementations, there's a lot of things you should know and the [Getting Started Guide](http://elixir-lang.org/getting-started) is a good start.

Elixir is a very new language but I'm really exciting about how its ecosystem is being evolved.

In the next post(s) I'll talk more about processes in Elixir.