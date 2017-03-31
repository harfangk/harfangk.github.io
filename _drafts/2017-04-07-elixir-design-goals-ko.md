---
layout: post
ref: elixir-design-goals
date: 2017-02-25 00:00:00 +0900
title: 엘릭서 언어 디자인 목표
lang: ko
---

이 글은 조제 발림이 [엘릭서 언어 공식 블로그](http://elixir-lang.org/blog)에 올린 [Elixir Design Goals](http://elixir-lang.org/blog/2013/08/08/elixir-design-goals/)라는 글의 전문을 번역한 글입니다.

# 엘릭서 언어 디자인 목표

August 08, 2013 · by José Valim . in Internals

During the last year, we have spoken at many conferences spreading the word about Elixir. We [usually started with introducing the Erlang VM](https://vimeo.com/53221562), then went on to talk about Elixir goals, saving some time at the end to do a live demo, showing some goodies like exchanging information between remote nodes and even hot code swapping.

작년 한 해 동안 엘릭서를 소개하기 위해 여러 컨퍼런스에서 발표를 했습니다. [보통은 얼랭 VM을 소개하는 것부터 시작해서](https://vimeo.com/53221562) 엘릭서의 목표에 대해서 이야기하고, 마지막에는 라이브 시연 시간을 마련해서 리모트 노드간 정보를 교환하거나 핫 코드 스와핑 같은 멋진 기능을 보여주었습니다.

This post is a summary of those talks, focusing on the language goals: compatibility, productivity and extensibility.

이 글은 그런 발표를 요약한 글로, 호환성, 생산성 및 확장성이라는 엘릭서 언어 목표를 집중적으로 다룹니다.

## 호환성

Elixir is meant to be compatible with the Erlang VM and the existing ecosystem. When we talk about Erlang, we can break it into three parts:

엘릭서는 얼랭 VM과 기존 생태계와 호환되도록 설계되었습니다. 얼랭은 보통 다음과 같은 세 부분으로 나눌 수 있습니다.

* A functional programming language, called Erlang
* A set of design principles, called OTP
* The Erlang Virtual Machine, referred to as EVM or BEAM

Elixir runs in the same virtual machine and is compatible with OTP. Not only that, all the tools and libraries available in the Erlang ecosystem are also available in Elixir, simply because there is no conversion cost from calling Erlang from Elixir and vice-versa.

* 얼랭이라 불리는 함수형 프로그래밍 언어
* OTP라 불리는 디자인 원칙의 집합
* EVM 또는 BEAM이라 불리는 얼랭 가상 머신

엘릭서는 동일한 가상 머신에서 작동하며 OTP와 호환됩니다. 그뿐 아니라 얼랭 생태계에서 사용 가능한 모든 도구와 라이브러리는 엘릭서에서도 사용가능합니다. 얼랭에서 엘릭서를 호출하는 것과 그 반대 경우 둘 다 전환 비용이 없기 때문입니다.

We frequently say that **the Erlang VM is Elixir’s strongest asset**.

All Elixir code is executed inside light-weight processes (actors), each with its own state, that exchange messages between each other. The Erlang VM multiplexes those processes onto many cores, making it trivial to run code concurrently.

우리는 **얼랭 VM이 엘릭서의 가장 큰 자산이라고 이야기하곤 합니다**.

모든 엘릭서 코드는 각자 자신만의 상태를 가지고 있고, 서로 메시지를 교환하는 가벼운 프로세스(액터) 내에서 실행됩니다. 얼랭 VM은 그런 프로세스들을 여러 코어에 분산해주기 때문에 코드를 병행해서 실행하는 것을 정말 쉽게 만들어 줍니다.

In fact if you compile any Elixir code, including the Elixir source, you will see all cores on your machine being used out of the box. With [technologies like Parallella](http://www.parallella.org/board/) becoming more accessible and affordable, it is hard to ignore the power you can get out of the Erlang VM.

Finally, the Erlang VM was designed to build systems that run forever, self-heal and scale. Joe Armstrong, one of Erlang’s creators, has recently given an excellent talk [about the design decisions behind OTP and the VM](http://www.infoq.com/presentations/self-heal-scalable-system).

Nothing that we are describing here is particularly new. Open source projects like CouchDB, Riak, RabbitMQ, Chef11 and companies like Ericsson, Heroku, Basho, Klarna and Wooga are already enjoying the benefits provided by the Erlang VM, some of them for quite a long time.

실제로 엘릭서 소스 코드 등 어떤 엘릭서 코드를 컴파일해도 기본적으로 머신의 모든 코어를 사용하는 것을 볼 수 있습니다. [Parallella](http://www.parallella.org/board/)같은 기술이 더 저렴해지고 접근성이 증가하는 상황에서 얼랭 VM에서 얻을 수 있는 능력을 무시하기는 어렵습니다.

마지막으로 얼랭 VM은 영원히 동작하고 스스로를 복구하며 스케일하는 시스템을 만들기 위해 디자인되었습니다. 얼랭의 개발자 중 한 명인 조 암스트롱이 [OTP와 얼랭 VM의 디자인적 결정사항](http://www.infoq.com/presentations/self-heal-scalable-system)에 대해서 최근에 훌륭한 발표를 한 적이 있습니다.

## 생산성

> Now we need to go meta. We should now think of a language design as being a pattern for language designs. A tool for making more tools of the same kind. […] A language design can no longer be a thing. It must be a pattern, a pattern for growth. A pattern for growing a pattern, for defining the patterns that programmers can use for their real work and main goals.

* Guy Steele, keynote at the 1998 ACM OOPSLA conference on “Growing a Language”

> 이제 메타적인 주제로 넘어가 봅시다. 우리는 언어 디자인을 언어 디자인에 대한 패턴이라고 생각해야 합니다. 동일한 종류의 도구를 더 만들기 위한 도구 말입니다. [...] 언어 디자인은 어떤 하나의 무엇인가여서는 안 됩니다. 이는 패턴, 즉 성장을 위한 패턴이어야만 합니다. 패턴을 키워내기 위한 패턴이자, 프로그래머들이 자신의 실제 업무와 주 목표를 위해 사용하는 패턴을 정의할 수 있는 패턴이어야 합니다.

* 가이 스틸, 1998 ACM OOPSLA 컨퍼런스 “Growing a Language”라는 기조연설에서

Productivity is, in general, a hard goal to measure. A language productive for creating desktop applications may not be productive for mathematical computing. Productivity depends directly on the field in which you intend to use the language, the available tools in the ecosystem and how easy it is to create and extend those tools.

For this reason, we have opted for a small language core. For example, while some languages have `if`, `case`, `try` and so on as language keywords, each with its own rules in the parser, **in Elixir they are just macros**. This allows us to implement most of Elixir in Elixir and also allows developers to extend the language using the same tools we used to build the language itself, often extending the language to the specific domains they are working on.

생산성은 일반적으로 측정하기 어려운 목표입니다. 데스크탑 어플리케이션을 만들 때 생산적인 언어는 수학적 계산을 할 때는 생산적이지 않을 수 있습니다. 생산성은 당신이 언어를 사용하려고 하는 분야와 생태계 내에서 사용 가능한 도구, 그리고 해당 도구를 얼마나 쉽게 만들고 확장할 수 있는지에 직접적으로 영향을 받습니다.

이런 이유로 우리는 언어의 핵심부를 작게 만들기로 결정했습니다. 예를 들어 어떤 언어에서 `if`, `case`, `try` 등은 파서에서 자신만의 규칙을 가진 언어의 키워드지만, **엘릭서에서는 모두 그냥 매크로일 뿐입니다**. 이를 통해 우리는 엘릭서의 대부분을 엘릭서를 사용해서 구현할 수 있었습니다. 또한 개발자들이 우리가 언어를 만들기 위해서 사용했던 것과 동일한 도구를 사용해서 자신들이 작업하는 특정한 분야에 맞추어 언어를 확장할 수 있도록 해줍니다.

Here is an example of how someone would implement `unless`, which is a keyword in many languages, in Elixir:

여러 언어에서는 키워드인 `unless`를 엘릭서에서 구현하는 예시입니다.

{% highlight elixir %}
defmacro unless(expr, opts) do
  quote do
    if(!unquote(expr), unquote(opts))
  end
end

unless true do
  IO.puts "this will never be seen"
end
{% endhighlight %}

Since a macro receives the code representation as arguments, we can simply convert an `unless` into an `if` at compile time.

Macros are also the base construct for meta-programming in Elixir: the ability to write code that generates code. Meta-programming allows developers to easily get rid of boilerplate and create powerful tools. A common example mentioned in talks is how our test framework uses macros for expressiveness. Let’s see an example:

매크로는 코드 표현을 인자로 받기 때문에 컴파일 타임에 `unless`를 `if`로 간단히 변환할 수 있습니다. 

매크로는 엘릭서 메타프로그래밍, 즉 코드를 생성하는 코드를 작성할 수 있는 기능의 기본적인 구성요소이기도 합니다. 메타프로그래밍은 개발자가 손쉽게 보일러플레이트 코드를 제거하고 강력한 도구를 만들 수 있게 해줍니다. 발표에서 테스트 프레임워크의 표현력을 위해서 매크로를 사용한다는 예시를 자주 사용했었습니다. 한 번 그 예시를 살펴봅시다.

{% highlight elixir %}
ExUnit.start

defmodule MathTest do
  use ExUnit.Case, async: true

  test "adding two numbers" do
    assert 1 + 2 == 4
  end
end
{% endhighlight %}

The first thing to notice is the `async: true` option. When your tests do not have any side-effects, you can run them concurrently by passing the `async: true` option.

Next we define a test case and we do an assertion with the `assert` macro. Simply calling `assert` would be a bad practice in many languages as it would provide a poor error report. In such languages, functions/methods like `assertEqual` or `assert_equal` would be the recommended way of performing such assertion.

In Elixir, however, `assert` is a macro and as such it can look into the code being asserted and infer that a comparison is being made. This code is then transformed to provide a detailed error report when the test runs:

처음 눈에 띄는 것은 `async: true` 옵션입니다. 테스트에 사이드 이펙트가 없으면 `async: true` 옵션을 넣어서 병렬적으로 테스트를 실행할 수 있습니다. 

다음으로 테스트 케이스를 정의하고 `assert` 매크로를 사용해서 어설션을 만듭니다. 그냥 `assert`를 호출하면 별로 좋지 않은 에러 리포트가 나올 것이기 때문에 여러 언어에서 권장되는 방법은 아닐 것입니다. 그런 언어에서는 해당 어설션을 수행할 때 `assertEqual`이나 `assert_equal` 같은 함수나 메서드를 사용하는 것을 권장할 것입니다. 

하지만 엘릭서에서는 `assert`가 매크로이기 때문에 어설션의 대상이 되는 코드를 살펴봐서 비교를 수행한다는 것을 추론할 수 있습니다. 그러면 이 코드는 테스트를 실행하면 구체적인 에러 리포트를 제공할 수 있도록 변환됩니다. 

{% highlight elixir %}
1) test adding two numbers (MathTest)
   ** (ExUnit.ExpectationError)
                expected: 3
     to be equal to (==): 4
   at test.exs:7
{% endhighlight %}

This simple example illustrates how a developer can leverage macros to provide a concise but powerful API. Macros have access to the whole compilation environment, being able to check the imported functions, macros, defined variables and more.

Those examples are just scratching the surface of what can be achieved with macros in Elixir. For example, we are currently using macros to compile routes from a web application into a bunch of patterns that are highly optimizable by the VM, providing an expressive but heavily optimized routing algorithm.

The macro system also caused a huge impact on the syntax, which we will discuss briefly before moving to the last goal.

이 간단한 예는 개발자가 매크로를 사용해서 간결하지만 강력한 API를 제공할 수 있는 방법을 보여줍니다. 매크로는 컴파일 환경 전체에 접근할 수 있기 때문에 임포트된 함수, 매크로, 정의된 변수 등을 확인하고 사용할 수 있습니다.

위 예시는 매크로를 사용해서 엘릭서에서 할 수 있는 것들의 극히 일부분일 뿐입니다. 예를 들어 우리는 매크로를 통해서 표현력이 높지만 고도로 최적화된 라우팅 알고리즘을 제공하고, 이를 사용해서 웹 어플리케이션의 라우트를 VM에서 고도로 최적화할 수 있는 패턴의 집합으로 컴파일하고 있습니다. 

## 문법

Although syntax is usually one of the first topics that comes up when Elixir is being discussed, it was never a goal to simply provide a different syntax. Since we wanted to provide a macro system, we knew that the macro system would only be sane if we could represent Elixir syntax in terms of Elixir’s own data structures in a straight-forward fashion. With this goal in mind, we set out to design the first Elixir version, which looked like this:

문법은 엘릭서에 대해서 이야기할 때 보통 가장 먼저 나오는 주제 중 하나이긴 하지만 우리의 목표는 단순히 다른 문법을 제공하는 것이 아닙니다. 우리는 매크로 시스템을 제공하고 싶었고, 그러기 위해서는 엘릭서 문법을 엘릭서의 자체적인 데이터 구조를 사용해서 명시적으로 표현할 수 있어야만 했습니다. 이를 위해서 우리는 첫 엘릭서 버전을 설계했는데, 다음과 같이 생겼었습니다. 

{% highlight elixir %}
defmodule(Hello, do: (
  def(calculate(a, b, c), do: (
    =(temp, *(a, b))
    +(temp, c)
  ))
))
{% endhighlight %}

In the snippet above, we represent everything, except variables, as a function or a macro call. Notice keyword arguments like `do:` have been present since the first version. To this, we slowly added new syntax, making some common patterns more elegant while keeping the same underlying data representation. We soon added infix notation for operators:

위 코드에서는 변수를 제외한 모든 것을 함수 또는 매크로 호출로 표현했습니다. 첫 버전에서부터 `do:` 같은 키워드 인자가 존재했다는 점에 주목해주세요. 여기에다가 새로운 문법을 천천히 추가해서, 자주 사용하는 패턴을 보다 세련되게 만드는 동시에 그 기반이 되는 데이터적 표현은 동일하게 유지했습니다. 연산자에도 곧 인픽스 표시를 추가했습니다.

{% highlight elixir %}
defmodule(Hello, do: (
  def(calculate(a, b, c), do: (
    temp = a * b
    temp + c
  ))
))
{% endhighlight %}

The next step was to make parentheses optional:

괄호를 선택사항으로 만드는 것이 다음 단계였습니다.

{% highlight elixir %}
defmodule Hello, do: (
  def calculate(a, b, c), do: (
    temp = a * b
    temp + c
  )
)
{% endhighlight %}

그리고 마지막으로 자주 사용되는 `do: (...)` 구성요소를 더 간편하게 쓸 수 있도록 `do/end`를 추가했습니다.

And finally we added `do/end` as convenience for the common `do: (...)` construct:

{% highlight elixir %}
defmodule Hello do
  def calculate(a, b, c) do
    temp = a * b
    temp + c
  end
end
{% endhighlight %}

Given my previous background in Ruby, it is natural that some of the constructs added were borrowed from Ruby. However, those additions were a by-product, and not a language goal.

Many language constructs are also inspired by their Erlang counter-parts, like some of the control-flow macros, operators and containers. Notice how some Elixir code:

제 루비 경력이 있는 만큼 추가된 구성요소 중 일부는 자연스럽게 루비에서 빌려왔습니다. 하지만 그렇게 추가된 부분은 부산물이었지 언어의 목표는 아니었습니다.

언어 구문 중 여럿은 또한 그에 대응되는 얼랭 구성요소에서 따왔습니다. 제어 흐름 매크로, 연산자, 컨테이너 등이 그 예시입니다. 일부 엘릭서 코드가 어떻게 얼랭에 대응하는지 보세요.

{% highlight elixir %}
# A tuple
tuple = { 1, 2, 3 }

# Adding two lists
[1, 2, 3] ++ [4, 5, 6]

# Case
case expr do
  { x, y } -> x + y
  other when is_integer(other) -> other
end
{% endhighlight %}

{% highlight erlang %}
% A tuple
Tuple = { 1, 2, 3 }.

% Adding two lists
[1, 2, 3] ++ [4, 5, 6].

% Case
case Expr of
  { X, Y } -> X + Y;
  Other when is_integer(Other) -> Other
end.
{% endhighlight %}

## 확장성

By building on top of a small core, most of the constructs in the language can be replaced and extended as required by developers to target specific domains. However, there is a particular domain that Elixir is inherently good at, which is building concurrent, distributed applications, thanks to OTP and the Erlang VM.

Elixir complements this domain by providing a standard library with:

작은 핵심부에만 기반을 두고 만들어진 언어이기 때문에 언어 구성요소의 대부분을 개발자들이 목표로 하는 구체적인 분야에 따라 필요한 대로 교체하고 확장할 수 있습니다. 하지만 엘릭서가 본질적으로 뛰어난 특정 분야가 있는데, 이는 병렬 분산 어플리케이션을 만드는 분야입니다. OTP와 얼랭 VM 덕분입니다.

엘릭서는 다음과 같은 특징을 가진 표준 라이브러리를 제공하여 이 분야를 더욱 보완합니다.

* Unicode strings and unicode operations
* A powerful unit test framework
* More data structures like ranges, including novel implementations for sets and dictionaries
* Polymorphic records (in contrast to Erlang’s compilation-time only records)
* Strict and lazy enumeration APIs
* Convenience functions for scripting, like working with paths and the filesystem
* A project management tool to compile and test Elixir code

And much more.

* 유니코드 문자열 및 유니코드 연산
* 강력한 유닛 테스트 프레임워크
* 새롭게 구현한 셋과 딕셔너리, 그리고 레인지 같은 추가적인 데이터 구조
* (얼랭의 컴파일 타임에 한정된 레코드와 대비되는) 폴리모픽 레코드 
* 스트릭트 및 레이지한 열거 API
* 경로나 파일시스템 등 스크립팅 용 편의성 함수
* 엘릭서 코드를 컴파일하고 테스트할 수 있는 프로젝트 관리 툴

그리고 그 외에도 더 있습니다.

Most of the features above provide their own extensibility mechanisms, too. For example, take the `Enum` module. The `Enum` module allow us to enumerate the built-in ranges, lists, sets, etc:

위에 언급된 기능 대부분에는 자체적인 확장 메커니즘이 있습니다. 예를 들어 `Enum` 모듈을 봅시다. `Enum` 모듈은 내장된 레인지, 리스트, 셋 등을 열거할 수 있도록 해줍니다.

{% highlight elixir %}
list = [1, 2, 3]
Enum.map list, fn(x) -> x * 2 end
#=> [2, 4, 6]

range = 1..3
Enum.map range, fn(x) -> x * 2 end
#=> [2, 4, 6]

set = HashSet.new [1, 2, 3]
Enum.map set, fn(x) -> x * 2 end
#=> [2, 4, 6]
{% endhighlight %}

Not only that, any developer can **extend** the `Enum` module to work with any data type as long as the data type implements [the `Enumerable` protocol](https://hexdocs.pm/elixir/Enumerable.html) (protocols in Elixir are based on Clojure’s protocol). This is extremely convenient because the developer needs to know only the `Enum` API for enumeration, instead of memorizing specific APIs for sets, lists, dicts, etc.

There are many other protocols exposed by the language, like [the `Inspect` protocol](https://hexdocs.pm/elixir/Inspect.html) for pretty printing data structures and [the `Access` protocol](https://hexdocs.pm/elixir/Access.html) for accessing key-value data by key. By being extensible, Elixir ensures developers can work **with** the language, instead of **against** the language.

그 뿐 아니라 모든 개발자는 어떤 데이터 타입이건 [`Enumerable` 프로토콜](https://hexdocs.pm/elixir/Enumerable.html)을 구현하기만 하면 `Enum` 모듈을 **익스텐드**해서 해당 데이터 타입에 사용할 수 있습니다. 개발자가 셋, 리스트, 딕셔너리용 개별적 API를 외우는 대신 `Enum` API만 알면 열거를 할 수 있기 때문에 매우 간편한 방식입니다.

언어에서 제공하는 프로토콜은 이 외에도 더 많이 있습니다. 데이터 구조를 보기 좋게 출력해주는 [`Inspect` 프로토콜](https://hexdocs.pm/elixir/Inspect.html)도 있고, 키를 사용해서 키-밸류 데이터에 접근할 수 있는 [`Access` 프로토콜](https://hexdocs.pm/elixir/Access.html)도 있습니다. 엘릭서는 확장이 가능하기 때문에 개발자가 언어와 **싸우는** 대신 **협력해서** 일할 수 있도록 해줍니다.

## 요약

The goal of this post was to sumarize the language goals: compatibility, productivity and extensibility. By being compatibile with the Erlang VM, we are providing developers another toolset for building concurrent, distributed and fault-tolerant systems.

We also hope to have clarified what Elixir brings to the Erlang VM, in particular, meta-programming through macros, polymorphic constructs for extensibility and a data-focused standard library with extensible and consistent APIs for diverse types, including strict and lazy enumeration, unicode handling, a test framework and more.

Give Elixir a try! You can start with our [getting started guide](http://elixir-lang.org/getting-started/introduction.html), or check out our sidebar for other learning resources.

이 글의 목적은 호환성, 생산성, 확장성이라는 언어 목표를 요약하는 것이었습니다. 얼랭 VM과 호환성을 유지함으로써 개발자들에게 병행, 분산, 장해 허용 시스템을 만들 수 있는 또다른 툴셋을 제공합니다.

또한 이 글에서 엘릭서가 얼랭 VM에 무엇을 제공하는지 명확히 전달했기를 바랍니다. 구체적으로는 매크로를 통한 메타프로그래밍, 확장성을 위한 폴리모픽 구성요소, 다양한 타입에 대한 확장성 있고 일관적인 표준 라이브러리입니다. 여기에는 스트릭트 및 레이지 열거, 유니코드 처리, 테스트 프레임워크 등도 포함됩니다.

엘릭서 한 번 해보세요! 공식 [엘릭서 시작 가이드](http://elixir-lang.org/getting-started/introduction.html)로 시작하거나, 아니면 사이드바에서 타 학습자료를 확인해볼 수 있습니다. 