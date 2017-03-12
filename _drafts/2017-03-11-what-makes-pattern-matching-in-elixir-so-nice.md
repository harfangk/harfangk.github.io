---
layout: post
ref: what-makes-pattern-matching-in-elixir-so-nice
date: 2017-03-11 00:00:00 +0900
title: What Makes Pattern Matching in Elixir So Nice?
lang: en
---

Many people pick pattern matching as one of the nicest features of Elixir. But it's hard to explain why or how it is nice - it's something experiential that arises from the combination of pattern matching and other elements of functional programming paradigm.

In this post I will present a few examples of pattern matching in Elixir to describe why it is such a great feature. The target audience of this post is people with experience in object-oriented programming (OOP) but without much exposure to functional programming (FP). Let's get started.

## Pattern Matching? What Is That?

Other people have written good explanations about pattern matching, so I will skip explaining what it is in this post. Since we're talking about Elixir, I'll post links to a few Elixir resources. Here's [one](http://elixir-lang.org/getting-started/pattern-matching.html) from the official Elixir language guide, and another [one](http://elixirschool.com/lessons/basics/pattern-matching/) from Elixir School. Instead I will focus on the actual code you would read and write.

## Real World Example of Pattern Matching

At first glance, pattern matching might look like a slightly more complicated switch statement. But it's much more powerful than that. Let's take a look at an example of typical Elixir code that uses pattern matching.

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

This is a function in `Enum` module from Elixir core library. As its name suggests, it returns a random element from the given `enumerable`. It first calls the `Enumerable.count/1` function, which returns a two-element tuple of `{:ok, value}` or `{:error, message}`, following a common convention in Elixir. Then it pattern matches that return value and calls appropriate functions based on the match. 

Let's look at the first case. `{:ok, 0}` matches to only one tuple: the one with `:ok` as the first element and `0` as the second element. 

The second case `{:ok, count}` matches to all tuples that have `:ok` as the first element, and any value other than `0` as the second element. At the same time, it binds that second element to a variable called `count`. 

The third case `{:error, _}` matches to all two-element tuples with `:error` as the first element, regarldess of the value of the second element.

## Pattern Matching = Destructuring + Control Flow

That one pattern matching with 9 lines are doing a lot of things in an incredibly concise way. Let's look at what it's doing.

First, it's checking the structure of the input data. All three cases match against two-element tuples. 

Second, it's accessing and checking the values contained in the data. The cases check whether the two-element tuples have `:ok` or `:error` as their first elements. 

Third, it's binding the value it accessed from the data. This happens in the second case, where it's binding the second element of the matched tuple to a variable.

These three operations together are simply called `destructuring` or `deconstructing` in FP.

Fourth, it's specifying how to respond to matched cases. Each case is followed by subsequent operations, which have access to the variable you bound through destructuring.

So pattern matching can specify the data structure you want, extract values from it, then pass those extracted values to other functions. These are operations that you do over and over again to implement more abstract logic. Pattern matching allows an incredibly succint way to efficiently write those codes with little boilerplate code.

But it doesn't stop there.

*If you are interested in a theoretical discussion about OOP and FP regarding pattern matching, I have written an optional section at the end of the post.*

## Multiple Clause Functions for Code Organization

A useful syntactic sugar for pattern matching is multiple clause function heads. Actually, the full definition of the `Enum.random/1` function I've introduced above looks like this:

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

The first two lines are type specifications for the function, so they are not relevant for this post. Let's look at the rest of the function definition.

There are two function definitions with identical names. It is a syntactic sugar to separate pattern matching cases into multiple clause function definition. In fact, the above function definition is equivalent to the following code:

{% highlight elixir %}
@spec random(t) :: element | no_return
def random(enumerable)

def random(enumerable) do
  case enumerable do
    first..last -> random_integer(first, last)
    enumerable -> 
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

As you can see, the first pattern matching was split into different function heads. So what's so great about this seemingly mundane feature? Multiple clause function heads are great because they provide a visually discernible way to break down functions into smaller parts that are easier to understand and maintain. 

Remember that pattern matching is already being used to extract data, manipulate control flow, and guarantee an output. On top of that, it can be also used for structuring the codebase. 

## Recursion and Pattern Matching

Let's take a look at another practical example of pattern matching. Recursion is a basic building block of functional programming, and it works seamlessly well with pattern matching. Here's `Enum.reverse/1` function from Elixir core library, which takes an enumerable and returns a list with elements in reverse order.

