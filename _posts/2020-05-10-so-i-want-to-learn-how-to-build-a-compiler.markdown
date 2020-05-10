---
layout: post
title:  "So I Want to Learn How to Build a Compiler"
date:   2020-05-10  14:14:17 -0700
categories: software engineering languages
author: Tyler Souza
share: true
---

I love learning and talking about programming languages. Mostly enjoy a different way of thinking that comes with it. Bring up Haskell? We’ll talk about Monads and functional programming. Clojure? Let’s talk about transducers. Bringing up Go? Let’s talk about go routines, concurrency and striving for simplicity. 

## Why Would I Want to Build a Compiler? That’s Crazy? 
There are couple of reasons why I want to build one: 
* It’s an interesting topic that piques my interest. 
* I want to build a language for ME. I want to build a language that I strive for simplicity, a language that I enjoy and one that can solve the problems that I have.
* I want to learn something new. I enjoy the topic and want to dive in. 

So what got me inspired? Well, one reason is talking to my colleagues about different languages and how they solve different problems. I always love a great conversation about languages or a specific one rather. 

Secondly, with all of this COVID-19 stuff going at the moment, I think it’s good to delve into my hobbies and start a good fun project. I’ve been hunting for a side-project to work on. 

Also, I tend to follow Johnathan Blow a lot in regards to language design. Definitely an inspiration in this topic. Especially when he discusses parsers in this stream.

<iframe width="560" height="315" src="https://www.youtube.com/embed/MnctEW1oL-E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## My Idea For a Language
Since this is a learning experience for me, I want to keep things simple. My idea for this language in mind is a functional based language with a C-styled syntax. 

Also, for the implementation language, I’ll probably use Python since it will be faster for me to iterate and come up with a prototype. I can probably switch over to C/C++ or Go if performance is a concern. However, depends on the goals as things develop. 

Also looking around, maybe a tool like PLY might be helpful for a first language. [PLY (Python Lex-Yacc)](http://www.dabeaz.com/ply/)

Though, first start is to learn about parsers, lexers, tokens, etc. 

## Rough Idea of the Language I Want to Build
Here’s a rough idea of the language I want to build. I don’t have a name for it yet, but this the basic idea. 

```
// primatives
str, bool, int, float

// you can create your own types

// constant variables. Always immutable
let foo = value

// maybe strongly typed?
let foo = int: 32
let bar = str: "some string"
let baz = bool: true

// type inference
let foo := "some value"
let bar := 32
let baz := true

// lists
[1, 3, 4]

// hash maps or dictionaries
{ key: value }

// you can create your own types
type Square = Float :: Float :: Float :: Float

// one example of a method.
fn filter_list(arg1, arg2) ->
  return arg1.filter(value) 
    |> arg2.add(value)
    |> foo(bar)
  

// inline return function
fn filter_foo() -> map(list, fn)

// function with types
fn foo(arg1: string, arg2: int) -> 
  // logic

// conditions
if -> then

// control flow 

// should there be a concept of loops? Especially since ther will be a 
// heavy focus on functional programming? espcially focus on higher-order
// functions such as map, filter, reduce, etc.  
```

## Closing Points
Now off to learning! Excited to get into it. Will keep updating the blog with what I’ve learned during the process. Cheers!