---
layout: post
ref: my-brief-foray-into-the-land-of-haskell-1
date: 2017-04-22 00:00:00 +0900
title: 하스켈 나라 탐험기 (1)
lang: ko
---

## 뭔가 무서운 하스켈 나라

I think few other programming languages evoked as much fear in me as Haskell did. After all, it is a language known for being arcane and unusual. Still, it seemed like an interesting language to learn so I've been eyeing for an opportunity to venture into the land of Haskell for a while.

하스켈은 뭔가 배우기 겁이 나는 언어입니다. 워낙 배우기 어렵고 특이하기로 유명하니까요. 그래도 뭔가 흥미를 끄는 면이 있었기 때문에 하스켈을 탐사할 기회에는 항상 눈독들이고 있었습니다. 

## 계획 수립

It seems that Haskell is hard to learn because of two reasons:

* Haskell is wildly different from mainstream imperative languages.
* Resources for learning Haskell are terrible in terms of beginner-friendliness.

크게 두 가지 이유 때문에 하스켈을 배우기 힘들다고 하는 것 같습니다.

* 하스켈은 일반적인 명령형 언어와 극명히 다르다.
* 입문자가 편히 하스켈을 배울 수 있는 자료가 없다.

Well, I can't do anything about the first reason. But the second reason looked familiar. I've seen the same pattern back in college. A professor knowledgeable in her area can be terrible at teaching due to lack of training in pedagogy. And I've already suffered from such professors in college, so I wasn't keen on subjecting myself to the same kind of torture again. I had to find a good guidance.

