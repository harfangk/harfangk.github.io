---
layout: post
ref: understanding-computation-review
date: 2017-08-16 00:00:00 +0900
title: Understanding Computation: Review
lang: en
---

## Summary

If you are a software developer without computer science background but want
a friendly introduction to computer science, I highly recommend this book.

This book explores some of core concepts of computer science, specifically
syntax and semantics of programming languages and basic abstract machines that
can execute programs. The author writes in a concise, easy-to-read, and
lighthearted style, using only codes that you would write at your daily job as
examples, instead of a typical academically rigorous writing full of bizarre
mathematical symbols.

## What Makes It Special

What I found most impressive was its easy-to-read style. I could just read
through the book because the author intentionally avoided using convoluted
expressions or sentence structure, whereas I usually have to go back and forth
between pages when I read theoretical books. He also stays quite down-to-earth
and oddly humorous, living up to the British stereotype. For example, after
introducing a massive mathematical notations that takes up a whole page, he
follows up with these words:

> Mathematically speaking, this is a set of inference rules that defines
a reduction relation on SIMPLE’s abstract syntax trees. Practically speaking,
it’s a bunch of weird symbols that don’t say anything intelligible about the
meaning of computer programs.

The book also provides concrete examples written in actual Ruby code for each
abstract concept it introduces. I think this is the most important factor that
sets this book apart from other books on computer science, because source code
is the language that software developers, the intended target audience of this
book, are most comfortable with. Those examples capture the abstract concepts
in concrete programs that we can build and interact with in the console, where
we feel comfortable.

For example, this is one of the code implementations the author provides when
he explains the concept of operational semantics and how a program gets
evaluated and reduced to its final value:

```ruby
>> Machine.new(
 LessThan.new(Number.new(5), Add.new(Number.new(2), Number.new(2)))
  ).run
  5 < 2 + 2
  5 < 4
  false
  => nil
```

The author doesn't stop at simple examples. In the last part of the book, he
explains the halting problem and demonstrates it with a Ruby program, not with
abstract mathematical thought exercise. I thought this was both incredibly
brilliant and comprehensible. Here's the demonstration:

```ruby
# do_the_opposite.rb

def halts?(program, input)
  # parse program
  # analyze program
  # return true if program halts on input, false if not
end

def halts_on_itself?(program)
  halts?(program, program)
end

program = $stdin.read

if halts_on_itself?(program)
  while true
  # do nothing
  end
end
```

> This code reads program from standard input, finds out whether it would halt
> if run on itself, and promptly does the exact opposite: if program would
> halt, do_the_opposite.rb loops forever; if program would loop forever,
> do_the_opposite.rb halts. Now, what does ruby do_the_opposite.rb
> < do_the_opposite.rb do? Just as we saw earlier with does_it_say_no.rb, this
> question creates an inescapable contradiction.

Explaining a new concept in a easy to understand manner is already an
impressive feat. Writing a whole book that introduces numerous complex concepts
while still keeping it easy to read is definitely worthy of applause. So
I cannot but appreciate the author's meticulous effort at catering the book to
his target audience.

## What Topics Does It Cover

The book covers a variety of topics, mostly those from programming language
theory and models of computation. Interestingly, the book has a kind of
narrative structure instead of simply listing technical concepts.

At first it briefly introduces various ways for understanding the meaning of
a program, such as operational semantics or denotational semantics. Then it
describes simple abstract machines, such as finite automata or pushdown
automata, that can execute those programs, while explaining how to parse the
programs into a format that machines can execute.

Afterwards the book introduces increasingly powerful abstract machines,
culminating in a universal Turing machine. Then it explores properties of
universal Turing machine by comparing it with other universal systems like
lambda calculus, cyclic tag system, or iota language. Finally it analyzes the
limitations of universal Turing machines and introduce undecidable problems
that cannot be generally solved by universal Turing machine, which is the most
powerful computer we can currently build.

That's a lot of complex topics to cover in a book that's slightly longer than
300 pages. Still, because of the way the author lays down it all in the book,
it's actually an enjoyable experience.

## What Did I Learn

As a newcomer into software development, in particular web development, I study
everyday to get better at this field. Still, the more I study, the more there
is to learn. Horizontally, there's an ever-increasing number of different ways
of doing things. There are new cool languages, yet another JavaScript
framework, new ways of deployng to AWS, better ways of writing API interface,
and so on. Then again, vertically, there are abstraction layers that reach
deeper than nine circles of Hell. I write in high level language, which gets
parsed, then compiled to bytecode, which is converted to machine code, that
gets executed at CPU, which is built with gates, and so on. Why, just today
I learned that below TCP/IP lie 7 layers of Open Systems Interconnection.
I guess even nine circles are not deep enough for this field.

Because I was overwhelmed by the number of things to learn, I couldn't even
think about what all these things aim to achieve in the end. The book taught me
that they could be abstracted away as implementation details, and that there
exists a bigger picture, that is, the meaning and intention of a program.
Without thinking about that ultimate goal, there was no way I could wholly
understand the goals and structures of these individual pieces of technology.

In terms of human communication, it was like being too immersed in technical
details of communication like how is a sentence structured grammatically, how
is a spoken sentence vocalized, or how does the human auditory system work.
They are valuable topics on their own, but without taking account of their
ultimate goal of conveying ideas among human beings, our understanding of the
topics would be incomplete at best.

This new way of thinking about program was the most valuable lesson I learned
from reading this book.

Discussion about undecidable problem also gave me answers to some lingering
questions I had. For example, as I wrote tests for my Ruby program, I thought
about how many tests I would have to write until I could trust that my program
would run without error. Is it possible to reach a point like that if I kept
filling in the test cases? Now I know what Rice's theorem is: any non-trivial
property of program behavior is undecidiable. So I learned that the answer to
my question is a definitive no.

When I first learned Haskell, I was thrilled by the slogan that "if it
compiles, it works". They said that the compiler guarantees that there'd be no
error in the program. But then how come it was still possible to have runtime
in errors in Haskell? I shrugged the question away, thinking that no system is
perfect. Now I have theoretical proof that such a guarantee is impossible. So
a rigorous type system can drastically reduce a category of errors, but it
cannot eliminate all errors.

That was quite a depressing lesson. So I am forever bound to work with
softwares that are doomed to eventual uncertainty? So I would never know
beforehand if this thing would work or blow up in my face in 10 seconds or
after processing 65,536 requests? I could have just continued working in
international diplomacy if I wanted to work with such uncertainties.
