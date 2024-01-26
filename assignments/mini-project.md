# Mini-project

## Intro

Many services we use everyday rely on passwords for security. With 2fa not
always being an option.
To protect yourselves from the inevitable data breach, it's important that
you use a unique strong password for each service.

Remembering that many passwords is not feasible for most people.
That's why password managers are handy.

A password manager helps the user keep track of credentials.
Generally it requires a method of authenticating to "unlock", which provides
access to all stored credentials.

They also often provide mechanisms to help organize credentials and generate
strong passwords.

[More](https://en.wikipedia.org/wiki/Password_manager)

## Goal

The goal of this assignment is to develop a simple password manager.

The password manager must be capable of safely storing credentials on a single
computer/device.

It should provide functionality for generating strong passwords.

Security and usability are main priorities!

You are allowed to work in groups.
However the requirements increase with the group size.

- 1 person: Focus on safely storing on a single device.
- 2 persons: More emphasis on user interface - making it easy to use in a secure way.
- 3+ persons: Plan for how a user can access their credentials across devices.

I'm not expecting anything as advanced as in the papers you read.

Project is compulsory/mandatory!

## Scale

The project will span two weeks.

You are very welcome to work in groups.
But each person in the group needs to make a reasonable contribution to the
project.

During the project you must design, program and document the security features
of your product.
I expect that you will spend majority of the time on programming.

I'm not going judge to harsh if the implementation has a couple of rough edges.
However I will put a lot of emphasis on the security considerations.
Both from a technical point of view and from a human aspect.

## Technical

You can implement it in whatever programming language, runtime and technology
you want to 😲.

You can store/manage passwords in a local database/vault. The file format is
up to you.

You must take measures to protect the database/vault, according to the **Protect
data at rest** principle.

You must argue for any cryptographic decisions you make.

Your application should also provide a mechanism to generate secure passwords.

## Deliverable

In addition to the code, your deliverable must include a discussion about the
security of your product.
You should argue for the security considerations you made and document any
pitfalls.

## Hand-in

Link to git repository.
The link must include commit hash.

I prefer GitHub, but will accept other platforms such as Bitbucket and GitLab.

In the root of your repository there must be a `README.md` containing:

- Instructions to run the application.
- Screenshots of the product.
- Discussion about security of your product.

I will put a lot of emphasis on the discussion during grading.
Prioritize quality over word count!

Make sure your project is public, so I can access it.