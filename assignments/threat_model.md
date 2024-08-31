# Threat modeling assignment

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
- REST API
- Web UI

The source code for all sub-systems is kept in a
[monorepo](https://en.wikipedia.org/wiki/Monorepo).

The plan is to build both desktop and mobile clients from the same .NET MAUI
codebase.

The web UI will be a PHP application, since that is what they use for the main
company site.

The REST API will be a Node+Express application using Mongoose to communicate
with a MongoDB database.

The back-end team is worried about SQLi, so they decided to go with a NoSQL
database.
They also thought NoSQL sounded cool and that a document store was fitting for
their data model.

The API supports both cookie based authorization and JWT in custom header.
Cookies are used for authorization of the web UI.
JWT for mobile and desktop client.

They will use their existing Apache web server to serve the web UI for
Passwordify, and it will also act as a reverse proxy for the REST API.
They expect that the number of users will grow over time, in which case they
will eventually reconfigure Apache to also act as a load balancer.

All systems are hosted on-premise with in-house servers.
They are behind a firewall with only HTTP and FTP traffic allowed from the internet.
Firewall rules are relaxed from internal company network, since operations team
needs full access to manage the servers.

## Functional requirements

### Registration

In order to ease adoption, they want users to be able to user their existing
login, and then automatically create a password vault for the user first time
they access the new Passwordify feature.

### Login

Users should be able login with a combination of password and then either email
or username.
The login process should look identical across clients (web, mobile & desktop).

Side-note: A SHA512 of the password is currently stored in a MySQL database,
used by the VPN service.

### Operations

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

## Database

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

Apply appropriate threat modeling technique.

Document potential threats.

Should the security requirements be changed?

Document how you would verify that threats have been addressed before release.

In the real world you would verify that all applicable threats have been
addressed before release.
But, to limit the scope of this assignment, you are only required to document how
to verify for some of the threats you find.

Focus on Passwordify.
Don't threat model the VPN service.

## Hand-in

Accepted file types are PDF, SVG, PNG and JPG.

Should include relevant diagrams, list of applicable threats + how to address.
It must be clear which part of the diagram a threat relates to.

Also include any refinements to the security requirements.

[Learn about security
requirements](https://www.synopsys.com/blogs/software-security/software-security-requirements.html)

