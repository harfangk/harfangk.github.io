---
layout: post
ref: understanding-computation-review
date: 2017-08-17 00:00:00 +0900
title: Understanding Computation 리뷰
lang: ko 
---

## 요약

If you are a software developer without computer science background who want
a friendly introduction to computer science, I highly recommend this book.

소프트웨어 개발자 중에서 컴퓨터 과학을 정식으로 배우지 않았지만 관심이 있어서
배워보고 싶은 분들께 이 책을 추천합니다.

This book explores some of core concepts of computer science, specifically
syntax and semantics of programming languages and basic abstract machines that
can execute programs. The author writes in a concise, easy-to-read, and
lighthearted style, using only codes that you would write at your daily job as
examples. It's much easier to read than a typical academic writing that's full
of bizarre mathematical symbols.

이 책은 컴퓨터 과학의 개념 중에서 프로그래밍 언어의 문법과 의미, 그리고
프로그램을 실행할 수 있는 기본적인 추상 기계를 다룹니다. 글이 간결하면서도 쉽고
재밌게 읽을 수 있도록 쓰여 있는데, 직장에서 매일 업무시간에 직접 작성할 법한
코드를 예시로 써서 개념을 설명합니다. 이런 주제를 다루는 책은 보통 기묘한
수학적 기호로 가득한데, 그에 비하면 정말 눈에 쏙쏙 들어옵니다. 

## 이 책만의 장점

What I found most impressive was its easy-to-read style. I could just read
through the book because the author intentionally avoided using convoluted
expressions or sentence structure. In contrast, when I read theoretical books,
I usually have to go back and forth between pages to keep up with the content.
He also stays quite down-to-earth and oddly humorous, living up to the British
stereotype. For example, after introducing a set of massive mathematical
notations that takes up a whole page, he follows up with these words:

이 책에서 가장 특별한 점은 읽기가 매우 편하다는 것입니다. 저자가 복잡한
표현이나 낯선 전문용어를 최대한 쓰지 않으려 노력했기 때문에 글을 읽다가 멈춰서
생각해볼 일이 적습니다. 반면 다른 이론서적을 볼 때는 내용을 이해하기 위해서
계속 책의 앞뒤를 왔다갔다해야 하는 경우가 많습니다. 또한 영국인답다고 해야할지,
매우 견실하게 글을 쓰면서도 오묘한 유머감각을 잊지 않고 있습니다. 예를 들어
간단한 프로그래밍 언어의 의미를 수학적으로 표현한 예시를 한 쪽 가득하게 보여준
뒤에 다음과 같이 적고 있습니다.

> Mathematically speaking, this is a set of inference rules that defines
a reduction relation on SIMPLE’s abstract syntax trees. Practically speaking,
it’s a bunch of weird symbols that don’t say anything intelligible about the
meaning of computer programs.

> 수학적으로 보자면 이는 SIMPLE 언어의 추상구문트리를 축약 방법을 정의해주는
> 추론 규칙을 모아놓은 것입니다. 현실적으로 보자면 이는 전혀 이해할 수 없는
> 방식으로 컴퓨터 프로그램의 의미를 설명해놓은 이상한 기호 덩어리일 뿐입니다.

The book also provides concrete examples written in actual Ruby code for each
abstract concept it introduces. I think this is what sets this book
apart from other books on computer science. Source code is the language
that software developers, the intended target audience of this book, are most
comfortable with. Those examples demonstrate abstract concepts in a way that
software developers can most easily understand.

그리고 이 책은 추상적 개념을 소개할 때마다 실제 루비 코드로 작성된 구체적인
구현 예시를 매번 제시합니다. 이 점이야말로 이 책이 컴퓨터 과학에 대한 책 중에서
돋보이는 이유라고 생각합니다. 소스 코드는 이 책의 대상 독자인 소프트웨어
개발자들이 가장 편하게 다룰 수 있는 언어입니다. 코드를 사용한 예제를
사용함으로써 소프트웨어 개발자들이 가장 쉽게 이해할 수 있는 방식으로 추상적
개념을 실제로 보여주는 것입니다.