{% highlight elixir %}
@spec reverse(t) :: list
def reverse([]), do: []
def reverse([_] = l), do: l
def reverse([a, b]), do: [b, a]
def reverse([a, b | l]), do: :lists.reverse(l, [b, a])
def reverse(enumerable), do: reduce(enumerable, [], &[&1 | &2])
{% endhighlight %}

The following code is the same function written without multiple function heads.

{% highlight elixir %}
@spec reverse(t) :: list
def reverse(enumerable) do
  []         -> []
  [_] = l    -> l
  [a, b]     -> [b, a]
  [a, b | l] -> :lists.reverse(l, [b, a])
  enumerable -> reduce(enumerable, [], &[&1 | &2])
end
{% endhighlight %}

Notice multiple function heads make the base case, the first three cases in this example, more discernible. They also reduce the amount of code you must hold in your head to understand the function, making it much easier to write and understand functions.

## Pattern Matching Is in the Leading Role

But if you actually think about it, none of the capabilities of pattern matching I've described here is new. 

Switch statement has existed for decades in imperative languages for manipulating control flow. Destructuring is now supported in OO languages that have more aggressively introduced elements of FP, such as Ruby, Swift, and ES6 JavaScript. Still, pattern matching in those languages have limited power and is relegated to a secondary role because its incompatibility with some OO principles.

In Elixir, pattern matching plays the leading role. The entire language feels like it's built around pattern matching, allowing it to show its full potential. And I think that's what makes using pattern matching feel so nice in Elixir. 

In practice, this means that you can think about everything in terms of pattern matching. Destructuring? Use pattern matching. Control flow? Use pattern matching. Code organization? Use pattern matching. It creates an uninterrupted flow of writing and organizing code without stopping to think about which tool to use for each particular case. 

## Conclusion

The power of pattern matching in Elixir cannot be described just in terms of its own capabilities. It's the harmony between the simple functional elements of the language and the tool that makes it such a pleasant experience. If you haven't tried Elixir, I recommend you to at least take a look at it to see how pattern matching works in it.

---

## No Encapsulation and More Explicit Control Flow 

Destructuring gives you a free access into the internal values of data structures, which might look like a total disregard for encapsulation. It is. Except that encapsulation does not exist in FP.

One of the key reasons for encapsulation in OOP is to control who has access to the data. Only the object that holds the data is permitted to read and write those data. Any other entity must request that object to handle the data. 

FP, in contrast, solves the writing permission problem in a completely different way. All data are immutable, so no one can write to them. 

Since there's no more need to control writing permission, there's no need to encapsulate data within objects for access control. So FP separates them into data types and modules. In OOP terms, that could be described as splitting complex objects into simpler data transfer objects and service objects. 

Each FP language handles reading permission in its own way. Dynamically typed languages, such as Erlang/Elixir and Closure, tend to stick to the types provided by the language. Programmers know how to handle those basic types, which means they know how to interface with almost all data. In other words, programmers have something akin to universal reading permission in those languages.

On the other hand, statically typed languages, such as Haskell/ML family languages, are built on powerful type system. Programmers create their own types that are suited to the logic they are implementing. So unless you know the interface for those types, you do not have permission to read those types. This works as practical reading permission.

Multi-paradigm languages that leverage elements from both OOP and FP paradigms go around the encapsulation problem in different ways. Scala provides extractor objects for defining interfaces for destructuring operation without compromising object encapsulation. Ruby core library provides convenient syntax and methods for destructuring commonly used data objects, but separates control flow from them. Swift limits destructuring to tuples, while supporting Swift-specific features such as unwrapping optionals or validating type casting. ES6 JavaScript provides powerful destructuring functionality, but separates it from control flow.

Pattern matching also allows you to explicitly state how do you want to process the data based on its structure. This might look like manual method dispatch in OOP, which means using control flow statements against the type of an object to choose what to do with it. Such a practice is frowned upon in OOP, which prefers delegating such a choice to objects using techniques like [function overloading](https://www.wikiwand.com/en/Function_overloading) and [subtype polymorphism](https://www.wikiwand.com/en/Subtyping). In contrast, FP prefers explicitly handling this at function level.

Here's one way that could help you wrap your head around this difference. Switch statement in OOP is about instructing what to do in certain situations. This is characteristic of its imperative language heritage. In contrast, pattern matching in FP is about establishing rules for handling different cases. This is characteristic of its declarative language heritage.

Unlike dynamically typed languages like Elixir, statically typed languages provide additional benefits to pattern matching. They ensure that cases cover all possible inputs, that they are not redundant, and that they do not try to match against impossible data types. The tradeoff is that the code gets more complicated as the programmer needs to know and understand ever-increasing kinds of type interfaces.