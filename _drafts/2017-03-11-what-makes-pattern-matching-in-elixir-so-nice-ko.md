---
layout: post
ref: what-makes-pattern-matching-in-elixir-so-nice
date: 2017-03-11 00:00:00 +0900
title: 엘릭서의 패턴 매칭이 정말 사용하기 좋은 이유는?
lang: ko
---

Many people pick pattern matching as one of the nicest features of Elixir. But it's hard to explain why or how it is nice - it's something experiential that arises from the combination of pattern matching and other elements of functional programming paradigm.

엘릭서에서 사용하기 가장 즐거운 기능으로 패턴 매칭을 말하곤 합니다. 하지만 도대체 왜 사용하기 즐거운지 설명하려고 하면 참 어렵습니다. 단순히 패턴 매칭이 기능 때문만이 아니라 다른 함수형 프로그래밍 요소의 조화 때문에 나타나는 경험이기 때문입니다. 

In this post I will present a few examples of pattern matching in Elixir to describe why it is such a great feature. The target audience of this post is people with experience in object-oriented programming (OOP) but without much exposure to functional programming (FP). *It's not written to compare pattern matching in Elixir with pattern matching in other languages.*

이 글에서는 엘릭서에서 패턴 매칭을 사용한 예시를 몇 가지 보여주고, 패턴 매칭이 그렇게 좋은 기능인지 설명하려 합니다. 대상 독자는 객체 지향 프로그래밍(OOP)에 익숙하지만 함수형 프로그래밍(FP)에 대해서는 잘 모르는 사람입니다. 엘릭서의 패턴 매칭과 타 언어의 패턴 매칭을 비교하기 위한 글은 아닙니다.

Let's get started.

시작해볼까요?

## 패턴 매칭? 그게 뭔가요?

Other people have written good explanations about pattern matching, so I will skip explaining what it is in this post. Since we're talking about Elixir, I'll post links to a few Elixir resources. Here's [one](http://elixir-lang.org/getting-started/pattern-matching.html) from the official Elixir language guide, and another [one](http://elixirschool.com/lessons/basics/pattern-matching/) from Elixir School. Instead I will focus on the actual code you would read and write.

