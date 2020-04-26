# Security Cryptographic Hash and Others

## Cryptographic Hash Algorithms

From Wikipedia:

> A hash function takes an input (or 'message', 'data') and returns a fixed-size alphanumeric string. The string is called the 'hash value', 'message digest', 'digital fingerprint', 'digest' or 'checksum'. The same input always produces the same hash value.

Even f(n) = n + 1 could be considered an extremely simple hash function for a number. The input is a `n` and `n + 1` is the hash value. However, it is not a good cryptographic hash function.

General hashes may not work well for cryptographic purposes. One example is CRC checksum.

The ideal cryptographic hash function has three main properties:

- It is extremely easy to calculate a hash value for any given data.
- It is extremely computationally difficult to deduce / retrieve the data given only the hash value.
- It is extremely unlikely that two slightly different messages will have the same hash. (hash collisions)

The difference between hashing and encryption / decryption is that encryption can be reversed by decrypting, using a specific key. However, hashing is designed to be a one-way function.

Does the hash value need to be sufficiently long enough? Yes. Due to [birthday attack that can discover hash collisions](https://www.youtube.com/watch?v=-SQq0vfzrrg).

### Why use a cryptographic hash?

Common cryptographic hash functions including:

- MD5. Warning: this one is not strong enough anymore!
- SHA-2. We use this to protect message integrity like in TLS/SSL
- bcrypt. This one is commonly used to store users' passwords in database.

<img src="backend-web-development/_media/hashing.png" alt="hashing" width="500"/>

(From: https://hackernoon.com/cryptographic-hashing-c25da23609c3)

### MD5 failure

> One basic requirement of any cryptographic hash function is that it should be computationally infeasible to find two distinct messages that hash to the same value. MD5 fails this requirement catastrophically; such collisions can be found in seconds on an ordinary home computer.

In 2004,it was shown that MD5 is not collision-resistant. Thus MD5 is not suitable for applications like SSL certificates or digital signatures that rely on this property for digital security.

### SHA-2 Hashing

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/DMtFhACPnTY?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Protecting Passwords

Websites store their user passwords in their databases. For security reasons, those passwords are not stored in plaintext. Instead, the values stored in the databases are actually the hashes of the original passwords, generated using cryptographic hash functions like bcrypt.

When you login those websites, you fill in your username and password in your browser, and those credentials are sent to the server side for verification. At that time, the web server would compute the hash value of the passwords you submit, and compare it with the hash value stored in its database. When the two hash values match, the password authentication is considered successful.

If anyone gains access to the database, they will see the hashes of the passwords, not the actual passwords.

### Keyed-hash (MAC)

Keyed-hash is a function that takes a message (of arbitrary length) and a secret key as input and output a fixed size MAC (message authentication code).

Without knowing the key, it is difficult to forge the MAC.

Popular and secure keyed-hashes include CBC-MAC and HMAC.

### HMAC with SHA-2

Used together for TLS/SSL.
HMAC is obtained by running a cryptographic hash function (like SHA256) over the data to be authenticated and a shared secret key. As with any MAC, it may be used to simultaneously verify both the data integrity and the authentication of a message.

## Security goals with cryptography

Text adapted from https://crypto.stackexchange.com/questions/5646/what-are-the-differences-between-a-digital-signature-a-mac-and-a-hash.

We have three aims:

- Integrity: Can the recipient be confident that the message has not been accidentally modified?

- Authentication: Can the recipient be confident that the message originates from the sender?

- Non-repudiation: If the recipient passes the message and the proof to a third party, can the third party be confident that the message originated from the sender?

| Security        | Hash | MAC       | Digital signature |
| --------------- | ---- | --------- | ----------------- |
| Integrity       | Yes  | Yes       | Yes               |
| Authentication  | No   | Yes       | Yes               |
| Non-repudiation | No   | No        | Yes               |
| Kind of keys    | none | symmetric | asymmetric        |

For MAC, receiver can forge any message the sender sends as the key is shared.

## Other Use Cases of Cryptography

### Blockchain

Blockchain is a [distributed ledger](https://medium.com/@vijay.betigiri/blockchain-explained-like-im-5-yrs-5f04b91b059c) that ensures all the information recorded in the system are never modified. This is achieved by using some of the encryption/decryption algorithms such as RSA.