For example, this is one of the code implementations the author provides when
he explains the concept of operational semantics and how a program gets
evaluated and reduced to its final value:

예를 들어 저자가 operational semantics 개념을 설명면서 프로그램이 최종적인
결과값에 도달할 때까지 평가되고 축약되는 과정을 보여줄 때, 다음과 같은 방식으로
구현된 코드를 매번 제시합니다.

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
brilliant and easy to understand. Here's the demonstration:

더 복잡한 개념도 예시를 들어 설명합니다. 책의 마지막 부분에서 정지 문제(halting
problem)을 다루는데 이 문제도 추상적인 수학적 상황을 제시하는 대신 루비
프로그램을 사용해서 설명합니다. 정말 비상하면서도 이해하기 쉬운 설명 방법이라고
생각하는데, 다음과 같은 방식으로 설명합니다.

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

> 이 코드는 표준 입력(standard input)에서 프로그램을 읽고, 해당 프로그램이
> 프로그램 자신을 인자로 받아 실행했을 때 정지하는지 확인한 뒤 그와 반대되는
> 동작을 취합니다. 프로그램임 정지하면 do_the_opposite.rb 는 무한루프에
> 들어가고, 프로그램이 무한루프에 들어가면 do_the_opposite.rb 는 정지합니다.
> 그럴 경우 ruby do_the_oppsite.rb < do_the_opposite.rb 를 실행하면 어떤 결과가
> 나올까요? 앞서 does_it_say_no.rb 예시에서 보았던 것과 마찬가지로 해결
> 불가능한 모순에 빠집니다.

Explaining a new concept in a easy to understand manner is already an
impressive feat. Writing a whole book that introduces numerous complex concepts
while still keeping it easy to read is definitely worthy of applause. So
I cannot but appreciate the author's meticulous effort at catering to the
intended audience of his book.

새로운 개념을 이해하기 쉽게 설명하는 것도 쉬운 일이 아닌데, 복잡한 개념 다수를
이해하기 쉽게 설명하는 책을 쓴다는 것은 분명히 대단한 일입니다. 책의 대상
독자에게 맞추어 글을 쓰기 위해 많은 노력을 기울였을 작가에게 감사드립니다. 

## 책에서 다루는 주제

The book covers a range of topics, mostly those from programming language
theory and models of computation. Interestingly, the book has a kind of
narrative structure instead of simply listing technical concepts.

이 책은 주로 프로그래밍 언어 이론과 연산 모델 관련 개념을 다루는데, 
기술적인 개념을 기계적으로 소개하는 것이 아니라 일종의 서사 구조를 갖추고
있습니다. 

At first it briefly introduces various ways for understanding the meaning of
a program, such as operational semantics or denotational semantics. Then it
describes simple abstract machines, such as finite automata or pushdown
automata, that can execute those programs, while explaining how to parse the
programs into a format that machines can execute.

처음에는 operational semantics나 denotational semantics 같은 프로그램의 의미를
이해하기 위한 방법론을 소개합니다. 이어서 프로그램을 실행할 수 있는 유한
오토마타(finite automata)와 푸시다운 오토마타(pushdown automata) 같은 기본적인
추상 기계를 설명하고, 이런 기계가 실행할 수 있는 형식으로 프로그램을 파싱하는
방법을 다룹니다.

Afterwards the book introduces increasingly powerful abstract machines,
culminating in a universal Turing machine. Then it explores properties of
universal Turing machine by comparing it with other universal systems like
lambda calculus, cyclic tag system, or iota language. Finally it analyzes the
limitations of universal Turing machines and introduce undecidable problems
that cannot be generally solved by universal Turing machine, which is the most
powerful computer we can currently build.

다음으로 더 강력한 추상 기계를 점차적으로 소개하면서 마지막에 범용 튜링
기계(universal Turing machine)을 다루고, 람다 대수(lambda calculus), 순화 태그
시스템(cyclic tag system), 이오타 언어(iota language) 등의 다른 범용 시스템과
범용 튜링 기계의 특성을 비교합니다. 마지막으로 현재 알려진 컴퓨터 중에서 가장
강력한 기계인 범용 튜링 기계로도 일반적인 해답을 제시할 수 없는 결정 불가능한
문제(undecidable problems)를 소개하면서 범용 튜링 기계의 한계를 다룹니다.

