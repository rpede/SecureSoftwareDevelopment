# Mini Project

## Intro

Many services we use every day rely on passwords for security.
With 2fa not always being an option.
To protect yourselves from the inevitable data breach, it's important that you
use a unique strong password for each service.

Remembering so many passwords is not feasible for most people.
That's why password managers are handy.

A password manager helps the user keep track of credentials.
Generally, it requires a method of authenticating to "unlock", which provides
access to all stored credentials.

They also often provide mechanisms to help organize credentials and generate
strong passwords.

[More](https://en.wikipedia.org/wiki/Password_manager)

## Goal

The goal of this mini-project is to develop a simple password manager.

The password manager must be capable of safely storing credentials on a single
computer/device.
It should also provide functionality for generating strong passwords.

Credentials are stored in a "vault" which must be encrypted.
The encryption key should be derived from a master-password.
Remember to use a salt/IV.
The vault can either be stored in a database or just written to a plain file.

You can implement it as a console application or with a GUI.
If you make it as a web app then the key derivation and encryption must happen
on the client.

## Deliverable

Submit a link to a public repository with your code.
The `README.md` should include setup instructions.
Such as SDKs, how to install dependencies, how to compile etc.
