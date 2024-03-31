# Authorization

## Introduction

Many tutorials and framework documentation often only describe authorization
so far as to allow or deny based on whether a user has authenticated or not.
Maybe they do a bit of role-based access control.
But that's where things usually stop.

Simply checking that a user is authenticated is not enough for a real world
application.

Following the principle of least privilege we almost always need more fine
grained control.

The aim of this assignment is for you to explorer this field within your
tech-stack.

## Context

Imagine you are working on the backend for a news site.
The site contains news articles written by journalists.
Editors correct factual mistakes in articles and remove hateful comments.

The business model rely on subscription fees and advertising.
Subscribes can for a small monthly fee access a version of the site without
adds.
Guests can still read articles published on the site, but will be presented with
ads.

## Policy

Your task is to implement authorization as a proof-of-concept that enforces the
following policy.

- Editor can:
    - Edit, and delete articles.
    - Edit and delete user comments.
- Writer / Journalist can:
    - Create and edit their own articles.
- Subscriber / Registered User can:
    - Comment on articles.
- Guest / Public User can:
    - Read articles.

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

## Bonus

You get 5 bonus points if I can run the code in GitHub Workspaces.
It supports:

- dotnet 8.0.203
- Python 3.10.13
- node v20.11.1
- sqlite3 3.41.2
- Docker 24.0.9-1

Please include script to generate tables.

## Limitations

As said, this will be a proof of concept.
Focus is on authorization / access control.

I don't expect you to do anything fancy regarding authentication (that's for
another lesson).

You can use either:

- [HTTP "Basic" authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)
- [JWT Bearer](https://jwt.io/introduction/)
- [Session Cookie](https://cyberchimps.com/blog/session-cookies/)

Frontend isn't required, nor expected.
A simple REST API is fine.

I will accept solutions using in-memory data storage like Dictionary (C#),
HashMap (Java) and Dict (Python).
Though a database is preferred.

## Deliverable

Link to GitHub repository with the code for your solution and README with
commands to get started.