# Hybrid crypto assignment

Most real world applications use a mixture of asymmetric and symmetric
cryptography.

The exact details of how this is done depends on the specific problem.

Generally we use asymmetric (public-key) crypto agree on a shared (symmetric)
key with is then used to encrypt and decrypt the data.

**Level 1**

Write a program that uses hybrid cryptography to send messages.

Use asymmetric cryptography in combination with key exchange to share a session
key. Then use symmetric cryptography to a exchange message using the session
key.

I recommend Elliptic-curve Diffie–Hellman (ECDH) for key exchange and
AES-GCM-256 for messages.

1. Exchange public keys
2. On both ends a derived key from the original key pair and the partners public key
3. One side generates a session key, encrypts it with derived key
4. Partner decrypts the session key using derived key
5. Use session key to encrypt+decrypt message

Use whatever programming language you want.

**Level 2**

Pair up with a partner that completed level 1.
Modify your program such that you can exchange messages over a [TCP
socket](https://www.geeksforgeeks.org/socket-in-computer-network/) with your
partner at the other end.

Work out how to encode the exchange and what order to send/receive.

Pay attention that parameters used for cryptographic operations on both ends
work together. Including to padding scheme.

There are different standard formats for keys. An example is X.509 with
ASN.1-DER encoding.
It is important that you use the same format on both ends.

[TCP Overview in .NET](https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/sockets/tcp-classes)