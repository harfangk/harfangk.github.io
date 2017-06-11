---
layout: post
ref: testing-cast-requests-elixir
date: 2017-06-12 00:00:00 +0900
title: Preventing Race Condition When Testing Cast Requests in Elixir
lang: en
---

This post assumes basic knowledge of `GenServer.cast/2` and `GenServer.call/2`
on how they work differently.

## TL;DR

Call a `GenServer.call/2` function after `GenServer.cast/2`. This prevents your
caller process from executing any more code until it receives a reply to the
`call/2` function. The receiver process handles and sends the reply to `call/2`
only after it has handled the message from `cast/2`, ensuring
sequential execution of code. `:sys.get_state/1` is conveniently available for
this kind of usage.

## Initial Solution

I discovered this issue when writing a simple test for `cast/2` function, which
calls the function and then makes an assertion about its expected result. The
test failed because of a classic race condition case: the assertion was made
before the `cast/2` function was handled by the receiver process.

I had to find a way to ensure that the assertion is made only after `cast/2`
was handled. The most immediate and crude solution was to call
`Process.sleep/1` to wait for a specified amount of time between `cast/2` and
`ExUnit.Assertions.assert/1`. This did make the test pass, but I wanted to
write something better than that.

## Better Solution

The next solution I found was to call `call/2` between `cast/2` and `assert/1`.
This makes use of how BEAM processes and their mailboxes work, and how `call/2`
works.

Mailbox concurrently receives messages sent by other processes, but
sequentially handles those messages. So a mailbox can serve as a sort of
synchronizing point for messages. For example, if we send messages A and B to
a process, we can be sure that A will be handled first and then B will be
handled.

Now remember that `call/2` blocks the caller process until it receives the
reply from the receiver process. By calling `call/2` after `cast/2`, we can
ensure that any code after `call/2` will be executed after the caller process
receives the reply. And the receiver process will handle the message from
`call/2` and send the reply only after it has handled the message from
`cast/2`. This solves the race condition.

You can see example codes [here](https://github.com/harfangk/url_shortener/blob/master/test/url_shortener_ets_cache_interface_test.exs).

## But Should We Even Solve the Problem?

But maybe there's an even better solution. If we can circumvent the whole race
condition issue, then we can get rid of the problem itself. This can be done by
just testing `handle_cast/2` that always accompanies the `cast/2` function.
Directly testing `handle_cast/2` spares us from having to deal with message
passing and race conditions that it causes. I believe that this is the best
approach for unit tests.

Unfortunately, that's not always an option. Integration tests require testing
the interaction among multiple processes. In that case, it might be necessary
to synchronize processes with `call/2` functions to simulate how they are
expected to work in production. On the other hand, sometimes we don't have
access to callback functions. In that case there is no other way but to test
both message passing and callback handling parts.

## Thinking Out Loud

By design, each BEAM process should be able to run on separate machines. This
means that message passing among BEAM processes should be able to deal with
classic network problems like availability, disconnection, unresponsiveness,
and so on. But my tests take none of these into accounts - they assume that
everything will be alright, which is never the case in the real world.

Is this okay? I guess so in this simple case, because the processes run within
the same BEAM instance. But if I do write more advanced distributed software,
I suspect I will have to write tests for potential network issues too.
