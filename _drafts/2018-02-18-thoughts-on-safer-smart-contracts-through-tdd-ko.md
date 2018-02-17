---
layout: post
ref: smart-contracts-with-tdd
date: 2017-02-18 00:00:00 +0900
title: Safer Smart Contracts Through Type-Driven Development - 감상문
lang: ko
---

## 서문

Last year, I learned of the Idris language and wrote a [blog post](https://harfangk.github.io/2017/10/24/tdd-with-idris-review.html) after reading a book on it. Coincidentally, I joined a company that worked on blockchain technology almost immediately after publishing that post. So when I came across a paper titled ["Safer smart contracts through type-driven development: Using dependent and polymorphic types for safer development of smart contracts"](https://publications.lib.chalmers.se/records/fulltext/234939/234939.pdf), which combined two topics of my interest, I just had to read it.

작년에 이드리스 언어에 관한 책을 한 권 읽고 [블로그 글을](https://harfangk.github.io/2017/10/24/tdd-with-idris-review-ko.html) 작성한 적이 있습니다. 그리고 그 글을 쓴 지 얼마 되지 않아 우연히 블록체인 기술을 다루는 회사에 입사해서 일을 하기 시작했습니다. 그러니 우연히도 제가 최근 흥미를 가진 두 가지 주제를 함께 다룬 ["Safer smart contracts through type-driven development: Using dependent and polymorphic types for safer development of smart contracts"](https://publications.lib.chalmers.se/records/fulltext/234939/234939.pdf)라는 논문에 대해서 알게 되자 꼭 읽어야겠다는 생각을 했습니다.

This post is a collection of the thoughts that came across my mind while I was reading the paper.
There won't be any new idea that builds on the contents of the paper. I assume basic understanding
of functional programming paradigm, in particular the distinction between pure functions and side
effects.

이 글은 해당 논문을 읽으면서 가지게 된 생각을 정리한 글입니다. 논문 내용에서 더 발전한 내용은 없습니다. 함수형 프로그래밍 패러다임, 특히 순수 함수와 사이드 이펙트 사이의 차이점에 대한 기본적인 이해를 가정하고 작성했습니다.

## 런타임에서 했을 연산을 타입을 활용해 컴파일 타임에서 하기

In blockchain industry, guaranteeing the correctness of smart contracts is one of the key areas for
improvement. As the industry grows, values moved through smart contracts would continue to grow as
well, which also means that the damage caused by faulty smart contracts would also increase in
proportion. As I learned more and more about smart contracts, I started to think that statically
typed functional languages would be great in this domain, since they are well-known for minimizing
run-time errors. It is no coincidence that financial institutions use such languages for formal
description of contracts. I've also personally experienced how type checking at compile time can
help prevent certain kinds of common errors such as null pointer exception.

스마트 컨트랙트가 올바르게 작성되었는지를 검증하는 것은 블록체인 업계에서 가장 개선이 절실한 분야 중 하나입니다. 업계의 규모가 커질수록 스마트 컨트랙트를 통해서 처리되는 금액 또한 증가할 것이고, 잘못된 스마트 컨트랙트 때문에 발생하는 피해액 또한 그에 비례해 증가할 것입니다. 스마트 컨트랙트의 현재 상황에 대해 알게 될수록 정적 타입 함수형 언어야말로 이 분야에 적합한 언어라는 생각을 하기 시작했습니다. 그 계통의 언어는 런타임 에러가 거의 발생하지 않는 것으로 유명하니까요. 금융 기관에서 유달리 그런 언어를 많이 쓰는 것에는 이유가 있습니다. 개인적으로도 엄격한 컴파일 타임 타입 체크가 널 포인터 에러 등 특정한 종류의 흔한 에러를 방지하는 것을 경험해 보았습니다.

The authors of the paper, Jack Pettersson and Robert Edström, had the same idea and decided to
explore it. Their thesis goes: "Our thesis is that a functional language with an extensive type
system can allow safer development of smart contracts."

저자인 Jack Pettersson와 Robert Edström 또한 같은 생각을 가지고 이를 연구해보았습니다. "우리의 논지는 방대한 타입 시스템을 가진 함수형 언어가 스마트 컨트랙트를 보다 안전하게 개발하도록 해줄 수 있다는 것이다."

After reading through their work, I became more confident that statically typed programming language
would be excellent for writing robust smart contracts. Human mind is incredibly powerful and
flexible but is also more prone to errors - especially when we are tired or distracted. For
reliability, we need to leverage the assistance of computers.

논문을 읽고 나서 정적 타입 프로그래밍 언어가 스마트 컨트랙트를 쓰는데 매우 적합하리라는 심증을 더욱 강하게 가지게 되었습니다. 인간의 정신은 매우 강력하고 유연하지만 실수 또한 잦으며, 이런 단점은 피곤하거나 주의력이 떨어졌을 때 더욱 두드러집니다. 신뢰성을 위해서는 컴퓨터를 잘 활용해야 합니다.

## 타입을 사용하여 사전조건 및 사후조건 표현

제가 논문에서 가장 흥미롭게 본 것은 아래 코드입니다.

```haskell
finalize : { auto p: LTE 20 b } -> Eff Int 
           [ STORE, ETH 0 b 0 0 ]
           (\winner => if winner == 0 
                          then [ STORE, ETH 0 b 0 0 ] 
                          else [ STORE, ETH 0 b 20 0 ])
```

It's a type signature for a function called `finalize`, which interprets the final result of
a rock-paper-scissors game where each player bets 10 Ethereum and the winner takes it all. This
type signature provides a ton of information, and could be hard to understand if you are not familiar with this kind of language.

`finalize`라는 함수의 타입 표기입니다. 이 함수는 두 사람이 각자 10 이더씩 걸고 참가하는 가위바위보 경기의 최종 결과를 판단하는 함수입니다. 승자는 20이더를 모두 챙겨가죠. 이 타입 표기에는 정말 많은 정보가 담겨 있기 때문에 이런 종류의 언어에 익숙하지 않다면 이해하기 어려울 수도 있습니다. 

Here's what each part means:

각 부분의 의미는 이러합니다.

`{ auto p: LTE 20 b }` part supplies a proof (`p` stands for proof) to the Idris compiler that `20`
is less than or equal to `b`, which stands for balance here. So based on the proof, the compiler can
assume that `b` is larger than `20` when the function is called. This ensures at compile time that
no overdraft would happen.

`{ auto p: LTE 20 b }`는 `20`이 `b`보다 작거나 같다는 증명을 이드리스 컴파일러에게 전달합니다. 여기서 `b`는 잔고(balance)를 나타내고 `p`는 증명(proof)을 나타냅니다. 컴파일러는 이 증명에 의거하여 이 함수가 호출될 때 `b`는 반드시 `20`보다 크다고 가정합니다. 그러면 잔고 이상의 금액을 전송하지 않는다는 것을 컴파일 타임에 보장할 수 있습니다.

`EFFECT` is the type Idris uses for describing side effects. You can find
a [tutorial](http://docs.idris-lang.org/en/latest/effects/) for `Effects` library at the official
Idris website. Note that currently another module is recommended over `Effects`, as stated in the above link: 
> Unless you have a particular reason to use Effects you are strongly recommended to use `Control.ST` instead.

`EFFECT`는 사이드 이펙트를 표현하기 위한 이드리스의 타입입니다. `Effects` 라이브러리에 관해서는 이드리스 공식 사이트에서 [튜토리얼](http://docs.idris-lang.org/en/latest/effects/)을 제공합니다. 단, 사이트를 보면 알겠지만 현재는 `Effects`가 아니라 다른 모델을 사용할 것을 권장합니다.
> Effects를 사용해야 할 특별한 이유가 있지 않은 이상 대신 `Control.ST`를 사용할 것을 강권합니다.

Here `Eff` is a constructor for `EFFECT` type that takes three arguments: a value of `Int`, a list of `Effect` values, and a function of
`Int -> List Effect`. 

`Eff`는 `EFFECT` 타입의 타입 컨스트럭터로, `Int` 타입의 값, `Effect` 값의 리스트, 그리고 `Int -> List Effect` 타입의 함수라는 세 개의 인자를 받습니다.

The first argument represents the return value of this function. For `finalize`, it's `Int` type. 

첫 번째 인자는 이 함수의 결과값을 나타냅니다. `finalize`의 경우 `Int` 타입이죠.

The second argument is called `input effects`. It means that this function can access effects `STORE` and `ETH 0 b 0 0` when it is called. 

두 번째 인자는 `인풋 사이드 이펙트`라고 불리는데, 이 함수가 호출되었을 때 `STORE`와 `ETH 0 b 0 0` 이펙트를 사용할 수 있다는 것을 의미합니다.

The third argument is called `output effects`. It is a function that takes the first argument and determines which side effect `finalize` should have between `[ STORE, ETH 0 b 0 0 ]` and `[ STORE, ETH 0 b 20 0 ]`. 

세 번째 인자는 `아웃풋 사이드 이펙트`라고 불립니다. 이는 첫 번째 인자를 바탕으로 `finalize` 함수가 `[ STORE, ETH 0 b 0 0 ]`와 `[ STORE, ETH 0 b 20 0 ]` 중에서 어떤 사이드 이펙트를 가질 지를 결정하는 함수입니다.

What's significant is that the type signature describes not only the function's output, but
also what kind of side effect it should have depending on the result of `finalize`. This gives a powerful tool for explicitly handling side effects, such as network communication, by enforcing their pre-condition side effects and post-condition side effects as types.

결과값 뿐만 아니라, 그 결과값에 따라서 해당 함수가 어떤 사이드 이펙트를 가지게 될 지도 타입 표기를 통해 표현할 수 있다는 사실이 정말 놀랍습니다. 이를 활용하면 사전 사이드 이펙트와 사후 사이드 이펙트를 타입을 사용해 명시하여, 함수에서 발생하는 네트워크 통신 같은 사이드 이펙트도 컴파일 타임에 명확히 표현하고 다룰 수 있게 됩니다.

## AST 변환의 원리

As a self-taught web developer, I lack knowledge in low-level areas such as hardware, assembly,
compiler, or operating systems, so those areas still remain mythical "here be dragons" territory to
me. So when the writers described the process of transforming the AST output of Idris to the AST of
Serpent, a language used in Ethereum Virtual Machine, I was surprised to see how straightforward it
was. In principle, I just needed to write derivarion rules for mapping from one AST to another! Of
course it would never be that simple, but I felt like I gained a glimpse into the process.

저는 주로 웹 개발을 독학했기 때문에 하드웨어, 어셈블리, 컴파일러, 운영체계 등의 로우레벨 분야에 대해서는 많이 알지 못합니다. 그래서 해당 분야는 항상 신비로운 미지의 땅처럼 느껴졌습니다. 그래서 논문의 저자들이 이드리스의 AST를 서펜트라는 이더리움 가상 머신에서 사용하는 언어의 AST로 변환하는 과정을 설명했을 때 그 과정이 생각보다 복잡하지 않아서 놀랐습니다. 기본 원리만 보면 한 AST에서 다른 AST로 변환하는 규칙만 작성하면 되는 것이었으니까요. 물론 실제로 해보면 그렇게 간단하진 않겠지만 그 과정에 대해서 어느 정도 이해하게 된 것 같아서 좋았습니다.

## 함수형 프로그래밍은 분산된 노드끼리 메시지를 주고 받는 것을 표현할 수 없다

In the discussion section, the authors express their disappointment with the functional
paradigm:

마지막 논의 부분에서 저자들은 함수형 패러다임에 실망한 이유를 이야기합니다.

> When writing smart contracts in our Idris implementation, most of the critical functionality has
> to exist in effectful rather than pure functions. This is because pure functions have no notion of
> communication, but Ethereum’s execution model is based entirely on messages that are sent between
> accounts, which it has in common with all smart contract platforms we are aware of. Furthermore,
> pure functions don’t directly encode program state, which is another important aspect of smart
> contract platforms.

> 이드리스 구현체로 스마트 컨트랙트를 작성할 때 대부분의 핵심 기능은 순수 함수가 아니라 사이드 이펙트를 가진 함수로 표현해야 했습니다. 이는 순수 함수에는 통신에 관한 개념이 없기 때문입니다. 하지만 이더리움의 실행 모델은 전적으로 계정끼리 주고받는 메시지에 기반을 두고 있고, 이는 우리가 알고 있는 모든 스마트 컨트랙트 플랫폼이 공통적으로 가지는 특성입니다. 또한 순수 함수는 프로그램의 상태를 직접 표현하지 않는데, 상태를 표현하는 것 또한 스마트 컨트랙트 플랫폼의 중요한 측면입니다. 

Since I've also thought that functional paradigm would be a great fit, I was surprised by the result
as the authors were. But they seem to be correct; if a model fails to represent key components of a domain,
it might not be the right model. The authors suggest that process calculus might be a better fit for
this domain. Since I've never heard of it, I had to look it up. It sounded related to the actor model that I got familiar through Erlang and Elixir, and does seem like a better model to formally represent complex network communications. I should definitely look into it later.

저도 함수형 패러다임이 이 분야에 매우 적합할 것이라고 생각했기 때문에 예상치 못한 결과에 저자들 만큼이나 놀랐습니다. 하지만 기본적으로 저자들의 관점이 옳다고 봅니다. 어떤 모델이 특정 분야의 핵심 요소를 표현해내지 못한다면 해당 분야에 적합한 모델이 아닐 수도 있습니다. 저자들은 [프로세스 캘큘러스(process calculus)](https://www.wikiwand.com/en/Process_calculus)가 더 적합할 수도 있다고 제시했는데, 처음 들어본 개념이기 때문에 한 번 찾아보았습니다. 얼랭과 엘릭서를 다루면서 배우게 된 액터 모델과 유사해 보이는데, 복잡한 네트워크 통신을 정식으로 표현하기에는 더 적합한 모델로 보이긴 했습니다.

## 디펜던트 타입 사이드 이펙트의 표현력

The authors note that a combination of dependent types and explicit side effect management of Idris
were essential for writing smart contracts in Idris. They wrote a sample implementation as part of
the paper, which was quite interesting to read.

저자들은 이드리스로 스마트 컨트랙트를 작성할 때 디펜던트 타입과 명시적인 사이드 이펙트 관리라는 이드리스의 두 특징 모두 핵심적이었다고 평가했습니다. 논문의 내용으로 이를 구현한 예시 코드 또한 제공하는데 매우 흥미롭게 읽었습니다. 

Recently I have been working extensively with Elm, and I think I get what the authors mean. Elm
also provides explicit management of side effects through `Cmd` and `Sub` types, but they are not as
descriptive as I want them to be. I know that it's in accordance with Elm's design philosophy, and
it does make Elm code much easier to approach and understand. But sometimes I do wish
that Elm had more robust type system so that I could be more expressive.

최근에는 주로 엘름으로 프로그램을 짜고 있는데, 저자들이 두 특징 모두 핵심적이었다고 할 때 무슨 의미로 말하는 것인지 알 것 같습니다. 엘름 또한 `Cmd`와 `Sub` 타입을 통해 명시적으로 사이드 이펙트를 관리하지만 제가 원하는 것보다 표현력이 부족할 때가 있습니다. 이는 엘름의 설계 철학에 따른 것이고, 덕분에 엘름 코드가 매우 읽고 이해하기 쉽다는 점은 잘 알고 있습니다. 하지만 엘름의 타입 시스템이 보다 강력해서 더 많은 것을 표현할 수 있으면 좋겠다는 생각이 가끔 드는 것은 어쩔 수 없습니다.

For example, let's think about how to express a part of the above snippet in Elm. The goal is: I want
to make sure that people cannot send more Ethereum than they own. Elm uses `Cmd Msg` type to
describe side effect, where `Msg` type is usually defined as a union type. Let's say it's defined as
`type Msg = SendEth Int | DepositEth Int`. If my Ethereum balance is 50, I shouldn't be able to
execute `Cmd (SendEth i)` when `i` is greater than 50. Because the value of `i` is determined at
run-time, the validity of this side effect can be checked only at run-time.

예를 들어 위의 코드의 일부를 엘름으로 표현해본다고 생각해봅시다. 목표는 보유하고 있는 이더리움 양 이상을 전송할 수 없게하는 것입니다. 엘름은 `Cmd Msg`타입을 사용해서 사이드 이펙트를 표현하는데, 이 때 `Msg` 타입은 보통 유니언 타입으로 표현합니다. 여기서는 `type Msg = SendEth Int | DepositEth Int`라고 정의해봅시다. 이더리움 잔고가 50이더라면 `i`가 50보다 클 경우에는 `Cmd (SendEth i)`를 실행할 수 없어야만 합니다. 이 때 `i`의 값은 런타임에 결정되므로 이 사이드 이펙트가 올바른지는 런타임에만 확인할 수 있습니다.

Idris can check this at compile time thanks to its dependent types. Because `ETH` is
defined as a dependent type, `ETH 0 b 0 0` and `ETH 0 b 20 0` in the above snippet are computed
as different types as they receive different values for the third argument. And because
effects are distinguished at type level, the compiler can guarantee that only valid side effects will be
executed. Such guarantee at compile time is impossible in Elm's type system.

반면 이드리스는 디펜던트 타입 덕분에 컴파일 타임에 이를 확인할 수 있습니다. `ETH`는 디펜던트 타입이기 때문에 세 번째 인자가 서로 다른 `ETH 0 b 0 0`와`ETH 0 b 20 0`는 서로 다른 타입으로 계산됩니다. 그리고 컴파일러가 타입 수준에서 사이드 이펙트를 구분할 수 있기 때문에 이 프로그램에서는 올바른 사이드 이펙트만 실행한다는 것을 컴파일 타임에 검증해줄 수 있습니다. 엘름의 타입 시스템에서는 컴파일 타임에 이를 보장하는 것은 불가능합니다. 

## 맺음말

I enjoyed Idris codes since they showed somewhat realistic example of how to write an Idris program. In particular, it was amazing to see how Idris can be used to specify not just the input and output types, but also the input and output side effects. Idris keeps expanding my horizon about what is possible in programming - I look forward to learning more of it.

조금 현실적인 용례에 대한 이드리스 프로그램을 볼 수 있어서 즐거웠습니다. 특히 인풋과 아웃풋의 타입 뿐 아니라 인풋 사이드 이펙트와 아웃풋 사이드 이펙트의 타입도 명시할 수 있다는 점은 정말 놀라웠습니다. 이드리스를 볼 때마다 제가 생각하던 프로그래밍의 한계를 넘어설 수 있기 때문에 앞으로도 더 많이 배워보고 싶습니다.
