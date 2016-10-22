---
layout: post
ref: lets-get-better-at-teaching
date:   2016-10-24 00:00:00 +0900
title: We'd Better Get Better At Passing Down Lessons Learned
lang: en
---

I was a Ruby on Rails developer for some time. And like all new Rails developers do, I ended up creating a horrible nightmare of fat and tangled controllers and models. The problem was exacerbated by the fact that I was self-taught and there was no experienced developer to guide me at work. I felt lost in that disaster.

Reading through numerous blog posts, I found out that it was not just me. This was a Rails thing shared by many Rails developers. It's funny how I could even see some kind of historical eras for solutions: the Fat Controller Age, the Fat Model Age, and the Plain Old Ruby Object Age. Other solutions like Ruby Object Mapper or Hanami framework use data mapper pattern. 

So I got burned a lot and learned a lot to deal with burns. Still, it would've been better if I didn't have to get my entire rear area incinerated in the process of learning that. Now I see Rails as what it is: it's a framework that increases the productivity of experienced web developers who know their domain. Such convenient ready-made conventions are called "the Rails Way."

That Rails is easy to learn for new developers is just a nice side effect. And a side effect should be carefully avoided, not mercilessly exploited. But it's impossible for new developers to know these things without someone to point out all the hidden traps of the Rails Way.

AciveRecord is an epitome of the Rails Way. It's a feral beast that's extremely pleasant to work with at first but will bite you in the ass as soon as you let your guard down. Many people have said many things about it, so I won't rant about ActiveRecord here. 

But I'd like to talk about something more general. This is a section about Active Record pattern in Clean Code by Robert C. Martin.

> Active Records are special forms of DTOs [Data Transfer Object]. They are data structures with public variables; but they typically have navigational methods like save and find. Typically these Active Records are direct translations from database tables, or other data sources.  

> Unfortunately we often find that developers try to treat these data structures as though they were objects by putting business rule methods in them. This is awkward because it creates a hybrid between a data structure and an object.

> The solution, of course, is to treat the ActiveRecord as a data structure and to create separate objects that contain the business rules and that hide their internal data (which are probably just instances of the ActiveRecord).

I got furious after reading that. That book was written in 2008 and is one of the most famous books about programming. Why did I read it only after all those painful months with Rails ActiveRecord? And more importantly, why do people repeat making the same mistake when the solution is already clearly laid out? Why did I and other newcomers have to suffer from this ridiculous thing again for years?

I remember panelists in Ruby Rogues podcast talking about the same frustration when they talked about the book Structure and Interpretation of Computer. They lamented that the book was published in 1979 and contains answers to many of the problems that we continue to see even today.

People in computer science are generally terrible at teaching. Few have experience in teaching, and even fewer have formal training in pedagogy. We should be doing a much better job at passing down our hard-earned experience to newcomers in the field. It would help advance our field. But more importantly, it would save new developers from the pain we had to endure. Well, unless you're a sadistic bastard who likes to watch them suffer like you did.