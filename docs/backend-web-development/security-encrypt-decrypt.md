# Security Encryption/Decryption

What we are going to cover:

- Principles of a good cipher
- Simple attacks
- Symmetric encryption/decryption
- Asymmetric encryption/decryption

## Definitions

**Encryption** is converting **plaintext** (original message) into **ciphertext** (coded form of the message).

**Decryption** is reversing the process, converting **ciphertext** into **plaintext**.

A **cipher** is an algorithm for performing encryption or decryption.

Why must we learn this?
We can easily just use whatever industry-standard algorithms without really understanding them. However, we will like to understand the algorithms so that we know what we are using, why we are doing this and how we can better use the algorithms in the correct way.

There are two major category of encryption/decryption algorithms, symmetric and asymmetric.

## Symmetric Encryption / Decryption

Symmetric encryption/decryption, where one secret key is used for both encryption and decryption. That means anyone with the correct secret key can decrypt the cipher text back to plain text correctly. Some of the well known symmetric encryption/decryption algorithms include:

- Triple DES
- AES

A secret key is just a number. AES can work with keys of three different sizes, 128 bits, 192 bits, and 256 bits. When we say AES-128, AES-256, we are referring to the size of the key as AES is actually always a 128-bit cipher.

The main problem with symmetric encryption is, how can the sender and receiver agree on a key securely on the Internet? If we send the key through email, it might be intercepted by a third-party. We cannot send a key over a public channel.

We shall try to solve this problem with asymmetric encrypion.

## Asymmetric Encryption / Decryption

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AQDCe585Lnc?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Asymmetric encryption/decryption, where there are two keys used for encryption/decryption, one of them is called private key, the other is called public key. There is a unique relationship between the two keys:

- A message encrypted by a public key can be decrypted by the corresponding private key

- A message encrypted by a private key can be decrypted by the corresponding public key

The owner of the private key should keep it as a secret and never share it with other people. In contrast, the public key can be shared with other people. Then the owner of the private key can encrypt a message with his/her private key and anyone holding the public key can decrypt the message.

When decryption is done successfully, people can also confirm that the message is created by the person holding the private key. This is the foundation of digital signature

One of the well known asymmetric encryption/decryption algorithms is called RSA.

The problem with asymmetric is that it is not as fast as using the symmetric algorithm to encrypt and decrypt.

## Asymmetric cryptography with symmetric key

Due to the problems of each type of cryptography, symmetric key (session key) is often used with the asymmetric public / private key cryptography.

Let's say Amy wants to establish a secure connection with Bob. She wants to send over the session key. However, she needs a secure way to send over the key. She uses the session key to encrypt the plain text. Then she uses Bob's public key to encrypt the session key.

Bob can now decrypt the session key using his private key. Now he has the session key, and he can decrypt the ciphertext to obtain the plain text.

Since asymmetric cryptography is used to encrypt the session key and not the entire plain text, it would take less time.

## Principles of a good cipher

Shannon's confusion and diffusion are two properties of a secure cipher.

### Confusion

Relationship between ciphertext and key is obscured.

One aim of confusion is to make it very hard to find the key even if one has a large number of plaintext-ciphertext pairs produced with the same key

Otherwise we can break the encryption using **ciphertext-only attack** (known-ciphertext attack)

This property makes it difficult to find the key from the ciphertext and if a single bit in a key is changed, most or all the bits in the ciphertext will be affected.

### Diffusion

Relationship between ciphertext and plaintext is obscured.
The influence of one plaintext bit is spread over many ciphertext bits.

## Simple attacks

### Ciphertext-only attack

cipher

### Known-plaintext attack

#### Frequency analysis

Otherwise we can break the encryption using **known-plaintext attack**
