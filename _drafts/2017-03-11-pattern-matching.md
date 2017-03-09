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

## Pattern Matching = Desctucturing + Control Flow

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

This is a function from Elixir core library `Enum` module. As its name suggests, it grabs a random element from the given argument. It calls the `Enumerable.count/1` function, which returns a two-element tuple of `{:ok, value}` or `{:error, message}`, a common convention in Elixir. Then it pattern matches that return value and calls appropriate functions based on the match. 

Let's look at the first case. `{:ok, 0}` matches only to the tuple with those particular values. In other words, it's matching on an exact integer value.

The second case `{:ok, count}` matches to all tuples that have `:ok` as the first element, and any value other than 0 as the second element. At the same time, it binds that second element to a variable called `count`. 

The third case `{:error, _}` matches to all tuples with `:error` as the first element, regardless of the second element.

I would like to bring your attention to two points here.

First, pattern matching can be used to extract values from data structures. This is more commonly known as destructuring. In this case it's used to match against values in a two-element tuple, but it's not limited to a single kind of data structure. You can add cases like `{{a, b, c}, d, e}`, a three-element tuple that has a three-element tuple as its first element, or `%{"a" => x}`, a map that has `"a"` key in it, to the above example and it will still work. 

This ability to specify the data structure you want, then freely destructure it, and then to pass those extracted value to other functions is extremely powerful and productive.

Note that I'm just demonstrating a possibility here. If you ever have to write a pattern matching that covers wildly different data structures, there's something terribly wrong with the input that you are pattern matching against so avoid it like a plague.

Second, pattern matching can be used to provide exhaustive control flow against any input. `_` means a wild card that matches against any value, so a case like `_ -> {:error}` can be used to provide a default behavior to a function when all other previous cases do not match. This ensures that a function always returns something against all inputs.

All this can be done through pattern matching. So pattern matching provides a succint way to to handle control flow, destructure data, and guarantee an exhaustive list of patterns.

But it doesn't stop there.

## Multi-clause Functions for Code Organization

A useful syntactic sugar for pattern matching is multi-clause function definitions. The full definition of the `Enum.random` function I've introduced above looks like this:

{% highlight elixir %}
@spec random(t) :: element | no_return
def random(enumerable)

def random(first..last),
  do: random_integer(first, last)

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

The first two lines are type specs for the function, so they are not relevant for this post. Let's look at the rest of the function definition.

There are two function definitions with identical names. It might look like function overloading, but it is not. It's just a syntactic sugar to separate pattern matching into two different function heads. In fact, the above function can be written like this, too:

{% highlight elixir %}
@spec random(t) :: element | no_return
def random(enumerable)

def random(enumerable) do
  case enumerable do
    first..last -> random_integer(first, last)
    _ -> 
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
end
{% endhighlight %}

So what's so great about this seemingly mundane feature? Multi-clause function is great because it provides a natural way to break down functions into smaller parts that are easier to understand and maintain. Remember that pattern matching is already being used to extract data, manipulate control flow, and guarantee an output. On top of that, it's also used for organizing code.

## Recursion and Pattern Matching

Let's take a look at another practical example of pattern matching. Recursion is a basic building block of functional programming, and it works seamlessly with pattern matching. Here's `Enum.reverse/1` function from Elixir core library, which takes an enumerable and returns a list with elements in reverse order.

{% highlight elixir %}
@spec reverse(t) :: list
def reverse([]), do: []
def reverse([_] = l), do: l
def reverse([a, b]), do: [b, a]
def reverse([a, b | l]), do: :lists.reverse(l, [b, a])
def reverse(enumerable), do: reduce(enumerable, [], &[&1 | &2])
{% endhighlight %}

Note that the first three cases match against lists with 0, 1, and 2 elements respectively. They represent the base case of a recursive function. Pattern matching makes the base case stand out, making the recursive functions much easier to write and to understand. 

## Pattern Matching Is in the Leading Role

But if you actually think about it, none of the capabilities of pattern matching I've described here is new. 

Switch statement has existed for decades in imperative languages. Destructuring is now supported in object-oriented languages that have more aggressively introduced elements of functional programming, such as Ruby, Swift, and ES6 JavaScript. Still, pattern matching in those languages have limited power and is relegated to a secondary role.

In Elixir, pattern matching plays the leading role. The entire language supports pattern matching, allowing it to show its full potential. And I think that's what makes using pattern matching feel so nice in Elixir. 

In practice, this means that you can think about everything in terms of pattern matching. Destructuring? Use pattern matching. Control flow? Use pattern matching. Code organization? Use pattern matching. This gives you a natural flow of writing and organizing code without stopping to think about which tool to use for each particular case. It makes writing code in Elixir such a pleasure.

## Conclusion

The power of pattern matching in Elixir cannot be described just in terms of its capabilities. It's the harmony between the whole language and the tool that makes it such a pleasant experience. 

If you haven't tried Elixir, I hope this post has given you enough incentive to do so!

## Theoretical Side Note

There are reasons why object-oriented languages make less use of pattern matching. I would even claim that some parts of object-oriented paradigm are in direct conflict with pattern matching.

Pattern matching allows you to match on data structures and deconstruct them. In order to do that, you need know about the internal structures of the data to write match cases, and have access to the internal structure to perform the match and deconstruct the data. This goes directly against the concept of escapsulation, an integral building block of OOP. 

Pattern matching then allows you to explicitly state how do you want to process that matched data. In OOP, this is equivalent to using control flow statements against the type of an object to explicitly state what to do with it. Such a practice is frowned upon in OOP, as the paradigm is based on delegating such decisions to objects.

Instead, it's recommended that you rely on [ad hoc polymorphism](https://www.wikiwand.com/en/Ad_hoc_polymorphism) (usually implemented through [function overloading](https://www.wikiwand.com/en/Function_overloading)) in statically typed languages,  [duck typing](https://www.wikiwand.com/en/Duck_typing) in dynamically typed languages, and [subtype polymorphism](https://www.wikiwand.com/en/Subtyping) (just called polymorphism in OOP) for function dispatching. 

Different multi-paradigm languages go around this issue in different ways. Scala provides extractor objects to support destructuring objects without compromising encapsulation. Ruby provides convenient interfaces for destructuring commonly used data structure objects, but separates control flow from them. Swift limits destructuring to tuples, while supporting Swift-specific features such as unwrapping optionals or validating type casting. ES6 JavaScript provides powerful destructuring functionality, but separates it from control flow.