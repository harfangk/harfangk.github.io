---
layout: post
ref: elixir-usability-improvement-over-erlang
date: 2017-03-18 00:00:00 +0900
title: What Makes Pattern Matching in Elixir So Nice?
lang: ko
---

Important overhaul Elixir did to Erlang to become appealing web-development tool

---

I often hear the same question: if Elixir works on the Erlang VM, why not use Erlang for programming web-applications instead of Elixir? The short answer is – you can’t create something akin to Phoenix/Ecto with pure Erlang.

Does it mean that Elixir is a totally different language? No, it doesn’t. I would describe Elixir as 80% of Erlang and 20% of important stuff that drastically improved usability of the language, especially when writing web-applications. The funny thing is that even Joe Armstrong once said that Erlang is not suited for web-development. But I would say that Erlang is more verbose to write web-applications.

So, let’s discuss most important differences between Elixir and Erlang.

1. Fixed strings

Erlang has hard relations with strings. First of all, because of Prolog heritage original string type is just a list of characters which is very inefficient. No one uses this type now, they use binaries instead. But binaries have weird syntax. Imagine you write a simple string and that’s what it looks like:

{% highlight erlang %}
<<"Hello world">> % binary-string in Erlang
{% endhighlight %}

{% highlight elixir %}
"Hello world" # the same string backed by binary but in Elixir
{% endhighlight %}

I had to create a special shortcut in Vim just not to type this mess of characters while programming in Erlang.

Another issue with binaries is that Erlang doesn’t have a decent library for working with them as as it does for strings. Of course, you can roll-out your hand-made solution, but it’s a big pain and fragmentation, because nobody would know an API of your library. Also, regexps in Erlang are supported only as a library. There is no operators and first-class support in syntax for them.

Elixir supports strings very well. The syntax is just simple quotes. String module has a very good API and even “left_pad” is present – String.pad_leading. Regexps have a special literal, comparison operator =~ and improved Regexp module.

2. Superb standard library

Historically, Erlang has a very inconsistent and poor standard library. For example, module lists doesn’t have a lot of important functions. Say, you want to find the first list element which is >10. Here is an Erlang version:

{% highlight erlang %}
case lists:dropwhile(fun(X) -> X =< 10 end, [1, 3, 8, 15, 7, 100]) of
  [] -> nil;
  [Y | _] -> Y
end.
%=> 15
case lists:dropwhile(fun(X) -> X =< 10 end, [1, 3, 8, 15, 7, 100]) of
  [] -> nil;
  [Y | _] -> Y
end.
%=> 15
{% endhighlight %}

The same solution in Elixir:


{% highlight elixir %}
Enum.find([1, 3, 8, 15, 7, 100], fn(x) -> x > 10 end) #=> 15
{% endhighlight %}

In Elixir, everything is put in very logical (especially if you know Ruby) modules. Also, there are brilliant Enum and Stream modules which unify work with lists, maps and other iterables. And they’re only partially covered by Erlang stdlib.

3. Added namespaces

While it looks like not a big deal, it is. Every Erlang module lives in a global flat space. There are no namespaces, no packages. So, not to collide with other packages, you should always prefix your modules with some unique tag. Then this prefix is used across the whole project and it’s just garbage. For example, if your project name is Cowboy, best practice is to have all modules to be prefixed with “cowboy_”: “cowboy_router”, “cowboy_stream”, etc.

And then, if you want to call a function from some module, you need to use this name in a code:

{% highlight erlang %}
cowboy_req:reply(400, Req).
{% endhighlight %}

Elixir emulates namespaces and submodules with aliases (because it still works in the ErlangVM). If a project has some funky name, e.g. “TenMinutesBlog” and it has a model TenMinutesBlog.User, you don’t need to use the full name because there is alias directive:

{% highlight elixir %}
alias TenMinutesBlog.User
{% endhighlight %}

Now it’s possible to reference this module just with a User name.

4. Added Structs

One of the most hated part of Erlang is records. They were added in a very ad-hoc manner and with a verbose syntax. Here is how to create and use a Person record in Erlang:

{% highlight erlang %}
-module(using_record).
-record(person, {fname, lname, phone, address}).

full_name(Person) ->
  Person#person.fname ++ " " ++ Person#person.lname.
{% endhighlight %}

Noticed that ‘#person’ noisy part? That’s how records roll in Erlang.

In Elixir structs are as simple, as they should be, and a similar code looks like this:

{% highlight elixir %}
defmodule Person do
  defstruct fname: nil, lname: nil, phone: nil, address: nil
