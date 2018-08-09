---
layout: post
title:  "Part 1: Wavvy, My Open Source Project Idea. Providing REST API Helpers and Syntactic Sugar"
date:   2018-08-08 17:55:17 -0700
categories: python opensource api
---

# Hello Idea!

I've decided to start an open source project and fill another repository on my GitHub. Since I've been working more on the backend side and building services/REST APIs, I've had an urge to dedicate myself in this area. I thought it would a wonderful area to provide tooling for REST API development in Python.

# The Concept

I thought be interesting to create a framework agnostic library that provides helper methods/classes for handling data, particularly handling Python dictionary data and encoding/decoding JSON data in an elegant way. 

At the base level, I wanted the library to have two classes: 
- Transform  --> a helper for formatting a json schema(s).
- Collection --> a helper for handling python dictionary collections.

Perhaps I can combine these into one class and handle a dictionary collection and json schema format. 

### Implementation Idea

Here's a basic example of how I would like this libary to work:

```python
from wavvy.transformer import Transform

data = Transform(some_dict, {
    'name' some_dict['name'],
    'key': some_dict['key'].lower(),
    'links': {
        'google': 'https://google.com',
        'twitter': 'https://twitter.com'
    }
})
```

A really basic example. Maybe the API could be named better. The Transform class could be named TransformJSON. Or! The `Transform` class could have methods that handle different types of schema types, such as XML. 

# Purpose

For fun and learing mainly. Plus it would be nice to use tools that I've personally built in the wild. Also, I find the `json.loads()` and `json.dumps()` methods in Python's default `json` module to be named oddly, and it would be nice to have some syntactic sugar/wrappers around them.

# Closing Points

Wanted to write about this project as a start it and will provide updates as I go along. I'm thinking of writing a post about this project when a milestone is reached or an idea has come up. You can follow the project on [Github](https://github.com/tysweezy/wavvy)