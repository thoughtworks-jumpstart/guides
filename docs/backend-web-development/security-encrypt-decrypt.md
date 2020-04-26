# Security Encryption/Decryption

Cryptology (coming from the Greek words κρυπτός (kryptos) meaning "hidden" and -λογία (-logia) denoting "study of", and hence is the study of hidden writings) is a very broad subject. It is often split into two sections: Cryptography (where γράφειν (graphein) means "wriring") and Steganography (where στεγανός (steganos) means "covered'' or "protected").

Steganography is the hiding of a message by a physical means.

Cryptography is split into two ways of changing the message systematically to confuse anyone who intercepts it:

The task of the cryptographer is to create a system which is easy to use, both in encryption and decryption, but remains secure against attempts to break it.

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

## Principles of a good cipher

Shannon's confusion and diffusion are two properties of a secure cipher.

### Confusion

Relationship between ciphertext and key is obscured.

One aim of confusion is to make it very hard to find the key even if one has a large number of plaintext-ciphertext pairs produced with the same key.

This property makes it difficult to find the key from the ciphertext and if a single bit in a key is changed, most or all the bits in the ciphertext will be affected.

### Diffusion

Relationship between ciphertext and plaintext is obscured.
The influence of one plaintext bit is spread over many ciphertext bits.

The statistics of the plaintext is "dissipated" in the statistics of the ciphertext.

## Simple ciphers

### Monoalphabetic Substitution Ciphers

Substitution Cipher works by replacing each letter of the plaintext with another letter.

![substitution cipher](https://crypto.interactive-maths.com/uploads/1/1/3/4/11345755/4433929_orig.jpg)

Each letter is encrypted as the next letter in the alphabet: "a simple message" becomes "B TJNQMF NFTTBHF". They were used for a long time but are now very easy to break. They played a big part in developing cryptography.

### Caesar Shift Cipher

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/sMOZf4GN3oc?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Exercises

Let's code our own caesar shift cipher!

Julius Caesar protected his confidential information by encrypting it using a cipher. Caesar's cipher shifts each letter by a number of letters. If the shift takes you past the end of the alphabet, just rotate back to the front of the alphabet. In the case of a rotation by 3, w, x, y and z would map to z, a, b and c.

abcdefghijklmnopqrstuvwxyz => defghijklmnopqrstuvwxyzabc

Create 2 function, caesarCipher and decryptCaesarCipher

```js
expect(caesarCipher(“apple”, 3)).toBe(“dssoh”);
expect(decryptCaesarCipher(“dssoh”, 3)).toBe(“apple”);
expect(caesarCipher(“abcde-fghij”, 3)).toBe(“defgh-ijklm”); // non-alphanumeric characters like `-` should be left unchanged
```

## Simple attacks

### Ciphertext-only attack

We can use ciphertext-only attacks if we can figure out the plaintext or better still, the key from the ciphertexts.

#### Frequency analysis

Collecting many ciphertexts allow us to do frequency analysis to figure out the plaintexts.

### Known-plaintext attack

Otherwise we can break the encryption using **known-plaintext attack**.

Knowing both the plaintext and ciphertext, an analyst will be able to figure out the key or keys used.

It could be a brute force method: Try all keys, decrypt the ciphertext and see if it matches the plaintext. This always works for every cipher and will give you the matching key.

For every modern cipher like AES (with key sizes of 128 bit or more) the key space is so large that you need much more time (until the end of the time) to check a significant portion of all keys.

There are two major category of encryption/decryption algorithms involving keys, symmetric and asymmetric.

## Symmetric Encryption / Decryption

Symmetric encryption/decryption, where one secret key is used for both encryption and decryption. That means anyone with the correct secret key can decrypt the cipher text back to plain text correctly. Some of the well known symmetric encryption/decryption algorithms include:

- Triple DES
- AES

A secret key is just a number. AES can work with keys of three different sizes, 128 bits, 192 bits, and 256 bits. When we say AES-128, AES-256, we are referring to the size of the key as AES is actually always a 128-bit cipher.

Real-life use of AES includes [Encryption at Rest for databases](https://docs.mongodb.com/manual/core/security-encryption-at-rest/)

### AES

[Stickman AES Explanation](http://www.moserware.com/2009/09/stick-figure-guide-to-advanced.html)

### Initialization Vector (IV)

Having a unique IV per encrypted file / data is crucial.

The IV adds randomness to your start of your encryption process. When using a chained block encryption mode (CBC) (where one block of encrypted data incorporates the prior block of encrypted data) we're left with a problem regarding the first block, which is where the IV comes in.

If you had no IV, and used chained block encryption (CBC) with just your key, two files that begin with identical text will produce identical first blocks. If the input files changed midway through, then the two encrypted files would begin to look different beginning at that point and through to the end of the encrypted file.

If someone noticed the similarity at the beginning, and knew what one of the files began with, he could deduce what the other file began with. Knowing what the plaintext file began with and what it's corresponding ciphertext is could allow that person to determine the key and then decrypt the entire file.

Now let's add the IV. If each file used a random IV, their first block would be different. The above scenario has been thwarted.

Now what if the IV were the same for each file? Well, we have the same problem scenario again. The first block of each file will encrypt to the same result. Practically, this is no different from not using the IV at all.

Therefore you require a unique IV.

Answer adapted from https://stackoverflow.com/questions/9049789/aes-encryption-key-versus-iv

### Electronic Code Book (ECB) vs Cipher Block Chaining (CBC)

Disadvantage of ECB is lack of diffusion. ECB encrypts identical plaintext blocks into identical ciphertext blocks, it does not hide data patterns well.

Visual explanation of why ECB mode is not suitable to be used for encryption. [ECB vs CBC](https://pthree.org/2012/02/17/ecb-vs-cbc-encryption/)

Zoom was using a [AES-128 key in ECB mode](https://www.zdnet.com/article/zoom-concedes-custom-encryption-is-sub-standard-as-citizen-lab-pokes-holes-in-it/)

![ECB encryption](http://upload.wikimedia.org/wikipedia/commons/c/c4/Ecb_encryption.png)
Image by wikipedia.

![CBC encryption](https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/CBC_encryption.svg/1202px-CBC_encryption.svg.png)
Image by wikipedia.

## Exercises

Try encrypt and decrypt AES. Code from https://hackthestuff.com/article/how-to-encrypt-and-decrypt-data-in-node-js-using-crypto

```js
const crypto = require("crypto");

//aes-256-cbc algo requires a 256 bit (32 bytes) length key and an 128 bit length iv (aka. initialisation vector).

// Nodejs encryption examples with CBC
const crypto = require("crypto");
const algorithm = "aes-256-cbc";
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

function encrypt(text) {
  let cipher = crypto.createCipheriv(algorithm, Buffer.from(key), iv);
  let encrypted = cipher.update(text);
  encrypted = Buffer.concat([encrypted, cipher.final()]);
  return { iv: iv.toString("hex"), encryptedData: encrypted.toString("hex") };
}

function decrypt(text) {
  let iv = Buffer.from(text.iv, "hex");
  let encryptedText = Buffer.from(text.encryptedData, "hex");
  let decipher = crypto.createDecipheriv("aes-256-cbc", Buffer.from(key), iv);
  let decrypted = decipher.update(encryptedText);
  decrypted = Buffer.concat([decrypted, decipher.final()]);
  return decrypted.toString();
}
```

## Problems with purely using symmetric encryption

The main problem with symmetric encryption is, how can the sender and receiver agree on a key securely on the Internet? If we send the key through email, it might be intercepted by a third-party. We cannot send a key over a public channel.

We shall try to solve this problem with asymmetric encrypion.

## Asymmetric Encryption / Decryption

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AQDCe585Lnc?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Asymmetric encryption/decryption, where there are two keys used for encryption/decryption, one of them is called private key, the other is called public key. There is a unique relationship between the two keys:

- A message encrypted by a public key can be decrypted by the corresponding private key

- A message encrypted by a private key can be decrypted by the corresponding public key

The owner of the private key should keep it as a secret and never share it with other people. In contrast, the public key can be shared with other people. Then the owner of the private key can encrypt a message with his/her private key and anyone holding the public key can decrypt the message.

When decryption is done successfully, people can also confirm that the message is created by the person holding the private key. This is the foundation of digital signature.

One of the well known asymmetric encryption/decryption algorithms is called RSA.

The problem with asymmetric is that it is not as fast as using the symmetric algorithm to encrypt and decrypt.

### Data Origin Authenticity with Signature

If Amy wants to prove that Bob is the one who sent the message, Bob needs to send the message with a cryptographic signature. To create the signature, a one-way hash of the data is created then signed. The private key of the sender is then used to encrypt the hash. This encrypted hash, and other information like the hashing function used, is the digital signature.

### Public key cryptography with digital signatures

The digital signature created by Bob along with the message is then encrypted with the public key of the recipient (Amy). Amy then can eventually decrypt the digital signature + message with her private key.

### RSA

- RSA stands for Rivest, Shamir, Adleman
- widely used, one of the first algorithms in 1977
- can be used for both public key cryptography and digital signature
- security is based on an assumption: factoring a very large integer is hard

RSA multiplies two large prime numbers for its algorithm.

How does one generate large prime numbers? Pick a large random number (a very large random number) and test for primeness. If that number fails the prime test, then add 1 and test again.

We take these two random prime numbers (p and q) and then multiply them together to create a modulus (N). The value of N is then part of the public and the private key. For RSA-2048 we use two 1024-bit prime numbers, and RSA-4096 uses two 2048-bit prime numbers.

![RSA algorithm](https://i.ytimg.com/vi/-jSX9fNJiN8/maxresdefault.jpg)

Multiplication is easy, but the difficult part is factoring the product of the multiplying those two primes, to get back the original two primes.

Factoring out the two prime numbers that makeup the N will take a very long time. However, generating and checking those two primes is relatively easy.

RSA-2048 is used commonly within digital certificates and for TLS.

### Digital certificates

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/heacxYUnFHA?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Exercises

### Sign and verify with RSA using Node RSA

1. Download Node RSA.
   https://www.npmjs.com/package/node-rsa

2. Using the instructions on the github, use Node RSA to generate a public / private key pair.

3. encrypt using public key and decrypt using private key for the following paragraph:

```
SINGAPORE: The number of COVID-19 cases in Singapore crossed the 13,000 mark on Sunday (Apr 26), after another 931 cases were confirmed as of noon.

The vast majority of the latest cases are work permit holders residing in foreign worker dormitories, the Ministry of Health (MOH) said in its preliminary release of figures.

Fifteen cases are Singaporeans or permanent residents, added MOH.

The new cases bring the national total to 13,624.
```

4. Export your private and public keys and share your public keys with the class.
5. Try to modify your code to read a private key in a key.pem file and a public key in key.pub file, rather than generating a pair each time.

## Problems with purely using asymmetric cryptography

- is very slow
- data size is very limited

## Asymmetric cryptography with symmetric key

Due to the problems of each type of cryptography, symmetric key (session key) is often used with the asymmetric public / private key cryptography.

Let's say Amy wants to establish a secure connection with Bob. She wants to send over the session key. However, she needs a secure way to send over the key. She uses the session key to encrypt the plain text. Then she uses Bob's public key to encrypt the session key.

Bob can now decrypt the session key using his private key. Now he has the session key, and he can decrypt the ciphertext to obtain the plain text.

Since asymmetric cryptography is used to encrypt the session key and not the entire plain text, it would take less time.

Protocols like HTTPS are very fast and can be used to encrypt very large streams of data because they only use RSA to exchange an symmetric AES key (a shared secret) and then continues the session with that shared secret.

### HTTPS over SSL/TLS Certificates

Hypertext Transfer Protocol Secure (HTTPS) is an extension of the HTTP for secure communication over a computer network, and is widely used on the Internet. In HTTPS, the communication protocol is encrypted using Transport Layer Security (TLS), or formerly, its predecessor, Secure Sockets Layer (SSL). The protocol is therefore also often referred to as HTTP over TLS, or HTTP over SSL.

When you visit a website, you may notice a green lock icon on the URL address bar. That indicates the website has a SSL/TLS certificate to prove its identity.

Each certificate has a private key and public key. The web server hosting the website holds the private key, and the browsers download the certificates which contains the public key.

Then the browser can send a challenge (**nonce**) to the web server. The challenge is basically some random number encrypted with the public key of the certificate. If the web server holds the corresponding private key, it should be able to decrypt the challenge and return the correct random number to the browser.

Upon receiving the correct random number sent in the challenge, the browser can confirm the web server holds the correct private key, hence its identify (as declared in the certificate) can be trusted.

This random number is then used as a secret to encrypt/decrypt the HTTP requests/responses between the browser and the server.

SSL and TLS simply refer to this handshake that takes place between a client and a server. The handshake doesn’t actually do any encryption of the actual data itself, it just agrees on a shared secret and type of encryption that is going to be used.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/4nGrOpo0Cuc?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

![TLS 1.2 Handshake](https://commons.wikimedia.org/wiki/File:Full_TLS_1.2_Handshake.svg)

Image from wikipedia

![TLS](https://www.imperva.com/wp-content/uploads/sites/13/2020/03/diagram-52@3x.png)
