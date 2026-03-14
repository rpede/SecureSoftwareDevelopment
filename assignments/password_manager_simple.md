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

Credentials are stored in a "vault" which must be encrypted using.
For that you can use a symmetric encryption algorithm such as AES with an
appropriate mode of operation such as CBC or GCM.
The encryption key should be derived from a master-password using a key
derivation function such as Argon2id, Script, Bcrypt or PBKDF2.
Tweak the parameters to make the key derivation slow enough that it becomes
infeasible to crack, but not that slow that it becomes an annoyance to the
user.
Remember to use a salt/IV that is generated with a secure random number
generator.
The vault can either be stored in a database or just written to a plain file.

You can implement it as a console application or with a GUI.
If you make it as a web app then the key derivation and encryption must happen
on the client.
Please don't use Blazor for this project, because it is not obvious, unless you
know it really well, whether code is executed on client or server.

## Feedback

I will publish the source code for two different implementations by the end of
the project.
You can then compare your solution against mine.

If you want feedback your solutions, then submit a link to a public repository
with your code.
The `README.md` should include setup instructions.

I will only attempt to run your code [GitHub
Codespaces](https://github.com/features/codespaces).
Here are some info about the environment as of 2026-03-14:

```
$ cat /etc/issue
Ubuntu 24.04.3 LTS \n \l
$ dotnet --version
10.0.100
$ java --version
openjdk 25.0.1 2025-10-21 LTS
OpenJDK Runtime Environment Microsoft-12574222 (build 25.0.1+8-LTS)
OpenJDK 64-Bit Server VM Microsoft-12574222 (build 25.0.1+8-LTS, mixed mode, sharing)
$ node --version
v24.11.1
$ python --version
Python 3.12.1
$ docker --version
Docker version 28.5.1-1, build e180ab8ab82d22b7895a3e6e110cf6dd5c45f1d7
```
