# Threat modeling assignment

## Introduction

Passwordify is a password management service from BestVPN Inc.

BestVPN is best known for their VPN service.
Since the company was founded the VPN space has become a lot more competitive.
To gain a competitive advantage they recently started to offer a password and
secret management they call Passwordify to all their existing customers.

## Security requirements

They main security requirements are:

- Keep credentials protected from un-authorized access.
- All data must be encrypted [at rest](https://en.wikipedia.org/wiki/Data_at_rest) (on servers).

## Sub-systems

Passwordify consist of the following sub-systems:

- Desktop client
- Mobile client
- REST API
- Web UI

The source code for all sub-systems is kept in a
[monorepo](https://en.wikipedia.org/wiki/Monorepo).

Both desktop and mobile clients are build from the same .NET MAUI codebase.

The web UI is a PHP application, since that is the technology their frontend team
is most familiar with.

REST API is a Node+Express application using Mongoose to communicate with a
MongoDB database.
The backend team was worried about SQLi so they decided to go with a NoSQL
database.
They also thought NoSQL sounded cool and that a document store was fitting for
their data model.

The API supports both cookie based authorization and JWT in custom header.
Cookies are used for authorization of the web UI.
JWT for mobile and desktop client.

An Apache web server serves the PHP UI and acts as a reverse proxy for the REST
API.
They expect that the number of users will grow over time, in which case they will
eventually reconfigure Apache to also act as a load balancer.

All systems are hosted on-premise with in-house servers.
They are behind a firewall with only HTTP and FTP traffic allowed from the internet.
Firewall rules are relaxed from internal company network, since operations team
needs full access to manage the servers.

## Login

Users can login with either email or username and password.
Password is compared against stored password hash using bcrypt (work factor 10).

## Operations

Authenticated users can:

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

This is the database model for the system.

**User document**

| Field | Type |
|-|-|
| _id | ObjectId |
| firstName | String |
| lastName | String |
| username | String |
| email | String |
| passwordHash | String |

**Vault document**

| Field | Type |
|-|-|
| _id | ObjectId |
| userIds | Array (user reference) |
| credentials | Array (embedded Credential document) |

*userIds* are the IDs of users that can access the vault.
A vault can be shared with other users by adding them to the array.

**Credential embedded document**

| Field | Type |
|-|-|
| name | String |
| siteUrl | String? |
| username | String |
| password | String |

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
The web UI and REST API are uploaded as [tarballs](https://en.wikipedia.org/wiki/Tar_(computing)).

A [cron](https://en.wikipedia.org/wiki/Cron) job looks for new version of the
web UI and REST API.
If it sees a new version it will download and extract the archive on the server
replacing the existing application.

Deployment of Android and iOS haven't been automated yet.
So a developer have to manually build, sign and upload those to their
respective stores.
