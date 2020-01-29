# Security Cryptographic Hash and Others

## Cryptographic Hash Algorithms

From Wikipedia:

> A cryptographic hash function is a hash function which takes an input (or 'message', 'data') and returns a fixed-size alphanumeric string. The string is called the 'hash value', 'message digest', 'digital fingerprint', 'digest' or 'checksum'. The same input always produces the same hash value.

Even f(n) = n + 1 could be considered an extremely simple hash function for a number. The input is a `n` and `n + 1` is the hash value. However, it is not a good hash function.

The ideal hash function has three main properties:

- It is extremely easy to calculate a hash for any given data.
- It is extremely computationally difficult to deduce / retrieve the data given only the hash.
- It is extremely unlikely that two slightly different messages will have the same hash.

The difference between hashing and encryption / decryption is that encryption can be reversed by decrypting, using a specific key. However, hashing is designed to be a one-way function.

### Why use a cryptographic hash?

Common cryptographic hash functions including:

- MD5. Warning: this one is not strong enough anymore!
- SHA-2
- HMAC
- bcrypt. This one is commonly used to store users' passwords in database.

<img src="backend-web-development/_media/hashing.png" alt="hashing" width="500"/>

(From: https://hackernoon.com/cryptographic-hashing-c25da23609c3)

### Protecting Passwords

Websites store their user passwords in their databases. For security reasons, those passwords are not stored in plaintext. Instead, the values stored in the databases are actually the hashes of the original passwords, generated using cryptographic hash functions like bcrypt.
When you login those websites, you fill in your username and password in your browser, and those credentials are sent to the server side for verification. At that time, the web server would compute the hash value of the passwords you submit, and compare it with the hash value stored in its database. When the two hash values match, the password authentication is considered successful.

If anyone gains access to the database, they will see the hashes of the passwords, not the actual passwords.

## Other Use Cases of Cryptography

### SSL Certificates

When you visit a website, you may notice a green lock icon on the URL address bar. That indicates the website has a certificate to prove its identity.

Each certificate has a private key and public key. The web server hosting the website holds the private key, and the browsers download the certificates which contains the public key.

Then the browser can send a challenge (**nonce**) to the web server. The challenge is basically some random number encrypted with the public key of the certificate. If the web server holds the corresponding private key, it should be able to decrypt the challenge and return the correct random number to the browser.

Upon receiving the correct random number sent in the challenge, the browser can confirm the web server holds the correct private key, hence its identify (as declared in the certificate) can be trusted.

### HTTPS

Hypertext Transfer Protocol Secure (HTTPS) is an extension of the HTTP for secure communication over a computer network, and is widely used on the Internet. In HTTPS, the communication protocol is encrypted using Transport Layer Security (TLS), or formerly, its predecessor, Secure Sockets Layer (SSL). The protocol is therefore also often referred to as HTTP over TLS, or HTTP over SSL.
A website that supports HTTPS uses the public/private key associated with its SSL certificate to create a random number and share it with the browser. That random number is used as a secret to encrypt/decrypt the HTTP requests/responses between the browser and the server.

### Blockchain

Blockchain is a [distributed ledger](https://medium.com/@vijay.betigiri/blockchain-explained-like-im-5-yrs-5f04b91b059c) that ensures all the information recorded in the system are never modified. This is achieved by using some of the encryption/decryption algorithms such as RSA.
