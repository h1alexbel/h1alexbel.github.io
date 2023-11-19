---
layout: post
date: 2023-11-19
place: Minsk, Belarus
title: "HTML Templating on Top of RESTful API"
tags: [ web, productivity ]
---

Designing a beautiful frontend requires HTML and CSS only.
But while constructing some app, eventually we end up with 
[SPAs](https://en.wikipedia.org/wiki/Single-page_application) or
[MPAs](https://asperbrothers.com/blog/spa-vs-mpa/).
Why?
I think we can simplify all of this stuff, going "back"
to templated HTML pages, rendering all your objects there.

<!--more-->

<img src="/assets/images/2023/11/broen-sourcing.png">

## Problem

Nowadays, all modern Frontend frameworks are completely
displaced HTML templates we had in the MVC era.

What have we got?
* Components
* Handlers
* Services and other great code bloaters.

All of them are help only to violate Thin Client Pattern
by adding extra complexity on plain HTML most of us need it these days.

> By keeping all the code inside one simple monolith,
we can boost productivity and reduce the time
required to deliver full-stack features to the end-users.

The main problem is that this extra complexity is not required.
For most of us.
And we want to reduce delivery time by utilizing the power
of a team that consists just of a few programmers and designers.
The team where each programmer can deliver full-stack
features.
Right from the backend to the Frontend part.

## What's Wrong With MVC?

No SPAs/MPAs, lets get back to MVC!
Our backend now will be responsible for aggregating objects and
placing them into HTML Templates and rendering nice HTML pages.
Plain Old MVC!

Don't rush it.

MVC is not the best here, I believe.
Well, HTML Templating is a good idea,
it will improve your app from simplicity side, 
and so, but ~~most~~ all modern Web Frameworks will force you
to not have an API in that case.
Instead, you will have a Controllers that return
you HTML templates, you will [manage](https://www.yegor256.com/2016/12/13/mvc-vs-oop.html) your model
to place all its fields to that template.

The result of that is that other apps cannot integrate you,
only [crawl](https://en.wikipedia.org/wiki/Web_crawler), maybe.

Ideally, application should expose both interfaces:
Web pages to the end-users, a.k.a User Interface,
and an API, objects that your application stores/handles/sources.
That's the separation of concerns.
That's the best scenario.

What options do we have for this one?
For now, I think we have only one option that will do this:
[XML+XSLT+HTML](https://www.yegor256.com/2014/09/09/restful-web-sites.html).

And that one is not ideal either.
A way more complex, and ~~there is no JSON~~ have certain limitations.

## Conceptual Solution

Imagine you have an Application, written in Java, that needs to be able to render users.
First, let's define a RESTful endpoint that will return us JSON response:
```text
GET /api/v1/users
```
And write some code for our "controller" (it is a conceptual code from [@l3r8yJ/towers](https://github.com/l3r8yJ/towers)):

```java
new ApiRouteOf("/api/v1/users", new JsonUsers(...)).serve();
```

Running our app, this endpoint should respond to us with JSON like this:

```json
[
  {
    "email": "test1@test.com",
    "id": "4b53a171-139c-4808-b61c-fcad879b5084"
  },
  {
   "email": "test2@test.com",
   "id": "a3303252-343c-4c4f-baf1-f4f5a6f333a4"
   }
]
```

now we have a RESTful endpoint serves User Resources as JSON,
that we can integrate, process it somehow else, and so on.

And now we need some View/User Interface for this data.
We can do this by "wrapping" the API endpoint
and placing Templates on top of it.
Something like this:

```java
new Templated(
  "/users.html",
   new ApiRouteOf(
     "/api/v1/users",
      new JsonUsers(...),
      new Render("users")
    )
).serve();
```

Finally, we create an HTML Template `users.html`:
```html
<% users >
...
```

Thus, our App can serve both API to integrate and HTML View to the end-users.

This is a concept on which we are [working](https://github.com/l3r8yJ/towers/issues/8) right now.
What do you think?
Don't hesitate to submit your ideas in the comments and [here](https://github.com/l3r8yJ/towers/issues).
