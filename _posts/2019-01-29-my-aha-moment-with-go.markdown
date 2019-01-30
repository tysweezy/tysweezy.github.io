---
layout: post
title:  "My “A-ha” Moment with Go - Understanding the Interface"
date:   2019-01-29 15:56:17 -0700
categories: software engineering go
author: Tyler Souza
share: true
---

I’ve been using and toying with Go for almost two years now. At the time I decided to learn the language, I was curious on what this language had to offer. 

Initially, I was really intrigued by it’s simplicity. Loved that it was minimal and easy to pick up. I learned the basics of the language in only a couple of hours and loved writing programs in it. I began to think, “where has this language been all of my life? Why am I just barley learning this language?” It quickly became my favorite language to use, but as I began to dig deeper, there were definitely frustrations that came along with it.

Here has been my process for learning Go so far:

* OMG! I love this language! Where has this been all my life?
* Why do we have to code this way? Why do we have to check if an error is nil. What’s the point of interfaces?! I’m going back to JavaScript, Python or PHP!! 
* I think I get his now. Go is fun again.
* I’m lost again. Not sure Go is for me. Losing all hope.
* A-ha!!! I get it now  that’s why it was designed this why. That’s awesome! Everything makes sense. Go is my favorite again.

Here’s some of the things I struggled with as a young Gopher: 

* What’s the point of interfaces? 
* Pointers (I don’t come from a C background, so they were/are new to me)
* Returning multiple values is cool, but why even return errors? Why can’t you do this within your functions? 
* Slices and slice manipulation were weird to me at first coming from a JS/PHP background where I was used to arrays from those languages.
* Minimalism is cool and all, but I was kind of frustrated with the common phrase “just use the standard library for everything” that was said throughout the community. I believe in this, but I don’t feel like you need to write everything from scratch (ex. database migrations as an example). 
* Go routines  are still a new topic for me. They are super cool rabbit-holed concept to get into.
* Etc.

## Interfaces
I’m going to be mainly discussing interfaces in this post, because understanding interfaces initiated my “a-ha” moment with Go. Understanding this part of the language actually made me fall in love with it. As a side note, I feel like finally understanding something is what drives me as a programmer and makes me love what I’m doing. I don’t mind the frustrations that come along. 

### What is an Interface? 

In my own words, interfaces are a collection in method definitions. Let me explain what I mean. 

In a language such as Java or PHP, you explicitly define the methods/types that are used when a class implements it. In PHP, when a class implements an interface, that class has to use those methods. For example:

```php
<?php

// ExampleInterface.php

interface ExampleInterface {
    recordList();
	getRecord($id);
}


<?php

// Example.php

class Example implements ExampleInterface {
    public function recordList() 
    {
		// ...
    }

    public function getRecord($id)
    {
		// ...
    }
}
```

There are few key differences with interfaces in Go. In Go, interfaces are implemented implicitly. Meaning you don’t have to directly implement an interface to satisfy the methods within it. Only the necessary types and input/output. 

[From the Golang tour](https://tour.golang.org/methods/10): _”Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.”_

## Example time
For example, let’s say I have a “watcher” which watches things. In it’s definition it has a `Watch() bool` function that returns a boolean. I also have a couple of structs, Task and Meeting.

```go
type Watcher interface {
	Watch() bool
}

type Task struct {
	name string
	complete bool
}

type Meeting struct {
	name string
	complete bool
}
```

Then, we can create functions that implement this interface:

```go
func (t Task) Watch() bool {
	return true
}

func (m Meeting) Watch() bool {
	return false
}
```

Now every time use those structs we can call the `Watch` method. 

```go
t := Task{name: "Todo", complete: true}
m := Meeting{name: "meeting with the bobs", complete: false}

t.Watch() // true
m.Watch() // false
```

This allows our code to be polymorphic and we don’t have to re-write logic for a watcher every time we need to use it.

A good example of interfaces is in [Go’s `io` package](https://golang.org/pkg/io/).  Let’s take a look at `io.Reader`

```
type Reader interface {
        Read(p []byte) (n int, err error)
}
```

You can actually use an interface as argument in a function, extending it’s use. For example, you can write a custom function for limiting the amount of bytes passed in.

```go
func CustomLimiter(r io.Reader, n int) Reader {
	// some limiter logic that returns a reader
} 
``` 

Actually, the io package already has this (called `LimitReader`), but I’m just using the above as an example to visualize the power of interfaces. 

```go
// LimitReader returns a Reader that reads from r
// but stops with EOF after n bytes.
// The underlying implementation is a *LimitedReader.
func LimitReader(r Reader, n int64) Reader { return &LimitedReader{r, n} }
```

Reading code from Go’s standard library is awe inspiring. After looking through this I realized the power that interfaces have. Are they needed all of the time? No. However, as your application or program grows, they are a powerful tool to keep your code clean and maintainable. Also, if you’re building a package, they may be necessary so that users of your package can extend the behavior of your code.

## The Empty Interface
Ah! The good ‘ole empty `interface{}` . An empty interface are used to handle values of an unknown type. Moreover, they may hold values of any type. I find them extremely useful when I’m accepting parameter in a function and I’m unaware of the type that is going to be passed in. 

Although, I would caution to not overuse an empty interface as the compiler won’t be able to check for the type the value holds.

## Closing Points
Once I’ve had this “light build” moment, I’m completely sold on Go now. It’s pretty much turned into my favorite language. Although, there were some frustrating learning curves, I will say that once you understand Go, you’ll end up loving it.  

Here are some resources on interfaces: 

* Francesc Campoy’s talk  - 
<iframe width="560" height="315" src="https://www.youtube.com/embed/F4wUrj6pmSI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
* Jon Calhoun’s blog post on Interfaces - [https://www.calhoun.io/how-do-interfaces-work-in-go/](https://www.calhoun.io/how-do-interfaces-work-in-go/)
* And of course, Go’s documentation - [A Tour of Go](https://tour.golang.org/methods/9)