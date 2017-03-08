---
layout: post
ref: pattern-matching
date: 2017-03-11 00:00:00 +0900
title: Pattern Matching
lang: en
---

One of the nicest features of Elixir is pattern matching. But it's hard to explain why or how it is nice - it's something experiential that arises from a combination of pattern matching and other elements of functional programming paradigm.

In this post I will try to demonstrate its usage in Elixir and show you why it is such a great feature. This post is mainly aimed at people experienced in object-oriented programming but without much exposure to functional programming. Let's get started.

## Pattern Matching? What Is That?

I will skip explaining what pattern matching is, since there already exist good explanations about it. Here's [one](http://elixir-lang.org/getting-started/pattern-matching.html) from official Elixir language guide, and another [one](http://elixirschool.com/lessons/basics/pattern-matching/) from Elixir School. Instead I will focus on the actual code you would write.

## Pattern Matching = Binding + Control Flow

At first glance, pattern matching might look like a glorified switch statement. But it's much more powerful than what you might have seen in an object-oriented language. Let's see a typical Elixir code that uses pattern matching.

{% highlight elixir %}
def random(enumerable) do
  case Enumerable.count(enumerable) do
    {:ok, 0} ->
      raise Enum.EmptyError
    {:ok, count} ->
      at(enumerable, random_integer(0, count - 1))
    {:error, _} ->
      case take_random(enumerable, 1) do
        []     -> raise Enum.EmptyError
        [elem] -> elem
      end
  end
end
{% endhighlight %}

This is a function from Elixir core library `Enum` module. As its name suggests, it grabs a random element from the given argument. It calls the `Enumerable.count/1` function that follows Elixir convention and returns a tuple of `{:ok, value}` or `{:error, message}`. Then it  pattern matches that return value and does different things based on the match. 

Let's look at the first case. `{:ok, 0}` matches only to the tuple with those particular values. In other words, it's matching on an exact integer value.

Second case `{:ok, count}` matches to all tuples that have `:ok` as the first element, and any value other than 0 as the second element. At the same time, it binds that second element to a variable called `count`. 

Third case `{:error, _}` matches to all tuples with `:error` as the first element, regardless of the second element.

This is far more powerful than switch statements from older object-oriented languages like Java. Other languages that have incorporated some elements of functional programming, such as Ruby or Swift, have more powerful switch statements with this level of power.

But pattern matching in Elixir is far, far more powerful than this.

## 


[009 RR What Makes Beautiful Code](https://devchat.tv/ruby-rogues/what-makes-beautiful-code)