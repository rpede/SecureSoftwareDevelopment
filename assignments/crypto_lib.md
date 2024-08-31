# Crypto library

## Introduction

I allow student to complete programming exercises in their programming language
of choice.

I can't provide examples in all possible programming languages.

Therefore, I want you to write your own code example for how to use various
cryptographic functions.

I will provide name and configuration, then you do some research on what
libraries or APIs to use in your favorite programming language.

Some functions will be built into the standard library.
But others, might require you to search for packages.

You will need to do some research on your own.

## Deliverables

I don't expect you to implement any of the algorithms on your own.
Just provide examples using existing implementations.

You will need to submit a PDF document with a code example for each algorithm
listed below.

Please include links to resources and libraries you use.


## Algorithms

### cryptographic random number generator (CRNG)

Provide a code example that use secure random generate to generate a 256 bit
value.

This value can be used as encryption key.

You can find some hints
[here](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html#secure-random-number-generation).

### Shared-key (symmetric) cipher

Provide and example of using **AES-256-GCM** or **AES-256-CBC** to encrypt and
decrypt a message.

That is [AES (Advanced Encryption
Standard)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
algorithm, with a key size of 256 bit and used in either
[Galois/counter (GCM)](<https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Galois/counter_(GCM)>)
or [CBC (Cipher block
chaining)](<https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_block_chaining_(CBC)>)
mode.

Your example should encrypt a message a decrypt it again.

Use previous code example to generate an encryption key.

You also need an IV (sometimes called Nonce).
It is usually 96 bit long.
Same IV is required for decryption.

You need to generate a new IV for each plain-text that you
encrypt.
Same IV shall not be used to encrypt multiple times with the same key.

**GCM** will both produce a cipher-text and an authentication tag.
You will need the authentication tag to decrypt.

**CBC** doesn't have an authentication tag.

### Hashing

Provide an example that uses **SHA512** to generate a hash of an input.

That is [SHA-2](https://en.wikipedia.org/wiki/SHA-2) with a size of 512 bits.

SHA256 is also commonly used, as a side note.

### Message Authentication Code (MAC)

Provide an example that have a function to generate a hash-based message
authentication (HMAC) using HMAC-SHA256 and a function to verify the HMAC.

You can generate a shared secret using the code from your first example.

### Diffie-Hellman key exchange

Provide an example of using Curve25519 ECDH key exchange for two parties to
compute the same shared secret.

That is [elliptic-curve diffie-Hellman
(ECDH)](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman)
with [Curve25519](https://en.wikipedia.org/wiki/Curve25519).

### Digital signatures

Provide an example of how to use Ed25519 to sign a message and verify its
signature.

Ed25519 is short for [EdDSA](https://en.wikipedia.org/wiki/EdDSA) with
[Curve25519](https://en.wikipedia.org/wiki/Curve25519).

### RSA (Rivest–Shamir–Adleman)

Provide an example of encrypting with RSA and decrypting.

### Key derivation

Provide an example of using PBKDF2-HMAC-SHA512 to derive a key from a password.

That is [PBKDF2 (Password-Based Key Derivation Function 2)](https://en.wikipedia.org/wiki/PBKDF2) using HMAC-SHA512 as the underlaying
algorithm.

