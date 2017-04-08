---
layout: post
ref: my-brief-foray-into-the-land-of-haskell-1
date: 2017-04-22 00:00:00 +0900
title: My Brief Foray into the Land of Haskell (1)
lang: en
---

## The Scary, Scary Land of Haskell

Haskell. I think few other programming languages invoked the same level of dreadfulness in me as Haskell did. After all, it is a language known for being arcane and unusual. Still, I love trying out new and difficult stuff. So I've been eyeing for an opportunity to venture into the land of Haskell for a while.

I kept looking for a way. 

## Setting Up the Plan

One of the most common complaints about Haskell is that it's so hard to learn. As I researched about why it was so hard, I could identify two causes:

* Haskell is disparate from mainstream imperative languages.
* Resources for learning Haskell are terrible in terms of being beginner-friendly.

I've seen the same pattern in universities. A professor is knowledgeable in her area but is terrible at teaching because she received no training in pedagogy. And I've already suffered from such professors in universities, so I wasn't keen on subjecting myself to the same kind of torture again. I had to find a good guidance.

Then I came upon an article titled [Functional Education](http://bitemyapp.com/posts/2014-12-31-functional-education.html) by @bitemyapp. The author discussed the problems in learning functional programming, and reviewed many existing resources for learning Haskell. I did not, and still do not, know enough to evaluate his reviews. But he sounded rational enough that I decided to follow his recommendation.

His recommendation was to go for Brent Yorgey's [cis194 course](http://www.seas.upenn.edu/~cis194/spring13/lectures.html) first and then [Data61 course](https://github.com/data61/fp-course).

Now I knew where to start. Time to venture forth.

## Flashback: My Equipment

In hinsight, I was actually quite well equipped to learn Haskell. Although I had barely a year under my belt as a professional software developer, my path somehow exposed me to many foundational concepts of Haskell. Specifically, I was already introduced to the following concepts: higher order function, recursion, type inference, parametric polymorphism, lazy evaluation, fold, and pattern matching.

I started my first serious software development with Ruby. Although Ruby is an objected-oriented language, it makes heavy use of higher order functions through blocks. Functions like `map` or `reduce`, which are basic patterns of functional programming, are also commonly used in Ruby.

Then I briefly worked with Swift for iOS development. Swift promotes using immutable data unless required otherwise. It also makes heavy use of higher order functions as callbacks. Swift also provides compile time type checking and type inference. Although I haven't really used generic programming in Swift, it did give me a chance to expose myself to generic programming.

My next serious involvement was with Elixir. Pattern matching and recursion are bread and butter of Elixir. I got more familiar with immutable data structures because Elixir, unlike Swift, provided no mutability whatsoever. I also gained deeper understanding of folding, which I've used in Ruby without really understanding it. I've known about the concept of lazy evaluation, but I first started using it in Elixir.

Still, I didn't know that I knew all these things when I dived into Haskell. Somehow the dots got connected in the end, I guess.

## 5/5 Would Recommend 

I liked the conciseness of the course material. It hit the right balance between terseness and verbosity. The instructor also provided relevant chapters from [Learn You a Haskell for Great Good](http://learnyouahaskell.com/) or [Real World Haskell](http://book.realworldhaskell.org/) as suggested reading each week. When the explanation in the course material felt too short, reading those chapters helped me learn more slowly and thoroughly. It's my favorite combination - concise core material complemented by extensive optional resources.

But I think the best thing about this course are homeworks. They are definitely challenging. I confess that I looked at the solutions available online when I couldn't even think of how to get started on some homeworks. Nevertheless, all homeworks can be solved just with the concepts introduced in the course material. This is actually an impressive feat - Yorgey made intellectually challenging homeworks out of concise course material. You would understand how hard that is if you've tried to create good exercise problems.

So I would recommend this course to someone who wants to learn Haskell. But here's a caveat. If you are completely new to functional programming, get a mentor or teacher. This course is still too fast-paced for such a person. 

If you are trying to learn functional programming by youself, I recommend starting with Elixir. Elixir is a simple, practical language that can familiarize you with basic concepts of functional programming. Once you have knowledge of those building blocks, learning more advanced functional programming concepts in Haskell would feel much less stressful.

In the next post, I will describe what I learned from my adventure in the land of Haskell.