end

defmodule UsingStruct do
  def full_name(person) do
    person.fname <> " " <> person.lname
  end
end
{% endhighlight %}

5. Allowed variable rebinding

Because functional languages use immutable data structures, any change to a data creates new data. So, every time you mutate a state – you need to assign it to a new variable. This leads to a very fragile code, if you want to add something inside your logical steps and carefully rename variables. Here is an Erlang code:

{% highlight erlang %}
Users1 = user:get_all_users(),
Users2 = user:add_user(Users1, User),
Users3 = user:remove_user(Users2, User2)
{% endhighlight %}

As soon, as we need to insert some intermediate step, we need to rename User3 to User4 and so on. This is a common issue in a pure FP-languages, and for example in Haskell you should use State-monad to handle such logic.

José Valim did a very dare trick in Elixir and allowed variable rebinding. Aha – so Elixir has a mutability! But it’s not a mutability per se. Because original data structure is not mutated and Elixir doesn’t have cycles, it’s safe. Same code in Elixir:

{% highlight elixir %}
users = User.get_all_users
users = User.add_user(users, user)
users = User.remove_user(users, user2)
{% endhighlight %}

Now you can add any intermediate step and nothing has to be renamed.

6. Added Pipe-operator |>

Erlang doesn’t have pipe operator, so if you want to compose a lot of calls, it’s better to split them and make some intermediate variable assignments. For example, let’s filter only even numbers, square them and reverse the final list in Erlang:

{% highlight erlang %}
lists:reverse(
  lists:map(
    fun(X) -> X * X end, lists:filter(fun(X) -> X rem 2 == 0 end, [1,2,3,4])
  )
).
{% endhighlight %}

This code is hardly readable and should be split into several steps. Elixir’s pipe operator provides much more readable solution:

{% highlight elixir %}
[1,2,3,4]
|> Enum.filter(fn x -> rem(x, 2) == 0 end)
|> Enum.map(fn x -> x * x end)
|> Enum.reverse
{% endhighlight %}

7. Added polymorphism

Erlang doesn’t have a simple way to emulate polymorphism. Elixir, on the contrary, has protocols and you can describe any set of functions that protocol should support.

For example, if we want to create our own iterable data structure and use it via Enum module, we need to implement all interface functions from the Enumerable protocol. Then we can work with our custom type using Enum module. This is used in Ecto and Phoenix to provide means for third-party libraries to extend a range of different supported types without changing original libraries.

8. Added Lisp-style macros

Erlang has C-like macros – so it’s just a dumb text generation. It also has a very cumbersome parse-transform engine. Parse-transform allows you to make a lot of stuff, but amount of efforts is huge and extension can be very brittle. I wrote quite a simple extension for Erlang once and never used it in production, because I couldn’t be sure it’d work after Erlang version is changed.

Elixir is a totally different story. Actually, the core syntax is very small, and everything is built around gluing this small syntax with macros. Mostly any stuff which is ad-hoc in another languages, is a macro in Elixir, e.g. if/else, case, defmodule, def, etc. Actually, macros are the most important feature in Elixir which made possible all the DSL stuff in Phoenix and Ecto.

9. Changed syntax

You probably expect to see some rant how Elixir syntax is much better than the Erlang one. In fact, top-level Erlang syntax is more terse and allows to write very nice and readable code. You can express state machine in a pure Erlang syntax and it looks like a magic DSL. For example, here is Fibonacci implemented in both languages:

{% highlight erlang %}
-module(fib).
-export([fib/1]).

fib(1) -> 1;
fib(2) -> 1;
fib(N) -> fib(N - 2) + fib(N - 1).
{% endhighlight %}

{% highlight elixir %}
defmodule Fib do
  def fib(1), do: 1
  def fib(2), do: 1
  def fib(n) do: fib(n - 2) + fib(n - 1)
end
{% endhighlight %}

Erlang code looks very similar to a normal mathematical notation, while Elixir is more verbose. But sometimes Erlang code is harder to change, because it uses semicolons and commas for statement separation. Also, Elixir derived syntax from Ruby, which made it very appealing for any Ruby developer.

When to use what?

Elixir is compiled to Erlang byte-code and it tries very hard to provide cool features with zero overhead. So, it’s unlikely that writing Erlang code you will get faster execution. But if you write some “low-level” stuff or a common library and want to share it with the whole eco-system, it makes sense to write it in Erlang.

On the contrary, if it’s some application with a decent amount of business logic and a lot of people should work on it – Elixir is a way to go.
