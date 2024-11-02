# Threat modeling assignment

<!--toc:start-->

- [Threat modeling assignment](#threat-modeling-assignment)
  - [Introduction](#introduction)
  - [Security requirements](#security-requirements)
  - [Sub-systems](#sub-systems)
    - [Database](#database)
    - [Authentication & Authentication](#authentication-authentication)
    - [Hosting](#hosting)
  - [Functional requirements](#functional-requirements)
    - [Registration](#registration)
    - [Login](#login)
    - [Requirements](#requirements)
  - [Database schema](#database-schema)
  - [What you need to do](#what-you-need-to-do)
    - [1. Diagram](#1-diagram)
    - [2. Identify threats](#2-identify-threats)
    - [3. Address the threats](#3-address-the-threats)
  - [Hand-in](#hand-in)
  <!--toc:end-->

## Introduction

Passwordify is a password management service from BestVPN Inc.

The main product of BestVPN is their VPN service.
Since the company was founded, the VPN space has become a lot more competitive.
To gain a competitive advantage they are planning to offer a password
management solution as an add-on to their VPN subscription.
They will call this new product "Passwordify".

## Security requirements

The main security requirements are:

- Keep credentials protected from unauthorized access.
- All data must be encrypted [at rest](https://en.wikipedia.org/wiki/Data_at_rest) (on servers).

## Sub-systems

Passwordify will consist of the following sub-systems:

- Desktop client
- Mobile client
- Web UI
- Authorization server
- REST API
- Document store (MongoDB)

The source code for all sub-systems is kept in a
[monorepo](https://en.wikipedia.org/wiki/Monorepo).

The plan is to build both desktop and mobile clients from the same .NET MAUI
codebase.

The web UI will be a PHP application.
Since that is what they use for the main company website.

The REST API will be a Node+Express application using Mongoose to communicate
with a MongoDB database.

### Database

The back-end team is worried about SQLi, so they decided to go with a NoSQL
database.
With all the talk that has been around NoSQL, they thought of it as future
proofing the system.
Out of the different types NoSQL database, they thought a document store was
the most fitting for their data model.

### Authentication & Authentication

The API supports both cookie based authorization and JWT in custom header.
Cookies are used for authorization of the web UI.
JWT is for mobile and desktop client.

The JWTs are issued by the authorization server using OAuth 2.0 authorization
code flow.
The signature is validated in REST API using a shared secret (symmetric
signature).

### Hosting

They will use their existing Apache web server to serve the web UI for
Passwordify.
The server is currently used to serve their WordPress based marketing site.
It will also act as a reverse proxy for the REST API hosted on a separate Nginx
server.
They expect that the number of users will grow over time, in which case they
will eventually reconfigure Apache to also act as a load balancer.

All systems are hosted on-premise with in-house servers.
They are behind a firewall with only HTTP and FTP traffic allowed from the internet.
Firewall rules are relaxed from internal company network, since operations team
needs full access to manage the servers.

## Functional requirements

### Registration

In order to ease adoption, they want users to be able to user their existing
login.
It will automatically create a password vault for the user first time they
access the new Passwordify feature.

### Login

Users should be able login with a combination of email/username and password.

For web, login is done directly through the REST API which sets a session
cookie.

For mobile and desktop, the login process happens through the authorization
server.

Side-note: A SHA512 of the password is currently stored in a MySQL database,
used for their VPN service.

### Requirements

Authenticated users should be able to:

- Retrieve a list of names and vault IDs of vaults they are allowed to access.
- Retrieve entire vault including credentials from vault ID.
- Search for other users to facilitate vault sharing.
- Create new vault.
- Update vault they have access to.
- Delete vault.

Unauthenticated users can:

- Create an account.
- View status page.

## Database schema

This is the intended database model.

**User table in MySQL**

This table is shared with the VPN service.

| Field        | Type              |
| ------------ | ----------------- |
| id           | int (primary key) |
| firstName    | varchar(255)      |
| lastName     | varchar(255)      |
| username     | varchar(100)      |
| email        | varchar(100)      |
| passwordHash | binary(64)        |

**Vault document**

| Field       | Type                                       |
| ----------- | ------------------------------------------ |
| \_id        | ObjectId                                   |
| userIds     | Array (user reference)                     |
| credentials | Array (embedded vault Credential document) |

_userIds_ are the IDs of users that can access the vault.
A vault can be shared with other users by adding them to the array.

**Vault Credential embedded document**

| Field    | Type    |
| -------- | ------- |
| name     | String  |
| siteUrl  | String? |
| username | String  |
| password | String  |

The engineers didn't see a need to encrypt vaults or credentials individually
since [full disk encryption](https://en.wikipedia.org/wiki/Disk_encryption#Full_disk_encryption)
is used on the database server.

## What you need to do

Form a group of 3-5 members.

Imagine you are a developer at BestVPN Inc.
Apply appropriate threat modeling technique to threat model the system
Passwordify described above.

### 1. Diagram

Draw a DFD of the Passwordify.
Diagram in multiple layers as you find appropriate in order to capture
meaningful details, without making it too intricate.

### 2. Identify threats

Use either STRIDE or attack trees to find potential threats.
Refer to the diagram(s) in part 1.

### 3. Address the threats

For each threat you've found in part 2, write down how you think the company
should deal with the threat.

Should the security requirements be changed?

## Hand-in

Hand-in a PDF with your threat modeling.
The PDF should include diagrams, threats found and how they could be addressed.

Remember to number elements in your diagram, so you can refer to it in your
list of threats.
Each threat must also have a number.

Also include any refinements to the security requirements that you think will
improve the overall security posture of the system.

[Learn about security
requirements](https://www.synopsys.com/blogs/software-security/software-security-requirements.html)