패턴 매칭이 무엇인지에 대해서는 다른 사람들이 잘 설명한 글이 이미 있으니 이 글에서는 직접 설명하지는 않겠습니다. 엘릭서 언어에 대해서 얘기하고 있으니 엘릭서 관련 자료를 몇 개 링크합니다. 엘릭서 언어 공식 가이드의 [설명(영문)](http://elixir-lang.org/getting-started/pattern-matching.html)과 엘릭서 스쿨에 있는 [설명(국문)](http://elixirschool.com/ko/lessons/basics/pattern-matching/)입니다. 이 글에서는 실제로 접하고 작성하게 될 코드를 살펴보겠습니다. 

## 엘릭서 패턴 매칭 실제 사용 예시

At first glance, pattern matching might look like a slightly more complicated switch statement. But it's much more powerful than that. Let's take a look at an example of typical Elixir code that uses pattern matching.

얼핏 보기에는 패턴 매칭은 조금 더 복잡한 스위치문처럼 보일 수도 있습니다. 하지만 패턴 매칭의 기능은 훨씬 강력합니다. 패턴 매칭을 사용하는 전형적인 엘릭서 코드를 살펴봅시다.

```elixir
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
```

This is a function in `Enum` module from Elixir core library. As its name suggests, it returns a random element from the given `enumerable`. It first calls the `Enumerable.count/1` function, which returns a two-element tuple of `{:ok, value}` or `{:error, message}`, following a common convention in Elixir. Then it pattern matches that return value and calls appropriate functions based on the match. 

엘릭서 코어 라이브러리의 `Enum` 모듈에 들어 있는 함수입니다. 함수 이름에서 알 수 있듯이 주어진 `enumerable`에서 무작위로 요소를 선택해서 반환합니다. 먼저 `Enumerable.count/1` 함수를 호출합니다. 이 함수는 일반적인 엘릭서 컨벤션을 따라 `{:ok, value}` 또는 `{:error, message}`라는 요소 두개짜리 튜플을 반환합니다. 그리고 함수는 그 반환 값에 대해서 패턴 매칭을 하고, 매치 결과에 따라서 적절한 함수를 호출합니다.

Let's look at the first case. `{:ok, 0}` matches to only one tuple: the one with `:ok` as the first element and `0` as the second element. 

첫 번째 케이스를 봅시다. `{:ok, 0}`는 첫번째 값으로 `:ok`, 두 번째 값으로 `0`을 가지고 있는 튜플 단 하나에만 매칭됩니다.

The second case `{:ok, count}` matches to all tuples that have `:ok` as the first element, and any value other than `0` as the second element. At the same time, it binds that second element to a variable called `count`. 

두 번째 케이스인 `{:ok, count}`는 첫 번째 값으로 `:ok`를 가지고 있는 모든 튜플에 매칭됩니다. 단 `{:ok, 0}`는 이미 이전 케이스에 매칭되었을 것이기 때무에 이 케이스에 도달하지 못합니다. 그리고 매치된 두 번째 값은 동시에 `count`라는 변수에 바인드 됩니다.

The third case `{:error, _}` matches to all two-element tuples with `:error` as the first element, regarldess of the value of the second element.

세 번째 케이스인 `{:error, _}`는 `:error`를 첫 번째 값으로 가진 값 두 개짜리 튜플 전부에 매칭됩니다. 두 번째 값은 무관합니다.

## 패턴 매칭 = 디스트럭쳐링 + 제어 구조

That one pattern matching with 9 lines are doing a lot of things in an incredibly concise way. Let's look at what it's doing.

위 예시에 있는 9줄짜리 패턴 매칭은 매우 간결하게 작성되었지만 여러 기능을 수행하고 있습니다. 어떤 일을 하는지 한 번 살펴봅시다.

First, it's checking the structure of the input data. All three cases match against two-element tuples. 

첫째, 인풋 데이터의 구조를 확인합니다. 세 케이스 모두 값 두 개짜리 튜플에 매치합니다.

Second, it's accessing and checking the values contained in the data. The cases check whether the two-element tuples have `:ok` or `:error` as their first elements.

둘째, 데이터 내부에 접근해서 값을 확인합니다. 케이스를 종합해보면 값 두 개짜리 튜플이 첫 번째 값으로 `:ok`나 `:error`를 가지고 있는지 확인합니다.

Third, it's binding the value it accessed from the data. This happens in the second case, where it's binding the second element of the matched tuple to a variable.

셋째, 접근한 데이터 내부의 값을 다른 변수에 바인드합니다. 두 번째 케이스를 보면 매치된 튜플의 두 번째 값을 변수에 바인드하는 것을 볼 수 있습니다.

These three operations together are simply called `destructuring` or `deconstructing` in FP.

FP에서는 이 세 가지 작업을 묶어서 `디스트럭쳐링(destructuring)` 혹은 `디컨스트럭팅(deconstructing)`이라고 불립니다.

Fourth, it's specifying how to respond to matched cases. Each case is followed by subsequent operations, which have access to the variable you bound through destructuring.

넷째, 매치된 케이스에 어떻게 대응할 지 명시합니다. 각 케이스마다 추후 작업을 지시할 수 있는데, 해당 케이스에서 디스트럭쳐링을 통해서 바인드한 변수를 넘겨받을 수 있습니다.

So pattern matching can specify the data structure you want, extract values from it, then pass those extracted values to other functions. These are operations that you do over and over again to implement more abstract logic. Pattern matching allows an incredibly succint way to efficiently write those codes with little boilerplate code.

정리하자면 패턴 매칭은 원하는 데이터 구조를 명시하고, 그 안에서 값을 추출하고, 추출한 값을 다른 함수에 넘겨줄 수 있습니다. 이런 작업은 추상적인 논리를 코드로 구현할 때 셀 수 없이 자주 작성하게 됩니다. 패턴 매칭은 적은 보일러플레이트 코드를 사용해서 이런 코드를 매우 효율적이고 간결하게 작성할 수 있도록 해줍니다.

But it doesn't stop there.

하지만 이게 다가 아닙니다. 

*If you are interested in a theoretical discussion about OOP and FP regarding pattern matching, I have written an optional section at the end of the post.*

## 코드 정리와 구성을 위한 Multiple Clause Function

A useful syntactic sugar for pattern matching is multiple clause function heads. Actually, the full definition of the `Enum.random/1` function I've introduced above looks like this:

multiple clause function은 패턴 매칭 관련해서 유용한 신택틱 슈가입니다. 앞서 소개한 `Enum.random/1` 함수의 전체 정의는 다음과 같습니다.

```elixir
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
```

The first two lines are type specifications for the function, so they are not relevant for this post. Let's look at the rest of the function definition.

첫 두 줄은 함수의 타입 스펙이며 이 글과는 무관하니 생략하고 나머지 코드를 봅시다.

There are two function definitions with identical names. It is a syntactic sugar to separate pattern matching cases into multiple clause function definition. In fact, the above function definition is equivalent to the following code:

동일한 이름으로 정의된 함수가 두 개 있습니다. 이는 패턴 매칭 케이스를 multiple clause function으로 분리한 신택틱 슈가입니다. 실제로 다음과 같이 작성한 코드도 위의 함수 정의와 동일합니다.

```elixir
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
```

As you can see, the first pattern matching was split into different function heads. So what's so great about this seemingly mundane feature? Multiple clause function heads are great because they provide a visually discernible way to break down functions into smaller parts that are easier to understand and maintain. 

코드를 비교해보면, 가장 상위 단계의 패턴 매칭을 여러 개의 함수 정의로 분리한 것을 명확히 볼 수 있습니다. 얼핏 보기에는 그리 중요하지 않은 이 기능에 어떤 의의가 있을까요? 바로 함수를 시각적으로 명확히 구분할 수 있는 작은 부분들로 함수를 분리할 수 있게 해준다는 것입니다. 코드를 분리하면 읽는 이가 머리 속에 담아두어야 할 코드의 전체 양이 줄어들어서 코드를 이해하고 유지보수하기 더 용이해집니다.

Remember that pattern matching is already being used to extract data, manipulate control flow, and guarantee an output. On top of that, it can be also used for structuring the codebase. 

패턴 매칭을 사용해서 이미 데이터를 디스럭쳐링하고 제어 구조도 다루고 있었다는 것을 생각해보세요. 패턴 매칭을 사용해서 그 뿐만 아니라 코드베이스를 관리하는 일까지 할 수 있습니다.

## 재귀와 패턴 매칭

Let's take a look at another practical example of pattern matching. Recursion is a basic building block of functional programming, and it works seamlessly well with pattern matching. Here's `Enum.reverse/1` function from Elixir core library, which takes an enumerable and returns a list with elements in reverse order.

패턴 매칭을 사용한 실제 예시를 하나 더 살펴봅시다. 재귀는 함수형 프로그래밍의 기본적인 기법으로, 패턴 매칭과 매우 잘 맞습니다. 아래 코드는 엘릭서 코어 라이브러리의 `Enum.reverse/1` 함수로 enumerable을 받은 뒤 리스트 안의 요소를 역순으로 뒤집어서 반환합니다. 

```elixir
@spec reverse(t) :: list
def reverse([]), do: []
def reverse([_] = l), do: l
def reverse([a, b]), do: [b, a]
def reverse([a, b | l]), do: :lists.reverse(l, [b, a])
def reverse(enumerable), do: reduce(enumerable, [], &[&1 | &2])
```

The following code is the same function written without multiple function heads.

다음은 동일한 함수를 multiple clauses function을 사용하지 않고 작성한 코드입니다.

```elixir
@spec reverse(t) :: list
def reverse(enumerable) do
  []         -> []
  [_] = l    -> l
  [a, b]     -> [b, a]
  [a, b | l] -> :lists.reverse(l, [b, a])
  enumerable -> reduce(enumerable, [], &[&1 | &2])
end
```

Notice how multiple function heads make the base case, the first three cases in this example, more discernible. Considering that understanding the base case is crucial to understanding a recursive function, such a visual assistance through pattern matching is definitely helpful.

이 예시에서 재귀 함수의 베이스 케이스는 첫 세 케이스입니다. Multiple clauses function을 사용하면 베이스 케이스가 더 명확히 분리됩니다. 재귀 함수를 이해하려면 베이스 케이스를 이해하는 것이 필수적임을 생각해보면, 패턴 매칭이 이를 시각적으로 도와준다는 것은 도움이 됩니다.

## 주연을 맡은 패턴 매칭

But if you actually think about it, none of the capabilities of pattern matching I've described here is new. 

하지만 곰곰히 생각해보면 여기서 이야기한 패턴 매칭의 기능 중 정말 새로운 것은 없습니다.

Switch statement has existed for decades in imperative languages for manipulating control flow. Destructuring is now supported in OO languages that have more aggressively introduced elements of FP, such as Ruby, Swift, and ES6 JavaScript. Still, pattern matching in those languages have limited power and is relegated to a secondary role because its incompatibility with some OO principles.

스위치문은 명령형 언어에서 프로그램 흐름을 제어하기 위해 오랜 세월 동안 사용되어 왔습니다. 디스럭쳐링은 루비, 스위프트, ES6 자바스크립트 등 FP 요소를 보다 적극적으로 받아들인 OO 언어에서도 이제 지원합니다. 하지만 해당 언어에 구현된 패턴 매칭은 여전히 제한적인 기능만을 수행할 수 있을 뿐 아니라, 일부 OO 원칙 및 방법론과 상충하기 때문에 부차적인 도구로만 사용되고 있습니다.

In Elixir, pattern matching plays the leading role. The entire language feels like it's built around pattern matching, allowing it to show its full potential. And I think that's what makes using pattern matching feel so nice in Elixir. 

엘릭서에서는 패턴 매칭이 주연을 맡습니다. 언어 전체가 패턴 매칭을 위해 만들어진 것처럼 느껴질 정도로 패턴 매칭이 그 기능을 전적으로 발휘할 수 있습니다. 저는 이것이야말로 엘릭서에서 패턴 매칭을 사용하는 것이 즐겁게 느껴지는 이유라고 봅니다.

In practice, this means that you can think about everything in terms of pattern matching. Destructuring? Use pattern matching. Control flow? Use pattern matching. Code organization? Use pattern matching. It creates an uninterrupted flow of writing and organizing code without stopping to think about which tool to use for each particular case. 

이는 실제적으로는 모든 것을 패턴 매칭이라는 관점에서 생각할 수 있다는 의미입니다. 디스터럭쳐링? 패턴 매칭으로 해결. 제어 흐름? 패턴 매칭으로 해결. 코드 구성 정리? 패턴 매칭으로 해결. 덕분에 어떤 상황에서 어떤 도구를 사용해야할 지 고민할 필요 없이, 하나로 연결된 끊임없는 흐름 상에서 코드를 작성하고 관리할 수 있습니다.

## 마무리

The power of pattern matching in Elixir cannot be described just in terms of its own capabilities. It's the harmony between the simple functional elements of the language and the tool that makes it such a pleasant experience. If you haven't tried Elixir, I recommend you to at least take a look at it to see how pattern matching works in it.

패턴 매칭의 기능만으로는 엘릭서에서 패턴 매칭을 사용하는 경험을 온전히 설명할 수 없습니다. 그 경험을 만드는 것은 엘릭서의 FP 요소와 패턴 매칭이라는 도구 사이의 절묘한 조화입니다. 엘릭서를 아직 사용해본 경험이 없다면, 엘릭서에서 패턴 매칭이 어떻게 동작하는지를 경험하기 위해서라도 한 번 해보는 것을 추천합니다.

---

## No Encapsulation and More Explicit Control Flow 

Destructuring gives you a free access into the internal values of data structures, which might look like a total disregard for encapsulation. It is. Except that encapsulation does not exist in FP.

One of the key reasons for encapsulation in OOP is to control who has access to the data. Only the object that holds the data is permitted to read and write those data. Any other entity must request that object to handle the data. 

FP, in contrast, solves the writing permission problem in a completely different way. All data are immutable, so no one can write to them. 

Since there's no more need to control writing permission, there's no need to encapsulate data within objects for access control. So FP separates them into data types and modules. In OOP terms, that could be described as splitting complex objects into simpler data transfer objects and service objects. 

As for reading permission, each FP language handles it differently. Dynamically typed languages, such as Erlang/Elixir and Closure, tend to stick to the types provided by the language. Programmers know how to handle those basic types, which means they know how to interface with almost all data. In other words, programmers have something akin to universal reading permission in those languages.

On the other hand, statically typed languages, such as Haskell/ML family languages, leverage powerful custom types. Programmers create their own types that are suited to the logic they are implementing. So unless you know the interface for those types, you cannot read data from those types. This works as practical reading permission.

Multi-paradigm languages that leverage elements from both OOP and FP paradigms go around the encapsulation problem in different ways. Scala provides extractor objects for defining interfaces for destructuring operation without compromising object encapsulation. Ruby core library provides convenient syntax and methods for destructuring commonly used data objects, but separates control flow from them. Swift limits destructuring to tuples, while supporting Swift-specific features such as unwrapping optionals or validating type casting. ES6 JavaScript provides powerful destructuring functionality, but separates it from control flow.

Pattern matching also allows you to explicitly state how  you want to process the data based on its structure. This might look like manual method dispatch in OOP, which means using control flow statements against the type of an object to choose what to do with it. Such a practice is frowned upon in OOP, which prefers delegating such a choice to objects using techniques like [function overloading](https://www.wikiwand.com/en/Function_overloading) and [subtype polymorphism](https://www.wikiwand.com/en/Subtyping). In contrast, FP prefers explicitly handling this at function level.

Here's one way that could help you wrap your head around this difference. Switch statement in OOP is about instructing what to do in certain situations. This is characteristic of its imperative heritage. In contrast, pattern matching in FP is about establishing rules for handling different cases. This is characteristic of its declarative heritage.

Unlike dynamically typed languages like Elixir, statically typed languages provide additional benefits to pattern matching. Compiler ensures that cases cover all possible inputs, that they are not redundant, and that they do not try to match against impossible data types. The tradeoff is that the code gets more complicated and require programmers to know and understand ever-increasing number of type interfaces.