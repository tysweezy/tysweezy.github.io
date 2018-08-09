---
layout: post
title:  "Pondering About My Main Backend Techstack of Choice"
date:   2018-07-07 22:55:17 -0700
categories: javascript techstak
---
For about eight months or so, I decided to commit myself and focus on Node.js as my main backend techstack of choice. PHP/Laravel have treated me well and I've been loving Go, but I wanted to focus on one tech stack, which is now Node as my main go-to. There's just something about Javascript that I can't get away from. Maybe it's the new features coming out, it's functional programming nature and it's flexibility. Who knows.

## Reflection

I've been pretty happy with my choice to stick to Node as my primary stack. I mainly try to stick to Postgres as my database, express as my web framework of choice, etc. I tend to take the Go philosophy with me when I start a new project. I start very basic and small depending on my needs, then I work my way up as I go along. I find Node/Express to be very nice for this.

## Some Thoughts on TypeScript

TypeScript is a superset of JavaScript. You can think of it as "JavaScript with types." I've been using for a current project that I'm working on and it has been a joy to work with. You can transfer your existing Node application from JS to TypeScript by just changing to file extension from ".js" to ".ts." ([Check out migrating from JavaScript][1]) You can refactor as you need to and organize your application's architecture as it grows.

For the database end-of-things, I've been using an ORM called [Typeorm][2]. It seems to be quite new, however, a really solid ORM. It's very similar to [Django's ORM][3], [SQLAlchemy][4] and Laravel's Eloquent. In fact, it does support both the Active Record pattern and the Data Mapper pattern. Really neat since I haven't found an ORM I've liked when developing in Node.

## And Then There's Python and Django

I really love Python! I really do! It was my first language that I've fully dove into and understood. [The dev shop I currently work at][5] primarily specializes in Django development and I've used it quite a bit over here. I love it (Did I mention that?)! As far as Django, I love using it for rapid prototyping and the Django Rest Framework is really solid. Since I'm a REST API development junky, it's one of my favorite tools. I do want to make reusable Python/Django open source packages in the near future, just depends on needs and use-cases around a particular problem. I think I'll keep coming back to Python as one of my main languages, and that's just fine. :)

## Closing Thoughts

It's kind of hard to decide which tech stack I want to choose as my main. Maybe it's better to master both? Perhaps focus on mastering backend development instead of a particular stack? Perhaps switching between Node and Python is a good thing while playing around with Go on the side! Though which technologies I invest my time in has been on my mind a lot lately. What are your thoughts?

 [1]: https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html
 [2]: http://typeorm.io/
 [3]: https://docs.djangoproject.com/en/2.0/topics/db/
 [4]: https://www.sqlalchemy.org/
 [5]: https://www.bixly.com/