That's a lot of complex topics to cover in a book that's slightly longer than
300 pages. Still, because of the way the author lays down it all in the book,
it's actually an enjoyable experience.

300쪽을 조금 넘는 책에서 다루기에는 내용이 좀 많아보이는데 저자가 글을
읽기 편하게 썼기 때문에 꽤나 즐겁게 읽을 수 있을 것입니다.

## 내가 배운 것들

### 프로그램의 의미

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

소프트웨어 개발, 구체적으로는 웹 개발을 시작한 지 얼마 안되었기 때문에 매일
실력을 키우기 위해서 공부하고 있습니다. 그래도 공부하면 공부할수록 오히려
배워야할 것들은 더 늘어나더군요. 수평적으로는 방법이 너무나 다양하고 계속
늘어나기만 합니다. 새로운 언어, 새로운 자바스크립트 프레임워크, 새로운 AWS 배포
방법, 새로운 API 작성법 등등 끝이 없습니다. 수직적으로는 구층지옥보다 깊이
내려가는 추상화 레이어가 있죠. 고급 언어로 코드를 작성하고, 그게 파싱되고, 그게
바이트코드로 컴파일되고, 머신코드로 변환되고, CPU에서 실행되고, CPU는
또 게이트를 사용해서 만들어졌고 등등 끝이 없습니다. 오늘만 해도 TCP/IP 밑에는
OSI 7 계층이 있다는 것을 새로 알게 되었는데, 이 분야에는 구층지옥도 모자란
것 같습니다.

Because I was overwhelmed by the number of things to learn, I couldn't even
think about what all these things aim to achieve in the end. The book taught me
that they could be abstracted away as implementation details, and that there
exists a bigger context, that is, the meaning and intention of a program.
Without thinking about that ultimate goal, there was no way I could fully
understand the goals and structures of these individual pieces of technology.

이렇게 배울 것이 너무나 많다보니 이 모든 것이 결국 어떤 목적을 위해서
존재하는지에 대해서는 생각해볼 여유도 없었습니다. 하지만 이 책을 읽고 난 뒤,
앞서 말한 것들은 모두 상세한 구현사항이라고 추상화시켜 생략해서 프로그램의
의미와 의도라는 보다 큰 관점에서 바라볼 수 있다는 것을 깨닫게 되었습니다.
그러지 않으면 각각의 세부적인 기술의 목표와 구조를 완전히 이해할 수 없다는
생각도 하게 되었습니다.

In terms of human communication, it was like being too immersed in technical
details of communication: how is a sentence structured grammatically; how is
a spoken sentence vocalized; or how does the human auditory system work. They
are valuable topics of their own, but without taking account of their ultimate
goal of conveying ideas among human beings, our understanding of the topics
would be incomplete at best.

사람 간의 의사소통에 비유하자면 기술적인 사항, 즉 문장이 문법적으로 어떻게
구성되어 있고, 발성이 어떻게 이루어지며, 청각 체계가 어떻게 동작하는지 등만
배우고 있었던 것과 유사한 상황이었습니다. 각 주제는 자체적으로도 흥미롭고
가치있긴 하지만 그 최종적인 목적, 즉 사람 사이에서 생각을 전달한다는 목표를
고려하지 않고서는 각각의 소주제를 온전히 이해하는 것은 불가능하다는 것이죠.

This new way of thinking about program was the most valuable lesson I learned
from reading this book.

프로그램을 이런 식으로 더 큰 관점에서 바라보는 것은 이 책을 읽으면서 얻은 가장
가치있는 교훈이었습니다.

### 버그 없는 프로그램을 작성하는 것은 불가능하다

Discussion about undecidable problem also gave me answers to some lingering
questions I had. For example, as I wrote tests for my Ruby program, I wondered
about how many tests I would have to write until I could trust that my program
would run without error. Is it possible to reach a point like that if I kept
writing more test cases? Now I know that the answer is a negative, according to
Rice's theorem that any non-trivial property of program behavior is
undecidiable.

