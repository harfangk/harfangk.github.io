---
layout: post
ref: my-brief-foray-into-the-land-of-haskell-2
date: 2017-04-29 00:00:00 +0900
title: My Brief Foray into the Land of Haskell (2)
lang: en
---

These are what I've learned during my brief foray into Haskell.

## Object Classes Are Not Types

I began learning software development with Ruby. In Ruby, everything is an object. An integer value is an instance of the class `Integer`. Likewise, a string value is an instance of the class `String`. I took these concepts to the heart. For me, type and object class were synonyms. They made little difference in Ruby. 

Elixir shook my naive misunderstanding a bit since it had no objects. But I just replaced Ruby data objects with Elixir data types without thoroughly reconsidering my view. For Elixir's simple type system, that was enough.

Haskell, on the other hand, lives and dies by its types. It completely broke my thoughts that object classes and types are similar things. Although those two concepts are  closely aligned in Ruby, I learned that they are different concepts after all. Haskell abstractions on data types, such as algebraic data types or type classes, opened up a whole new kind of abstraction to me. 

## The Perfectionist Compiler

My first encounter with a compiler was in Java. I hated it. Compiler was an evil thing that forced me to manually annotate every single variable, write verbose code, and pedantically criticized me for simple mistakes.

Swift compiler was much better. It had type inference so it didn't force me to write verbose code. It made me adopt a debugging style different from the `puts` oriented style I've used in Ruby, but it was okay. 

Haskell compiler was a whole new beast. It was more powerful and thorough than other compilers by an order of magnitude. It made a crystal-clear demand that my program had to specify correct input types, process those inputs with functions with correct type signatures, and produce correct output types. In other words, it had to be correct from the beginning to the end at least in terms of types.

After all, all program should be correct. But Haskell was different in that it required rigorous correctness immediately upfront, whereas other languages delegated a large portion of writing correct code to later phases of writing code.

Haskell compiler felt like a brilliant and meticulous perfectionist. Still, it didn't feel unnecessarily petulant. 

## Who Framed Generic Programming

I first encountered generic programming in Java. I had a terrible first impression - it felt incredibly clunky and convoluted. Then I came across generic programming once more in Swift later. I've learned much more about programming in the meantime and saw more use in it, but it still required too many ceremonies to use. I considered it as a poor attempt at simulating duck-typing in a statically typed language. 

Haskell showed me the full power of generic programming - more commonly called parametric polymorphism in Haskell. Whereas generic programming in Java or Swift felt like an awkward workaround the language, Haskell provided a natural, inherent capability for parametric polymorphism. 

 

## Through the Laziness-Glass

## Monad, Monad, Monad

{% highlight haskell %}
f :: Int -> Int -> Int -> Int
f x y z = x + y + z
{% endhighlight %}

{% highlight haskell %}
{% endhighlight %}