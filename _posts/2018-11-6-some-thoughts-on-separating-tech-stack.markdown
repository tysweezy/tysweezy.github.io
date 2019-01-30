---
layout: post
title:  "Some Philosophies on Separating Your Stack"
date:   2018-11-06 10:45:17 -0700
categories: ramblings software engineering
author: Tyler Souza
share: true
---

For web applications, it’s a common pattern to separate your backend (typically a REST API) from your front-end/user interface nowadays. It’s a practice that I’ve been using for all of my projects for the past year or so. 

I think this is a good idea for a number of reasons:
- Decouples your backend from your user interface.
- Multiple parties can access your backend data, such as, a mobile application, desktop application, other user’s who want to access your API, etc.
- Each end of the stack has it’s own responsibility. The front-end can focus on user-interface related things and the backend can focus on the data side of things.
- If you have a dedicated front-end and/or backend team,  you have two different repos in which those teams can dedicated the efforts towards.
- Reduces mixing configuration for both frontend and backend purposes.
- Many more to list.

## However…
However, there is nothing wrong with keeping the traditional monolith and having your frontend and backend within the same project. It really depends on your use case. If you are absolutely sure that you’re only going to be building a web application, then doing it the “traditional” way is perfectly fine. It could be easier and less costly to have the both ends of the stack within the same project and server.

Though, I still choose to separate the backend/frontend has my default because I don’t know what I’ll need in the future. Maybe I’ll need a mobile application to talk to my backend. Also, I don’t know what problems I’ll come across. Maybe I’ll need to scale? Maybe I’ll need different parties to access my data? Also, I just like the idea of separation itself. It just feels right. Reduces brain clutter for me. You can focus on one area at-a time.

## Ways to separate your stack

There are two common ways I’ve seen two stacks separated:
- Have both projects be on different repos but on the same server, just in different directories and different ports. This can be done with and [Nginx reverse proxy server](https://linode.com/docs/web-servers/nginx/use-nginx-reverse-proxy/).
- Or you can just have each end be on different servers.
	
## The Nginx Proxy Way
You could have your backend on the same server/domain and direct that traffic to that port using an [nginx reverse proxy](https://linode.com/docs/web-servers/nginx/use-nginx-reverse-proxy/). Here’s an example:
```
location /api/ {
  proxy_pass http://localhost:5000;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection 'upgrade';
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
}
``` 

If user’s hit `localhost:5000/api`, then it will direct to user to the backend server. Then you can configure your frontend  as you need to.

## The Different Server Way
I’d recommend going this route. Much easier to balance the load and scale your apps if both ends are running on a different server. Though, keep-in-mind that you may run into CORS issues, but that should be a simple issue to fix. You should also make sure you don’t allow all hosts to access your API in production.  Anyway, there are a number of services to choose from. DigitalOcean, Linode, AWS, Heroku, etc. just to name a few. 

## My Technology of Choice
I usually default to a [Node.js](https://nodejs.org/en/) (express) backend with [Vue](https://vuejs.org/) as my front-end. I use the [Vue-CLI](https://github.com/vuejs/vue-cli) to quickly spin up a new front-end project. 

I also typically use [Laravel](https://laravel.com/) as my API backend sometimes. I don’t go straight to [Lumen](https://lumen.laravel.com/) unless speed is a concern. I usually keep everything, especially blade templates in case I need them. Just a suggestion, but you could use Laravel’s front end stuff for API docs, admin settings, [Swagger](https://swagger.io/) for API docs, etc. 

However, it typically doesn’t matter what stack I’m in. I can work with [Go](https://golang.org/) or [Django/Python](https://www.djangoproject.com/) backend. I can hang out with [React](https://reactjs.org/), [Angular](https://angular.io/) and friends. I just stick to the overall concept. 

## Closing Points
I personally think that separating your stack is a smart idea. It just makes sense to me. For scalability reasons, for development reasons, etc. I think it’s a great pattern to follow for most use-cases. Though, it may not fit every use-case. Which brings me to my last point, at the end-of-the-day, it really depends on the cards you’re handed with and the problem you are trying to solve. 

_I don’t have a comment section. However you can come chat with me on [Twitter](https://twitter.com/tysweezy)!_

