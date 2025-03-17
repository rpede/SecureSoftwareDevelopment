# Mini Project

## Background

There are many reasons why it can be important to keep communication
confidential.
Here are some examples where a lot is at stake if private conversation gets
exposed to the wrong people.

- Director of a company negotiating a merge with another.
  - Disclosure could: Affect stock prices. Allow competitor to gain an advantage position. Make the deal fall apart.
- Legal strategy of a lawyer in a high profile lawsuit.
  - Disclosure could: Give the opposing side an advantage, leading to losing the case.
- Managers discussion about a potential layoff round.
  - Disclosure could: Create panic among employees making them leave prematurely.
- An employee searching for new job openings.
  - Disclosure could: Result in termination.
- Spouse in an abuse marriage seeking help.
  - Disclosure could: Worsening of the abuse.
- Gang selling illegal drugs.
  - Disclosure could: Lead to gang members being arrested.
- Terrorist planing suicide attack.
  - Disclosure could: Lead to the terrorist being neutralized before the attack could be carried out.
- Gay rights activist in a country where same-sex relationship is illegal.
  - Disclosure could: Lead to harassment, jail time and (in some countries)
    execution.

The above list was made to illustrate that the use of cryptography in
communication is not truly black and white.
The point is that technology that gives strong protection can be used for good
and bad.
**Banning its use, does not prevent it from being used for bad.**
The technology exists, so we might as well use it for good.

I could make another list for where breach of integrity could have serious
consequences as well.
But, I'll leave it as a mental exercise for you to imagine.

## The project

Implement a chat/messaging application with focus on confidentiality and
integrity (security goals from CIA triad).

Given that you just learned about cryptography you should put your new gained
knowledge to use.

Consider ways that a chat protocol can be implemented such that it gives
reasonable protection against the security controls (integrity and
confidentiality), while still being feasible to implement as a
proof-of-concept, within the given time frame.

## Scope

Your solution should allow two-people to communicate with each other without the
risk of eavesdropping.
Group chats, where more than two people send/receive messages at a time, is out scope.

**Bonus** if your application allows for near real-time communication.
It can be achieved with WebSocket, socket.io, SignalR, TCP-socket, using a
message queue or so on.

I don't expect the UI to look fancy.
A simple unstyled HTML page, plain GUI or CLI/TUI is fine.
The only requirement regarding UI is that it should be obvious how to use it.

## Groups

Working in groups is encouraged, but I prefer no more than 4 people in a group.

## Hand-in

You must hand-in a link to git repository with your code before deadline.
The repository must be public.

The repository must have a `README.md` in the root containing instructions on how to run the solution.
It also needs to describe how you use cryptography to prevent eavesdropping and
why you chose to do it that way.

Please include screenshots of you solution in action.

## Real world chat apps

This is just to give you and idea about the real world landscape of encrypted
messaging and shouldn't be considered part of the assignment.
As solutions in the real world can be far more complex than what can be
expected from you during a short project like this.

Traditional many chat apps have messages encrypted from device to server (at
best).
It means that whoever runs the server can technical read peoples messages.
This setup makes it possible for intelligence agencies to get a warrant for
listening in on the traffic.
In corrupt countries this could be abused to crack down on protests.

Many chat apps these days have adopted end-to-end encryption (E2EE).
Meaning that messages remain encrypted all the way from senders device to
receives device.
It means that nobody including the company behind the service (assuming keys
are kept safe) can read the messages.
Therefore, intelligence agencies can't mandate a company to give them access to
messages.

Many apps that claim to do E2EE encryption doesn't have it for everything the
app supports.
One example could be having E2EE only for direct messages, but not for group
chats or stories.

There is also the concept of perfect backward and forward secrecy.
Backward secrecy means that if an attacker is able to break the encryption key
for a message somehow, then they would not be able to read previous seen
messages.
Related is forward secrecy, meaning that if a message key is broken, then the
attacker would not be able to read future messages between the parties.

The Signal protocol is by many considered to be state-of-the-art when it comes
to encrypted chat.