또한 결정 불가능한 문제에 대해 배우고 나니 평소에 코드를 작성하면서 가지게
되었던 의문에도 답을 얻었습니다. 예를 들어 루비 프로그램에 테스트를 작성하면서
생각했던 것인데, 얼마나 많은 테스트를 작성하면 프로그램이 에러 없이 실행된다고
확신할 수 있게 되는 것인지 궁금했습니다. 테스트를 더 많이 작성하면 그 지점에
도달할 수 있는 것일까? 이제 라이스의 정리(Rice's theorem)에 따르면 프로그램이
자명하지 않은 성질을 가지는지 아닌지는 결정 불가능하다는 것을 알게 되었으므로
저 질문의 답은 '불가능하다'라는 것을 알게되었습니다.

When I first learned Haskell, I was thrilled by the slogan that "if it
compiles, it works". They said that the compiler guarantees that the program
would contain no errors. But then it was still possible to have runtime errors
in Haskell. How come? I shrugged the question away, just thinking that no
system is perfect. Now I have theoretical understanding that such a guarantee
is impossible. So even though a rigorous type system can drastically reduce
a category of errors, it still cannot eliminate all errors.

하스켈을 처음 배웠을 때 "컴파일만 되면 문제 없이 동작한다"라는 표어를 보고
큰 기대를 가졌었습니다. 프로그램에 에러가 없다고 컴파일러가 보장한다는
말이었죠. 그런데 실제로는 하스켈 프로그램엠서도 런타임 에러가 발생할
수 있더군요. 어째서? 당시에는 어차피 완벽한건 있을 수 없다고 그냥 대충
넘어갔었는데, 그런 식으로 보장하는 것 자체가 이론적으로 불가능하다는 것을 이제
알게 되었습니다. 엄격한 타입 시스템으로 특정한 종류의 에러를 박멸할 수는 있어도
모든 에러를 제거하는 것은 불가능하니까요.

So all the best practices for making bug-free software were futile by nature.
While I do find that Sisyphean endeavor tragically beautiful in some way, I do
find it a bit disappointing and depressing. I hoped for something more definite
in software development than in my previous field of international security.
I can accept it as a fact of life, but I do feel as if that cake, which I was
promised with, has turned out to be a lie.

결국 버그가 없는 소프트웨어를 만들기 위한 그 많은 베스트 프랙티스도 결국
근본적으로 그 목적을 결코 달성할 수는 없다는 말인데, 무모한 일에 도전하는
모습에 뭔가 비극적인 아름다움이 있긴 하지만 개인적으로는 조금 실망스럽고
안타까운 결론이었습니다. 예전에 다뤘던 국제안보 분야에 비하면 소프트웨어 개발은
더 확실한 무엇인가 있기를 바랐는데 말이죠. 원래 그런거다라고 받아들일 수야
있지만 기대하고 있던 케익이 사실 거짓말이었다는 말을 들은 느낌입니다. 

### 동등한 프로그래밍 언어 중에서 최선의 언어

I had read that most programming languages are Turing complete, which meant
that any Turing complete language could be used to do anything another one
could do, but I didn't give much thought to it. That statement started to make
more sense after I've read this book. After seeing that vastly different
systems like a Turing machine, lambda calculus, rule 110 system, or other still
more weird systems have equivalent power, I found it easier to accept that most
of widely used languages are equivalent in power. This meant that all that
bickering about which language is more powerful is nothing more than waste of
time.

대부분의 프로그래밍 언어가 튜링 완전(Turing complete)하다는 말은 예전에도 들은
적이 있었습니다. 튜링 완전한 언어가 계산할 수 있는 문제는 어떤 튜링 완전한
언어라도 계산할 수 있다는 말로 무슨 의미인지는 잘 몰랐는데 이 책을 통해서 조금
더 잘 알게 되었습니다. 튜링 기계, 람다 대수, 룰 110 시스템이나 그 외 기묘한
시스템이 모두 극적으로 다른 형태를 가지면서도 동일한 계산 능력을 가지고 있다는
것을 보고 나니 널리 쓰이는 프로그래밍 언어들이 동일한 계산 능력을 가지고 있다는
것은 오히려 쉽게 납득할 수 있었습니다. 이는 어떤 프로그래밍 언어가
더 강력한지에 대한 논쟁은 그저 시간 낭비일 뿐이라는 것을 의미합니다.

But if they are all equivalent, what other technical criteria should we use to
choose which language to use? For some projects, there exists just one
reasonable choice. For example, C/C++ is the only reasonable language for
console game development. JavaScript is the only language that runs on a web
browser. For most other situations, however, there are a lot of languages to
choose from. As far as I know, the choice mostly depends on personal preference
of the CTO, or operational requirements like the ease of hiring developers in
the local area. But I wonder if there exist some fundamental technical criteria
that can provide the answer from engineering perspective.

하지만 모든 언어가 동등하다면 어떤 언어를 사용할지 판단할 때, 어떤 기술적
기준을 따라야 하는 것일까요? 일부 프로젝트에는 선택지가 사실상 단 하나
뿐입니다. 예를 들어 콘솔용 게임을 개발하려면 C/C++ 외에 대안이 없죠.
웹 브라우저 환경에서는 자바스크립트 외에 다른 언어는 실행할 수 없습니다. 하지만
그 외의 경우에는 보통 여러가지 언어 중에서 선택이 가능합니다. 제가 아는 바로는
그러한 결정은 CTO의 개인적인 선호도나 개발자 채용이 용이한가 등의 운영적 측면을
고려해서 내려지는 경우가 대부분입니다. 하지만 공학적인 관점에서 이에 대한 판단
기준이 될만한 근본적인 기술적 기준이 존재하는지 궁금해졌습니다.

## 쩌는 트롤링

Finally, I want to share a beautiful piece of trolling the author throws at the
readers. In the middle of the book, the author implements FizzBuzz using lamba
calculus implemented only with Ruby's anonymous funtions.

글의 마무리로 저자가 책에 저지른 트롤링을 하나 공유하려 합니다. 책 중간 쯤에
루비의 익명함수 기능만을 활용하여 람다 대수를 구현하고, 이를 써서 피즈버즈
프로그램을 구현하는 부분이 있습니다. 해당 챕터 마지막에 전체 프로그램 코드를
한 번 보여주는데, 4쪽에 걸쳐서 이어지는 장대한 트롤링 코드입니다.

I want to note that, however, this is not some meaningless trolling. The author
thoroughly demonstrates how to implement each part of this code, so if
you actually read the book, you can discern some of the moving parts in the
code. In fact, I consider this another testament to the author's excellent
writing skill.

단, 의미 없는 트롤링은 아닙니다. 저자는 이 코드의 각 부분을 어떻게 구현하는지
매우 꼼꼼하게 설명하며, 덕분에 책을 읽었다면 이 코드 내에서 일부 부분을 실제로
인지하고 이해할 수 있습니다. 그래서 이 코드는 오히려 작가가 여기까지 책을 읽고
따라온 독자에게 치는 유쾌한 장난이라고 보면 됩니다.

You can copy the following code to a Ruby file and run it to see if it is
actually a working FizzBuzz program.

다음 코드를 루비 파일에 복사한 뒤에 실행해보면 실제로 동작하는 피즈버즈
프로그램인지 확인 가능합니다.

Now enjoy the code:

그러면 코드를 보실까요?

```ruby
=begin
FizzBuzz program with linebreaks

-> k { -> f { -> f { -> x { f[-> y { x[x][y] }] }[->
x { f[-> y { x[x][y] }] }] }[-> f { -> l { -> x { -> g { -> b { b }[-> p { p[->
x { -> y { x } } ] }[l]][x][-> y { g[f[-> l { -> p { p[-> x { ->
y { y } } ] }[-> p { p[-> x { -> y { y } } ] }[l]] }[l]][x][g]][-> l { ->
p { p[-> x { -> y { x } } ] }[-> p { p[-> x { -> y { y } } ] }[l]] }[l]][y] }]
} } } }][k][-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { ->
y { x } }]][-> l { -> x { -> l { -> x { -> x { -> y { -> f { f[x][y] } } }[->
x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[l][f[x]] } }]
} }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[->
f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { ->
y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[->
g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]]
} }[m][n]][-> x { -> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { ->
y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[f[-> n { -> p { ->
x { p[n[p][x]] } } }[m]][n]][m][x] }][-> x { -> y { -> f { f[x][y] } } }[->
x { -> y { x } }][-> x { -> y { x } }]] } } }][-> p { -> x { p[x] } }][->
p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p
[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p
[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]]]]]]]]]
]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]
} }]][-> n { -> b { b }[-> n { n[-> x { -> x { -> y { y } } }][-> x { ->
y { x } }] }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }]
}[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { ->
y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[->
g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]]
} }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { ->
h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] } ][m]
} } }][n][-> p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]]
} }]]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][->
x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { ->
f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y]
} } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { ->
y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { ->
x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { ->
f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[->
x { -> y { x } }][-> x { -> y { x } }]][-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[->
n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[->
p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { ->
n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][->
p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]]
} }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { ->
x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[->
m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { ->
x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]]
} }]]]]]][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]]
} } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { ->
x { p[p[p[p[p[x]]]]] } }]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { ->
x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]]
} }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[->
n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[->
p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { ->
n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][->
p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]]
} }]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { ->
n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }]
} }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]][->
b { b }[-> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> f { ->
x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { ->
n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { ->
y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]]
} }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { ->
n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }]
} } }][m] } }[m][n]][n][x] }][m] } } }][n][-> p { -> x { p[p[p[x]]] } }]]][->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][->
n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[->
n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[->
m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m]
} }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { ->
x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { ->
x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]]
} }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[->
n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[->
p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]][-> n { ->
p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { ->
x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]]
} }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]][-> b { b }[-> n { n[-> x { ->
x { -> y { y } } }][-> x { -> y { x } }] }[-> f { -> x { f[-> y { x[x][y] }]
}[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { ->
n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { ->
n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }]
} } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { ->
x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m]
} }[m][n]][n][x] }][m] } } }][n][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { ->
y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y]
} } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[->
x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][->
n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[->
n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[->
m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m]
} }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { ->
x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { ->
p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { ->
x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]]
} }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]]
} } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]]
} } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m]
} }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { ->
x { p[p[p[p[p[x]]]]] } }]]]]]][-> m { -> n { n[-> m { -> n { n[-> n { -> p { ->
x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]]
} }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][-> f { -> x { f[-> y { x[x][y] }]
}[-> x { f[-> y { x[x][y] }] }] }[-> f { -> n { -> l { -> x { -> f { ->
x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> l { ->
x { -> g { -> b { b }[-> p { p[-> x { -> y { x } }] }[l]][x][-> y { g[f[->
l { -> p { p[-> x { -> y { y } }] }[-> p { p[-> x { -> y { y } }] }[l]]
}[l]][x][g]][-> l { -> p { p[-> x { -> y { x } }] }[-> p { p[-> x { ->
y { y } }] }[l]] }[l]][y] }] } } } }][l][-> l { -> x { -> x { -> y { ->
f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y]
} } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][->
x { -> y { x } }]][x]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[->
x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }] } }[->
b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { ->
y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]]
} }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][-> n { -> f { ->
x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }[-> m { ->
n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][->
p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]]
} }]]]][-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { ->
y { x } }]][-> x { f[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[->
y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[->
x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { ->
f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m]
} }[m][n]] } }[n][m]][-> x { -> n { -> p { -> x { p[n[p][x]] } } }[f[-> m { ->
n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }]
} } }][m] } }[m][n]][n]][x] }][-> p { -> x { x } }] } } }][n][-> m { ->
n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][->
p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]]
} }]]][x] }]][-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }]
}[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { ->
y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[->
g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]]
} }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { ->
h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] }][m]
} } }][n][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]]
} } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { ->
x { p[p[p[p[p[x]]]]] } }]]] } }][n]]]] }]
=end

fizzbuzz_program =
  -> k { -> f { -> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> l { -> x { -> g { -> b { b }[-> p { p[-> x { -> y { x } } ] }[l]][x][-> y { g[f[-> l { -> p { p[-> x { -> y { y } } ] }[-> p { p[-> x { -> y { y } } ] }[l]] }[l]][x][g]][-> l { -> p { p[-> x { -> y { x } } ] }[-> p { p[-> x { -> y { y } } ] }[l]] }[l]][y] }] } } } }][k][-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][-> l { -> x { -> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[l][f[x]] } }] } }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[m][n]][-> x { -> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[f[-> n { -> p { -> x { p[n[p][x]] } } }[m]][n]][m][x] }][-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]] } } }][-> p { -> x { p[x] } }][-> p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]]] } }]][-> n { -> b { b }[-> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] } ][m] } } }][n][-> p { -> x { p[p[p[p[p[p[p[p[p[p[p[p[p[p[p[x]]]]]]]]]]]]]]] } }]]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]][-> b { b }[-> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] }][m] } } }][n][-> p { -> x { p[p[p[x]]] } }]]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]][-> b { b }[-> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] }][m] } } }][n][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]]][-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> n { -> p { -> x { p[n[p][x]] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]]]][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> n { -> l { -> x { -> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> l { -> x { -> g { -> b { b }[-> p { p[-> x { -> y { x } }] }[l]][x][-> y { g[f[-> l { -> p { p[-> x { -> y { y } }] }[-> p { p[-> x { -> y { y } }] }[l]] }[l]][x][g]][-> l { -> p { p[-> x { -> y { x } }] }[-> p { p[-> x { -> y { y } }] }[l]] }[l]][y] }] } } } }][l][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }[-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][x]][-> l { -> x { -> x { -> y { -> f { f[x][y] } } }[-> x { -> y { y } }][-> x { -> y { -> f { f[x][y] } } }[x][l]] } }] } }[-> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }[-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]]][-> x { -> y { -> f { f[x][y] } } }[-> x { -> y { x } }][-> x { -> y { x } }]][-> x { f[-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { -> n { -> p { -> x { p[n[p][x]] } } }[f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n]][x] }][-> p { -> x { x } }] } } }][n][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]][x] }]][-> f { -> x { f[-> y { x[x][y] }] }[-> x { f[-> y { x[x][y] }] }] }[-> f { -> m { -> n { -> b { b }[-> m { -> n { -> n { n[-> x { -> x { -> y { y } } }][-> x { -> y { x } }] }[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]] } }[n][m]][-> x { f[-> m { -> n { n[-> n { -> f { -> x { n[-> g { -> h { h[g[f]] } }][-> y { x }][-> y { y }] } } }][m] } }[m][n]][n][x] }][m] } } }][n][-> m { -> n { n[-> m { -> n { n[-> n { -> p { -> x { p[n[p][x]] } } }][m] } }[m]][-> p { -> x { x } }] } }[-> p { -> x { p[p[x]] } }][-> p { -> x { p[p[p[p[p[x]]]]] } }]]] } }][n]]]] }]

# Helpers

LEFT  = -> p { p[-> x { -> y { x } } ] }
RIGHT = -> p { p[-> x { -> y { y } } ] }
IF    = -> b { b }
IS_EMPTY  = LEFT
FIRST     = -> l { LEFT[RIGHT[l]] }
REST      = -> l { RIGHT[RIGHT[l]] }

def to_integer(proc) proc[-> n { n + 1 }][0] end

def to_boolean(proc)
  IF[proc][true][false]
end

def to_array(proc)
  array = []

  until to_boolean(IS_EMPTY[proc])
    array.push(FIRST[proc])
    proc = REST[proc]
  end

  array
end

def to_char(c)
  '0123456789BFiuz'.slice(to_integer(c))
end

def to_string(s)
  to_array(s).map { |c| to_char(c) }.join
end

# Print result

to_array(fizzbuzz_program).each do |p|
  puts to_string(p)
end
```
