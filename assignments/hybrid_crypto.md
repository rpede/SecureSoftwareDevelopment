# Hybrid crypto assignment

Most real world applications use a mixture of asymmetric and symmetric
cryptography.

The exact details of how this is done depends on the specific problem.

Generally we use asymmetric (public-key) crypto agree on a shared (symmetric)
key which is then used to encrypt and decrypt the data.

**Level 1**

Write a program that uses hybrid cryptography to send messages.

Use asymmetric cryptography in combination with key exchange to share a session
key.
Then use symmetric cryptography to an exchange message using the session key.

I recommend Elliptic-curve Diffie–Hellman (ECDH) for key exchange and
AES-GCM-256 for messages.

1. Exchange public keys
2. On both ends, derive a key from the original key pair and the partners public key
3. One side generates a session key, encrypts it with derived key
4. Partner decrypts the session key using derived key
5. Use session key to encrypt+decrypt message

Use whatever programming language you want.

**Level 2 (optional)**

Modify your program such that you can exchange messages over a [TCP
socket](https://www.geeksforgeeks.org/socket-in-computer-network/) or
[WebSocket](https://www.geeksforgeeks.org/what-is-web-socket-and-how-it-is-different-from-the-http/).

You will need to program.
One that listens for connections (server).
And another that connects to the first (client).

Work out how to encode the exchange and what order to send/receive.

Pay attention that parameters used for cryptographic operations on both ends
work together. Including to padding scheme.

There are different standard formats for keys/certificates.
An example is X.509 with ASN.1-DER encoding.
Find out how you can export the public key in a format that can be sent over
the socket and imported at the other end.

- [TCP Overview in .NET](https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/sockets/tcp-classes)
- [Python - Socket Programming HOWTO](https://docs.python.org/3/howto/sockets.html)
- [A Guide to Java Sockets](https://www.baeldung.com/a-guide-to-java-sockets)
- [Socket.IO (JavaScript)](https://socket.io/)

