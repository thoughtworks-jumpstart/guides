# Security Cryptography basics

## What is cryptography?

As part of the authentication process, we will use cryptography techniques to prove the identity of the user.

Cryptography is the practice and study of techniques for secure communication in the presence of third parties. In layman terms, it's the techniques to protect some secrets when they are shared between people.

## Why use cryptography?

With cryptography techniques, we can:

- hide a secret so that only the intended recipient can interpret it.
- provide a way to verify the original message was not altered by malicious people when the recipient gets it.
- provide a way to prove the message is indeed created by the sender (and not another malicious person).

## Kerckhoffs's principle

A cryptographic system should be secure even if everything about the system, except the key, is public knowledge.

In other words, we should not use obscurity (not knowing how the algorithm works) as a form of security. The security of the encrypted message only relies on protecting the secret encryption key.
