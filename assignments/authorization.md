# Authorization

## Introduction

Many tutorials and framework documentation often only describe authorization
so far as to allow or deny based on whether a user has authenticated or not.
Maybe they do a bit of role-based access control.
But that's where things usually stop.

Simply checking that a user is authenticated is not enough for a real world
application.

Following the principle of least privilege we almost always need more
fine-grained control.

The aim of this assignment is for you to explorer this field within your
tech-stack of choice.

## Context

Imagine you are working on the back-end for a news site.
The site contains news articles written by journalists.
The site have editors which can correct factual mistakes in articles and remove
hateful comments.

The business model rely on subscription fees and advertising.
Subscribers can, for a small monthly fee access a version of the site without
adds.
Guests can still read articles published on the site, but will be presented with
ads.

(Implementing ads are out-of-scope)

You can use whatever web-framework you like to implement the solution.

## Policy

Your task is to implement authorization as a proof-of-concept that enforces the
following policy.

- Guest / Public User can:
  - Read articles and comments.
- Subscriber / Registered User can also:
  - Create comments on articles.
  - Update and delete comments on articles.
- Editor can:
  - Read, edit, and delete articles.
  - Read, edit and delete user comments.
- Writer / Journalist can:
  - Read all articles.
  - Create, edit and delete their own articles.
  - Delete user comments on their own articles.

## Schema

You can use this (SQLite) schema as your starting point.

```sql
CREATE TABLE IF NOT EXISTS users (
    user_id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
);

CREATE TABLE IF NOT EXISTS articles (
    article_id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL,
    author_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (author_id) REFERENCES users(user_id) ON DELETE CASCADE
);

CREATE TABLE IF NOT EXISTS comments (
    comment_id INTEGER PRIMARY KEY,
    content TEXT NOT NULL,
    article_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (article_id) REFERENCES articles(article_id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```

- `articles.author_id` is the id of the writer/journalists who wrote the article.
- `comments.user_id` is the id of the subscriber who wrote the comment.

You can also create your own schema instead.
You are not required to use a SQL database.

It is also acceptable to use an in-memory data storage like Dictionary (C#),
HashMap (Java) and Dict (Python) instead of a database.
Though a database is preferred.

## Limitations

Implementing an advertising system is out of scope.
Focus on CRUD endpoints and authorization policies.

As said, this will be a proof of concept.
Focus is on authorization / access control.

I don't expect you to do anything fancy regarding authentication (that's for
another lesson).

You can use either:

- [HTTP "Basic" authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)
- [JWT Bearer](https://jwt.io/introduction/)
- [Session Cookie](https://cyberchimps.com/blog/session-cookies/)

Front-end isn't required, nor expected.
A simple REST API is fine.

## Tips

You are allowed to user GenAI (ChatGPT, Claude, Gemini, Copilot etc) to create
the endpoints.
You might need to take it step-by-step.
Start with data-access.
Then CRUD endpoints for articles.
Last, CRUD endpoints for comments.
**Do not** prompt it to implement authorization/access-control.
The whole point is that you should gain a good understanding on how to
implement authorization rules in your preferred framework.
Referrer to the documentation for your framework rather than prompting.

If you use .NET for back-end you can take advantage of [ASP.NET Core
Identity](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity-api-authorization).

Using something like [Swagger UI](https://swagger.io/tools/swagger-ui/) or
[Scalar](https://scalar.com/) to test the API, is a good idea if your framework
of choice supports it.
Alternatively you could use Postman or write integrations test to validate your
authorization polices.

## Deliverable

Link to GitHub repository with the code for your solution and README with
commands to get started.

It should be possible to start the application just by cloning the repository and
running `docker compose up`.
Or if you use the starter project with `dotnet run`.

`README.md` should contain an example of interacting with the API.

Ideal group size is 2 persons.

## Summary

Implement a web-API enforcing the described access-control policies.

Make it simple to evaluate your solution.

If I have to fiddle around with things to get your solution running or figure
out how to interact with the API, then I'm not going to evaluate it.
