# Mini Project

## Intro

Many services we use every day rely on passwords for security. With 2fa not
always being an option.
To protect yourselves from the inevitable data breach, it's important that
you use a unique strong password for each service.

Remembering that many passwords is not feasible for most people.
That's why password managers are handy.

A password manager helps the user keep track of credentials.
Generally, it requires a method of authenticating to "unlock", which provides
access to all stored credentials.

They also often provide mechanisms to help organize credentials and generate
strong passwords.

[More](https://en.wikipedia.org/wiki/Password_manager)

## Goal

The goal of this assignment is to develop a simple password manager.

The password manager must be capable of safely storing credentials on a single
computer/device.
It should also provide functionality for generating strong passwords.

Security and usability are the main priorities!

You are allowed to work in groups.
However, the requirements increase with the group size.

- 1 person: Focus on safely storing on a single device.
- 2 people: More emphasis on user interface - making it easy to use securely.
- 3 persons: Plan for how a user can access their credentials across devices.
- 4 persons: Implement a way to access credentials across devices.

I'm not expecting anything as advanced as in the papers you read.

Project is compulsory/mandatory!

## Scale

The project will span two weeks.

You are very welcome to work in groups.
But each person in the group needs to make a reasonable contribution to the
project.

During the project you must design, program and document the security features
of your product.
I expect that you will spend the majority of the time on programming.

I'm not going judge too harshly if the implementation has a couple of rough edges.
However, I will put a lot of emphasis on the security considerations.
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

### 1st week

Link to a GitHub repository with a `README.md` containing a description of your
security model.

Including diagrams will be appreciated.
You can use [draw.io](https://app.diagrams.net/) or some other tool.

Any graphics must be embedded within the `README.md`.

I won't be looking at the code at this point.

### 2nd week

Here you a link to your final solution.

You must submit a link to a GitHub repository.
The link must include commit hash.

The commit must include working code.

In the root of your repository there must be a `README.md` containing:

- Instructions to run the application.
- Screenshots of the product.
- Discussion about security of your product.
  - What do you protect against (who are the threat actors)
  - What is your security model (encryption, key handling etc.)
  - Any pitfalls or limitations in your solution.

Prioritize quality over word count!

Make sure your project is **public**, so I can access it!!!

## Link to a commit

Here is how you get a link to GitHub repository including a commit hash.

First navigate to your repository.
Then open the commit history by clicking on the "x Commits" button.

![](../gh_commit_history.png)

Then click the browse button.

![](../gh_browse_commit.png)

Copy link from address bar.

