# Security Authentication and Authorization

## Information security CIA triad

Why do we need to protect our information? What do we need to protect?

<img src="backend-web-development/_media/cia-triad.png" alt="cia triad" width="600"/>

(From: https://thorteaches.com/cissp-the-cia-triad-and-its-opposites/)

### Confidentiality

We need to keep information secret from others. No one else should be allowed to know the information except those that are authorized to.

### Integrity

We need to protect our information from being modified by unauthorized users. Otherwise, we will be unable to determine the integrity of our information.

### Availability

We need to have access to our information when we need it. We should be able to allow users that are authorized to access our information.

With authentication and authorization, we can protect our information.

## Authentication

Authentication is the process for the server side to identify who is the current user on the client/browser side. This process is triggered when the user login, or when the user tries to access a protected resource on the server side.

In the authentication process, typically a user would submit his user name and password to the server, and the server side needs to verify those credentials.

If the user's identify is verified, a HTTP cookie or token needs to be sent back to the client side so that the subsequent requests in the same session do not trigger authentication again.

## Session Management

After authentication is done, the subsequent requests in the authenticated session need to carry the cookie or token issued by the server side, so that those requests do not trigger authentication again.

There are quite a few challenges and choices with session management:

- Should the server side issue cookie or tokens upon successful authentication?
- Where should client side store the cookie/tokens safely?
- How can the cookie/token be included in the requests?
- How to prevent those cookie/token being stolen during transmission?
- How to invalidate/revoke a cookie/token when the user logout?

We will discuss more about this topic soon.

## Authorization

Authorization is the process for the server side to decide if the current user has access to the requested URL/resource. This can be done when there is a cookie or token associated with the request, which is issued by the server upon successful authentication.

Based on the user identity revealed from the cookie or token, and the privilege/permission assigned to the user, the server side can either grant or deny the current request accordingly.
