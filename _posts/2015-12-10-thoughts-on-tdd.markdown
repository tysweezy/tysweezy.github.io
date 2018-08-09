---
layout: post
title:  "Quick Thoughts on Test-Driven Development and Design Patterns"
date:   2015-12-10 22:55:17 -0700
categories: tdd rant
---

As a developer, a lot of times I feel constantly worried that I’m doing the “wrong thing” when I’m writing code. I worry if I’m doing “x” correctly, while I’m trying to focus on “y” thing. This mostly occurs for me when terms such as “[test-driven development][1]” and “[design patterns][2]” get thrown around. I usually catch myself worrying about these topics at the wrong times; especially when I’m just trying to write something simple and get the product out here in the wild. Here are my personal thoughts and advice when approaching these topics.

![enter image description here][3]

## Test-Driven Development

Before I dive in, I just want to mention that even if you don’t practice the TDD practice, you still test without even noticing it. Let’s say you’re building a site and you’re creating a form. You make a change, save, reload the browser and fill out the form to see if all works. BOOM! You just did some form of testing. Yes, you did it manually, but you still tested. TDD is more of an automated form of testing, in which, you don’t have to do the tedious task of reloading the browser every time you make a change. Note that I’m FAR from an expert in the testing world (pretty much a noob), but I know enough to kind of talk about it (I think).

![enter image description here][4]

A common concern for myself and other developers is that thought, “should I be writing a test?” I think that answer kind of depends. You should ask yourself: is my app small enough to even need tests? Do I need to future proof my app? Etc. If you’re just working on a weekend throw away project, then it’s probably not necessary to write tests. However, if you’re working on a big enterprise software application, then it might be a good idea to write tests. As a matter of fact, I think [Jeffery Way][5] brought this up in the latest [Laravel podcast][6].

If you’re just purely interested in TDD and want to study it, my recommendation would be to start learning what unit testing is. Then go on to other concepts such as: functional tests, acceptance testing, etc. [Unit testing][6] is process in which you’re testing “units” or a specific feature of your application. For example, if you’re writing a trivial calculator app and want test an “add” method, you are testing that specific “add” feature or unit. It’s actually not as hard as it sounds. Write a test, run test and see if it fails, make some write some code, run tests, refactor and repeat.

## Design Patterns

Software design patterns. There are so many of them. I’m one of those devs that loves to make clean, reusable code. I giggle with joy when I write some clean, badass code; especially when I implement a design pattern I’ve just learned. However, I fall in the “am I doing right at the right place and time?” trap. I’ve caught myself over-engineering code, wasting so much time over something really simple. It’s like writing an entire complex class when your could’ve written the same script with a couple of functions.

![enter image description here][7]

Just like with the TDD topic, you need to ask yourself questions: Am I going to be reusing code later on in my app? With this piece of complex functionality, am I going to be repeating myself? If you’re writing an app that only has a few database tables, a couple models and controllers, a few views, then it’s probably not necessary to implement a design pattern. If you’re writing something complex and you start to repeat yourself, it’s probably a good idea to implement a design pattern specific to your needs. Also note, if you are repeating yourself, it’s best to follow the DRY(don’t-repeat-yourself) practice.

## Closing Points

![enter image description here][8]

Overall, I just wanted to share my anxiety I have when coming across these beasts. It’s not easy for a developer who is just starting out to be slammed with these topics. I’ve been doing web development for about 5 years and I feel like I’ve only scratched to surface; still feeling like a noob today. Just remember to take small steps, learn one thing at a time. In terms of developing an application, just make it work and run, then worry about refactoring when you hit an epiphany.

Also, speaking of TDD. If you have a chance, you all should read Kent Beck’s, *“[Test-Driven Development][9].”* It’s a very good read. Definitely my rainy day book for sure.

Sources

<https://en.wikipedia.org/wiki/Software_design_pattern> [https://en.wikipedia.org/wiki/Test-driven_development][2] <http://searchsoftwarequality.techtarget.com/definition/unit-testing> [http://www.laravelpodcast.com/episodes/20182-episode-38-repositorybeanfactory][6]



 [1]: https://en.wikipedia.org/wiki/Test-driven_development
 [2]: https://en.wikipedia.org/wiki/Software_design_pattern
 [3]: http://i.giphy.com/7MZ0v9KynmiSA.gif
 [4]: http://i.giphy.com/VHHxxFAeLaYzS.gif
 [5]: https://twitter.com/jeffrey_way
 [6]: http://searchsoftwarequality.techtarget.com/definition/unit-testing
 [7]: http://i.giphy.com/ycNkwiWl7Ralq.gif
 [8]: http://i.giphy.com/zrvFl1IDvy0PC.gif
 [9]: http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530