Then I came upon an article titled [Functional Education](http://bitemyapp.com/posts/2014-12-31-functional-education.html) by @bitemyapp. The author discussed the problems in learning functional programming, and reviewed many existing resources for learning Haskell. I did not, and still do not, know enough to evaluate his review. But he sounded rational enough that I decided to follow his recommendation.

His recommendation was to begin with Brent Yorgey's [cis194 course](http://www.seas.upenn.edu/~cis194/spring13/lectures.html) first and then take [Data61 course](https://github.com/data61/fp-course).

Now I finally knew where to start. Time to venture forth.

첫 번째 이유는 제가 뭐 어쩔 수 없는 부분입니다. 하지만 두 번째 이유는 보자마자 떠오르는 것이 있었습니다. 대학 시절 경험인데요, 자기 분야를 정말 잘 아는 교수가 오히려 교수법을 배우지 못해서 가르치는 것은 정말 못 하는 경우가 있죠. 대학 때 그런 교수들 때문에 이미 많이 고생했기 때문에 하스켈을 배우면서 똑같은 경험을 하고 싶은 생각은 결코 들지 않았습니다. 뭔가 좋은 안내서가 필요했습니다.

그러다가 @bitemyapp이 작성한 [Functional Education](http://bitemyapp.com/posts/2014-12-31-functional-education.html)라는 글을 읽게 되었습니다. 함수형 프로그래밍을 배울 때 문제점에 대해서 논의하고, 하스켈을 배우는 여러 자료를 평가한 글입니다. 저는 저자가 내린 평가들이 얼마나 정확한지 평가할 능력은 그 글을 처음 읽을 때나 지금이나 없습니다. 하지만 평가 내용이 꽤나 합리적인 것처럼 보였기 때문에 그 사람이 추천한 방식을 따르기로 했습니다. 

저자는 먼저 브렌트 요기의 [cis194 강의](http://www.seas.upenn.edu/~cis194/spring13/lectures.html)를 듣고나서 [Data61 강의](https://github.com/data61/fp-course)를 진행하라고 추천했습니다.

## 회상: 내가 이미 갖추고 있던 지식

In hindsight, I was actually quite well equipped to learn Haskell. Although I had barely a year under my belt as a professional software developer, my path somehow exposed me to many foundational concepts of Haskell. Specifically, I was already familiar with or exposed to the following concepts: higher order function, recursion, type inference, parametric polymorphism, lazy evaluation, fold, and pattern matching.

I started my first serious software development with Ruby. Although Ruby is an objected-oriented language, it makes heavy use of higher order functions through blocks. Functions like `map` or `reduce`, which are basic patterns of functional programming, are also commonly used in Ruby.

Then I briefly worked with Swift for iOS development. Swift promotes using immutable data unless required otherwise. It also makes heavy use of higher order functions as callbacks. Swift also provides compile time type checking and type inference. Although I haven't really used generic programming in Swift, it did give me a chance to expose myself to it.

My next serious involvement was with Elixir. Pattern matching and recursion are bread and butter of Elixir so I was comfortable with them. I got more familiar with immutable data structures because Elixir, unlike Swift, provided no mutability whatsoever. I also gained deeper understanding of folding, which I've used in Ruby without really understanding it. I've known about the concept of lazy evaluation, but I first started actually using it in Elixir.

Still, I didn't know that I already had all these ingredients ready when I dived into Haskell. Somehow the dots got connected in the end, I guess.

하스켈을 배우는 과정에서 깨달은 점인데, 저는 생각보다 하스켈을 배울 준비가 잘 되어 있었습니다. 소프트웨어 개발을 본격적으로 한 기간은 일 년 정도밖에 안 되었음에도 불구하고 어쩌다보니 하스켈의 기본이 되는 개념들에 많이 익숙해져 있었습니다. 구체적으로는 고차함수, 재귀, 타입 추론, 파라미터 폴리모피즘, 느긋한 계산법, 폴드, 그리고 패턴 매칭 등을 이미 접해봤었습니다.

소프트웨어 개발을 제대로 해본 것은 루비를 사용해서였습니다. 루비는 객체지향 언어이지만 블록 문법에서 고차함수를 정말 많이 사용합니다. 함수형 프로그래밍의 기본 패턴이 되는 `map`이나 `reduce` 함수도 루비에서 일상적으로 사용됩니다.

그리고 나서 iOS 개발 때문에 스위프트를 잠깐 다루었습니다. 스위프트는 정말 가변형 데이터 타입이 필요하지 않은 이상 불변형 데이터 타입을 기본적으로 사용하는 것을 권장합니다. 또한 고차함수를 콜백 형식으로 많이 사용합니다. 그리고 제너릭 프로그래밍을 스위프트에서 많이 사용해보지는 않았지만 어떤 개념인지 익숙해질 기회는 있었습니다.

그 후 가장 많이 다룬 언어는 엘릭서입니다. 패턴매칭과 재귀는 엘릭서의 기본이 되는 도구이기 때문에 매우 익숙해질 수 있었습니다. 그리고 스위프트와 달리 엘릭서는 불변형 데이터 타입만 사용할 수 있기 때문에 불변형에도 더 익숙해졌습니다. 그리고 루비에서는 잘 이해하지 못하고 그냥 사용했던 폴드를 보다 잘 이해하게 되었습니다. 마찬가지로 느긋한 계산법의 개념도 엘릭서에서 조금 더 체화할 수 있었습니다. 

물론 하스켈을 처음 배우려고 할 때는 제가 이런 요소들을 이미 갖추고 있다는 것을 모르고 있었습니다. 어쩌다보니 점이 연결된 셈입니다.

## CIS194 (SP 13) 실제로 추천합니다

I liked the conciseness of the course material. It hit the right balance between terseness and verbosity. The instructor also provided relevant chapters from [Learn You a Haskell for Great Good](http://learnyouahaskell.com/) and [Real World Haskell](http://book.realworldhaskell.org/) as suggested reading each week. When the explanation in the course material felt too short, reading those chapters gave me an opportunity learn more slowly and thoroughly. It's my favorite combination of learning materials - concise core material complemented by extensive optional resources.

But I think the best thing about this course is homeworks. They are definitely challenging. I confess that I looked at the solutions available online when I couldn't even think of how to get started on some homeworks. Nevertheless, all homeworks can be solved just with the concepts introduced in the course material. This is actually an impressive feat - Yorgey made challenging homeworks out of the concise course material. You would understand how hard that is if you've tried to create good exercise problems yourself.

강의에서 가장 마음에 드는 점은 간결함이었습니다. 너무 짧지도 너무 번잡하지도 않은 설명이 좋았습니다. 게다가 강사가 [Learn You a Haskell for Great Good](http://learnyouahaskell.com/) 책과 [Real World Haskell](http://book.realworldhaskell.org/) 책에서 강의 내용에 관련된 챕터를 권장 과제로 내주었는데, 강의 내용이 너무 짧다고 느껴질 때는 해당 책의 챕터를 읽으면서 보다 꼼꼼하고 천천히 내용을 익힐 수 있었습니다. 핵심 내용을 간결하게 전달하고 이를 보조할 수 있는 구체적인 자료를 제공하는 것은 제가 가장 좋아하는 방식입니다.

하지만 제 생각에 이 강의에서 가장 뛰어난 부분은 숙제입니다. 쉽지는 않습니다. 숙제를 하면서 어디부터 시작해야할 지 감도 안 잡힐 때는 저도 인터넷에 공개된 해답을 참고하기도 했습니다. 하지만 실제로는 강의 내용에 소개된 개념만 가지고도 숙제를 풀 수 있습니다. 이건 꽤나 대단한 일입니다. 연습 문제를 만들려고 해본 사람이면 이해하겠지만 간결한 강의 내용의 범위 안에서 적절히 어려우면서도 학습 효과가 뛰어난 숙제를 만드는 것은 쉽지 않은 일입니다.

So I would recommend this course to someone who wants to learn Haskell. But there's a caveat. If you are completely new to functional programming, get a mentor or teacher. This course is still too fast-paced for such a person. I have a personal rule of thumb when choosing a learning material for learning by myself: I should be able to understand 70% of the material with ease, and actively learn only the other 30%. Otherwise, I would end up losing motivation out of exhaustion.

If you are trying to learn functional programming by youself, I recommend starting with Elixir. Elixir is a simple, practical language that can familiarize you with basic concepts of functional programming. Once you have knowledge of those basic building blocks, learning more advanced functional programming concepts in Haskell would feel much less stressful.

In the next post, I will describe what I learned from my adventure in the land of Haskell.

그래서 하스켈을 배우고 싶은 사람에게는 이 강의를 추천하고 싶습니다. 하지만 조건이 있습니다. 함수형 프로그래밍이 완전 처음이라면 멘토나 교사를 구하세요. 그런 사람에게는 이 수업은 여전히 너무 페이스가 빠를 것 같습니다. 제가 혼자서 공부할 때 자료를 선택할 때는 기준이 있는데, 전체 내용의 70%는 쉽게 이해할 수 있어야한다는 것입니다. 그래야 나머지 30%에 집중력을 쏟아 공부해도 지쳐서 중간에 포기하는 일이 없어지니까요. 제가 이 강의를 혼자서 공부할 수 있었던 이유는 앞서 언급한 개념들을 이미 알고 있었기 때문에 모르는 부분만 배우면 되었기 때문입니다.

함수형 프로그래밍을 처음부터 독학하시려면 엘릭서도 추천드립니다. 엘릭서는 간단하고 실용적인 언어이기 때문에 함수형 프로그래밍의 기초적인 개념을 배우기에 적합합니다. 그런 기초 개념을 익히고 나면 하스켈을 통해서 함수형 프로그래밍의 고급 개념을 배우는 것이 훨씬 덜 부담스러울 것입니다. 

다음 글에서는 하스켈 나라를 탐험하면서 무엇을 배웠나에 대해서 적어보겠습니다.