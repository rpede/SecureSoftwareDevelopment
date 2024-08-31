# Threat modeling assignment

## Introduction

Passwordify is a password management service from BestVPN Inc.

BestVPN is best known for their VPN service.
Since the company was founded the VPN space has become a lot more competitive.
To gain a competitive advantage they are planning to offer a password management
solution as an addon to their VPN subscription.
They call this new product "Passwordify".

## Security requirements

They main security requirements are:

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

The backend team is worried about SQLi, so they decided to go with a NoSQL
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

## Exercise 1

In groups apply appropriate threat modeling technique for Passwordify.

## Exercise 2

Include the supply chain described below in your threat modeling.

## Supply chain

The various sub-systems are build from the monorepo by a self-hosted Jenkins
server.
Jenkins build is triggered by commits to main branch.
All developers have full access to version control system which is also
self-hosted.

The company trust the competence of their developers. So if it builds, it gets shipped.

Build artifacts for all the subsystem is automatically uploaded to the company
FTP server.
Only the installation for desktop client is linked to from the company website.
The web UI and REST API are uploaded as [tarballs](<https://en.wikipedia.org/wiki/Tar_(computing)>).

A [cron](https://en.wikipedia.org/wiki/Cron) job looks for new version of the
web UI and REST API.
If it sees a new version it will download and extract the archive on the server
replacing the existing application.

Deployment of Android and iOS haven't been automated yet.
So a developer have to manually build, sign and upload those to their
respective stores.
