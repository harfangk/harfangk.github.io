---
layout: post
ref: elm-function-operators
date: 2018-02-21 00:00:00 +0900
title: Guide to Function Operators in Elm: |>, <|, >>, <<
lang: en
---

## Summary

Once we get through the introduction to Elm, we start to encounter some odd-looking operators in Elm
codes. I'm talking about the ones like `>>`, `<<`, `|>`, and `<|`, which modify how functions are
composed and applied. In this post I will try to explain how and when to use them.

## Why Do We Need These?

We don't. I mean, Elm as a language will still work fine even if we removed these operators. But
these operators let us write codes in a way that more naturally maps to our mental models, mainly by
helping us compose smaller functions into a more abstract function or express a function as a series
of smaller functions. Let's see some examples.

## Examples

### `|>` Operator

Here's an example task that I want to accomplish: given a list of random integers, I want to get the
first digit of each integer. Here's my initial implementation without function operators:

```elm
getFirstDigits : List Int -> List Int
getFirstDigits list =
    let
        stringList = List.map toString list
        firstStringDigits = List.map (String.left 1) stringList
        firstResultIntDigits = List.map (String.toInt) firstStringDigits
        firstIntDigits = List.map (Result.withDefault 0) firstResultIntDigits
    in
        firstIntDigits
```

I had to assign the result of each smaller operation and pass it over to next operation. It's not
bad - we see this kind of code often in imperative languages. But `|>`, commonly known as `pipe
operator`, can make this code much cleaner.

```elm
-- In multiple lines:
getFirstDigits : List Int -> List Int
getFirstDigits list =
    list
    |> List.map toString
    |> List.map (String.left 1)
    |> List.map (String.toInt)
    |> List.map (Result.withDefault 0)

-- In one line:
getFirstDigits : List Int -> List Int
getFirstDigits list =
    list |> List.map toString |> List.map (String.left 1) |> List.map (String.toInt) |> List.map (Result.withDefault 0)
```

Thanks to `|>`, we no longer have to assign intermediate results to pass them to the next step. How
does this work? `|>` passes its first argument to its second argument, which is always a function,
as can be seen in its type signature: `(|>) : a -> (a -> b) -> b`. Instead of expressing the
function as several distinct operations, we can express it as a pipeline of connected operations.

It's important to note that there's no magic involved here. `|>` is just a syntactic sugar. The
above function can be expressed like this too:

```elm
getFirstDigits : List Int -> List Int
getFirstDigits list =
    (List.map (Result.withDefault 0) (List.map (String.toInt) (List.map (String.left 1) (List.map toString list))))
```

But we use the style using `|>` because it is easier to read and understand that way. In general,
`|>` is useful when you want to express a series of connected operations.

### `<|` Operator

`<|` is the reverse of `|>` as it pipes its second argument into its first argument. Its definition
is `(<|) : (a -> b) -> a -> b`. Using `<|`, the above function can be writen like this:

```elm
getFirstDigits : List Int -> List Int
getFirstDigits list =
    List.map (Result.withDefault 0) <| List.map (String.toInt) <| List.map (String.left 1) <| List.map toString <| list
```

As you can see, `<|` lets us express the flow from right to left. If you are used to mathematical
notations, which flow from right to left, you'll feel at home. Or maybe if your native language is
written right-to-left. Personally I find that the right-to-left flow conflicts with the
overall top-left-to-bottom-right flow, so I rarely use this operator. But I do find it useful in
some cases. For example, I like it when the last argument of a function spans over multiple lines
like this:

```elm
-- with <|
test : Test.Test
test "subtraction should have identity property" <|
    \_ ->
        let
            a = negate (2 ^ 50 + 32)
            b = 0
        in
            Expect.equal (a - b) a

-- with parentheses
test : Test.Test
test "subtraction should have identity property"
    (\_ ->
        let
            a = negate (2 ^ 50 + 32)
            b = 0
        in
            Expect.equal (a - b) a
    )
```

I find that `<|` provides a nice visual separation, telling me that I don't have to think about the
code that comes before `<|` until I'm done with the code that comes after it.

### `>>` and `<<` Operators

These two operators let us compose two functions into one larger function. They are easier to
understand with actual examples:

```elm
toString : a -> String

String.length : String -> Int

toString >> String.length : a -> Int

String.length << toString : a -> Int
```

As you can tell from the function signatues, `>>` and `<<` combines two functions into one. Their
definitions show this in more abstract way.

```elm
(<<) : (b -> c) -> (a -> b) -> (a -> c)

(>>) : (a -> b) -> (b -> c) -> (a -> c)
```

Functionally they are identical, except that they work in opposite directions. The direction of
arrow hints at the direction of function composition. But if they are almost identical, which one
should we use? I think this is more about personal taste. Using either composition operators, our
example function can be expressed like this:

```elm
getFirstDigits : List Int -> List Int
getFirstDigits list =
    List.map (Result.withDefault 0) << List.map (String.toInt) << List.map (String.left 1) << List.map toString <| list

getFirstDigits : List Int -> List Int
getFirstDigits list =
    list |> List.map toString >> List.map (String.left 1) >> List.map (String.toInt) >> List.map (Result.withDefault 0)
```

If you prefer right-to-left flow, go with the pair of `<<` and `<|`. On the other hand, if you
prefer left-to-right flow, go with the pair of `>>` and `|>`. Just avoid mixing operators with
different flow direction - it's rather confusing.

But where do we use this function composition? Abstractly, they let us mix and match smaller
functions to create a larger one. Remember that all functions in Elm are curried and can be broken
down into functions that take only one argument. It means that we are given almost infinite
possibilities for mixing and matching functions.

Practically, this gives us incredible amount of freedom when writing funtions. Let's get back to our
example function.

```elm
getFirstDigits : List Int -> List Int
getFirstDigits list =
    list
    |> List.map toString
    |> String.left 1
    |> List.map (String.toInt >> Result.withDefault 0)
```

Here I used function composition to group two small functions into one for transforming string to
integer, because I think that those two functions are more meaningful when they are composed
toegther. I find function composition most useful when grouping logically coupled functions. You can
also separate them into a new function in that case, but function composition gives us a faster
inline option.

## Implications of Function Operators

Depending on the context and our mental model, there are dozens of different ways to express this code. Here's another one:

```elm
-- Split to separate function
getFirstDigits : List Int -> List Int
getFirstDigits list =
    list
    |> List.map toString
    |> String.left 1
    |> List.map stringToInt

stringToInt : String -> Int
stringToInt s = String.toInt << Result.withDefault <| s
```

Overall, I gave 9 different ways to write this function in this post. If we didn't have function
operators, we would have been restricted to using nested parentheses, intermediate assignments, and
defining new functions. As we've seen here, function operators greatly expand our freedom of
expression when writing code.

Nevertheless, they can lead to chaos when abused. Mixing different styles of functions makes it
harder for others to read and understand our code - which include our future selves. So it's
important to have disciplines and conventions when using these operators.

## Summary

We've looked at the four function operators in Elm: `>>`, `<<`, `|>`, and `<|`, and how they are
used in an example function. I've deliberately left out formal discussions about them because there
are already great resources about those topics. Besides, I found that discussions about when and how
to use these operators were almost nonexistent, so I wanted to fill in that gap. I hope that this
post was helpful in that respect.
