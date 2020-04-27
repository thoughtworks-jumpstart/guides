# JSON Web Token

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

## Why do we need a JSON web token?

Short answer:
When a user logins, we need to give the user something that they can use to prove that they are the same user for subsequent requests. This is so that the user does not have to login again for the rest of the session.

Long answer:
In authentication, when the user successfully logs in using their credentials, a JSON Web Token can be returned to the client side (e.g. the mobile app or browser). The client side saves the token (e.g in memory, or cookies).

Whenever the user wants to access a protected route or resource, the client side submit the previously saved token to server again. The JWT token in the requests help to identify who the user is, and the server side application can decide if the request on the resource should be allowed (this is called authorization).

## Benefits of using JSON web token

We could have used cookies and session id! Why JSON web token?

As JWTs are self-contained, all the necessary information is there, reducing the need to query the database.

To be specific, the user information is obtained from the JWT token instead of looking up in database on the server side. So it helps to make the authorization process faster and stateless.

This is beneficial, because when there are lots of users of a website, we can add more instance of web servers in the backend, and if the requests are stateless, that means a request an be served by any instance of web server. This make it very easy to scale the capacity of the website by adding more servers.

## How does a JSON web token look like?

Use https://github.com/thoughtworks-jumpstart/how-jwt-works/blob/master/how-jwt-works.js to demo the construction of a JWT token.

In its compact form (JWS Compact Serialization format), JSON Web Tokens consist of three parts separated by dots (.), which are:

- Header
- Payload
- Signature

The **header object** contains information about the JWT itself: the type of token, the signature or encryption algorithm used, the key id, etc.

The **payload object** contains all the relevant information carried by the token. There are several standard claims, like `sub` (subject) or `iat` (issued at), but any custom claims can be included as part of the payload.

The **cryptographic** signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.

Note that you need a secret to sign and verify JWT tokens. This secret should be stored securely on the server side and only accessible to the application using this secret.

Therefore, a JWT typically looks like the following.
xxxxx.yyyyy.zzzzz

Go to https://jwt.io/ to see an example.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Decoding it using base64url, we can see the plaintext of the three parts.

## Why use base64url encoding?

We have been using this term a few times above. What exactly is this thing? Why do we need it?
This is one algorithm that converts binary data (or text data) into a format that can be carried in HTTP request URL or headers.

According to the specification of HTTP protocol, there are certain characters (such as + and =) that are not allowed to apppar as part of URL or request/response header. Both characters have a special meaning in the URI address: “+” is interpreted as space, while “=” is used to send data via query string as “key=value” pair.

On the other hand, people often include JWT tokens as part of their HTTP requests (as we will show below), so it's important to make sure JWT token value do not contain those forbidden characters.

People created this base64url encoding scheme for this purpose.

## Payload is not secure!

Base64url is an **encoding** scheme, not an **encryption** scheme. It is easily reversed and the payload is not secret anymore. Do not store sensitive information inside JWT payload.

## Why have a signature? What is RS256 or HS256?

Adapted from https://community.auth0.com/t/jwt-signing-algorithms-rs256-vs-hs256/7720

> Both choices refer to what algorithm the identity provider uses to sign the JWT. Signing is a cryptographic operation that generates a “signature” (part of the JWT) that the recipient of the token can validate to ensure that the token has not been tampered with.
>
> - RS256 (RSA Signature with SHA-256) is an asymmetric algorithm, and it uses a public/private key pair: the identity provider has a private (secret) key used to generate the signature, and the consumer of the JWT gets a public key to validate the signature. Since the public key, as opposed to the private key, doesn’t need to be kept secured, most identity providers make it easily available for consumers to obtain and use (usually through a metadata URL).
> - HS256 (HMAC with SHA-256), on the other hand, is a symmetric algorithm, with only one (secret) key that is shared between the two parties. Since the same key is used both to generate the signature and to validate it, care must be taken to ensure that the key is not compromised.
>   If you will be developing the application consuming the JWTs, you can safely use HS256, because you will have control on who uses the secret keys. If, on the other hand, you don’t have control over the client, or you have no way of securing a secret key, RS256 will be a better fit, since the consumer only needs to know the public (shared) key.

### A good secret for HS256

Text adapted from https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/

[JSON Web Algorithms](https://tools.ietf.org/html/rfc7518) defines the minimum key length to be equal to the size in bits of the hash function used along with the HMAC algorithm:

> "A key of the same size as the hash output (for instance, 256 bits for "HS256") or larger MUST be used with this algorithm." - JSON Web Algorithms (RFC 7518), 3.2 HMAC with SHA-2 Functions

If a short key like `secret` (which is ironically very not secret!) is used, we can use [brute force attacks to guess the key](https://auth0.com/blog/brute-forcing-hs256-is-possible-the-importance-of-using-strong-keys-to-sign-jwts/).

Alternatively, use RS256.

## Using JWT for http request authentication and authorization

In authentication, when the user successfully logs in using their credentials, a JSON Web Token can be returned to the client side (e.g. the mobile app or browser). The client side saves the token (e.g in memory, or cookies) and submit it to server again in subsequent requests.

Whenever the user wants to access a protected route or resource, the JWT token in the requests help to identify who the user is, and the server side application can decide if the request on the resource should be allowed (this is called authorization).

## How to submit JWT tokens to web servers

There are two ways for the client side to submit JWT tokens to the server side:

- Using HTTP request header. There is one HTTP header called Authorization. The content of the header should look like the following:

```
Authorization: Bearer <token>
```

- Using cookies. If JWT token is saved in cookies, the cookie is sent automatically together with the request when ever the user visit the same server that issues the cookie.

### Using Authorization HTTP request header

For the first way of using the request header, we have this demo project at https://github.com/thoughtworks-jumpstart/express-jwt-lab/blob/master/app.js which shows you how to issue JWT token from server side and verify it in subsequent requests.

In the app.js file:

- The handler for /signin generates a JWT token and send it back in response body.
- The handler for /secret verifies the JWT token and only display secret text if the token can be verified

A library called jsonwebtoken is used to sign and verify JWT tokens.

Clone the project, start the server, and test the API using REST API clients like Postman.

### Using cookies and same origin policy

For us to read cookies, we need `cookie-parser`.

```
npm install cookie-parser
```

In app.js, we use this middleware.

```js
// app.js
const cookieParser = require("cookie-parser");

app.use(cookieParser());
```

### Install json web token and bcryptjs

Install the star of the show, the token we will be creating and reading.

```
npm install jsonwebtoken
```

We shall use bcryptjs for hashing our passwords. (which uses bcrypt)

```
npm install bcryptjs
```

### Add a schema with user and password

We shall create an `Trainer` model.

```js
//trainer.model.js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const trainerSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true,
    minlength: 3,
    lowercase: true,
  },
  password: {
    type: String,
    required: true,
    minlength: 8,
  },
  firstName: String,
  lastName: String,
  salutation: {
    type: String,
    enum: ["Dr", "Mr", "Mrs", "Ms", "Miss", "Mdm"],
  },
});

trainerSchema.virtual("fullName").get(function () {
  return `${this.salutation} ${this.firstName} ${this.lastName}`;
});

trainerSchema.virtual("reverseName").get(function () {
  return `${this.lastName}, ${this.firstName}`;
});

trainerSchema.pre("save", async function () {
  if (this.isModified("password")) {
    const rounds = 10;
    this.password = await bcrypt.hash(this.password, rounds);
  }
});

const Trainer = mongoose.model("Trainer", trainerSchema);

module.exports = Trainer;
```

### New trainer route for creating a new Trainer

```js
router.post("/", async (req, res, next) => {
  try {
    const trainer = new Trainer(req.body);
    const newTrainer = await trainer.save();
    res.send(newTrainer);
  } catch (err) {
    next(err);
  }
});
```

```json
{
  "username": "ash3",
  "password": "iWannaB3DVeryBest"
}
```

I can create trainers now! Note that the hash is different even for same passwords. This is thanks to the salting of bcrypt.

Try using Postman now!

### Protect a route trying to find trainers by username

```js
const protectRoute = (req, res, next) => {
  try {
    if (!req.cookies.token) {
      throw new Error("You are not authorized");
    }
    req.user = jwt.verify(req.cookies.token, process.env.JWT_SECRET_KEY);
    next();
  } catch (err) {
    err.statusCode = 401;
    next(err);
  }
};

router.get("/:username", protectRoute, async (req, res, next) => {
  try {
    const username = req.params.username;
    const regex = new RegExp(username, "gi");
    const trainers = await Trainer.find({ username: regex });
    res.send(trainers);
  } catch (err) {
    next(err);
  }
});
```

`req.cookies` is populated by the `cookie-parser` middleware.

### Generating a JWT token and finding the secret

```js
const getJWTSecret = () => {
  const secret = process.env.JWT_SECRET_KEY;
  if (!secret) {
    throw new Error("Missing secrets to sign JWT token");
  }
  return secret;
};

const createJWTToken = (username) => {
  const today = new Date();
  const exp = new Date(today);

  const secret = getJWTSecret();
  exp.setDate(today.getDate() + 60); // adding days

  const payload = { name: username, exp: parseInt(exp.getTime() / 1000) };
  const token = jwt.sign(payload, secret);
  return token;
};
```

You can refactor this to put `getJWTSecret` in a `config` folder.

- Create the config folder under your project root directory.
- Then create a `jwt.js` file inside the config folder, with the `getJWTSecret` function.

For your tests and your code to be able to find the `JWT_SECRET_KEY`, you can load the environment variables using `dotenv` in **app.js**.

```js
require("dotenv").config();
```

#### More on the exp field in the JWT payload

In the example above, the expiration date of the JWT token is set to 60 days later after it's generated. Then the expiration time is saved into the exp field of the JWT token.

What is this exp field? Why must I use this term?

It represents **Token Expiration,** and you can find [more details here](https://www.npmjs.com/package/jsonwebtoken#token-expiration-exp-claim). Once this field is set in a token, it's validated later on when we call the jwt.verify(token, secret). So a token that passes the expiration time will fail the verification.

#### Login and logout

Read documentation about `res.cookie` and `res.clearCookie` first.

```js
const bcrypt = require("bcryptjs");

router.post("/logout", (req, res) => {
  res.clearCookie("token").send("You are now logged out!");
});

router.post("/login", async (req, res, next) => {
  try {
    const { username, password } = req.body;
    const trainer = await Trainer.findOne({ username });
    const result = await bcrypt.compare(password, trainer.password);

    if (!result) {
      throw new Error("Login failed");
    }

    const token = createJWTToken(trainer.username);

    const oneDay = 24 * 60 * 60 * 1000;
    const oneWeek = oneDay * 7;
    const expiryDate = new Date(Date.now() + oneWeek);

    // Can expiry date on cookie be changed? How about JWT token?
    res.cookie("token", token, {
      expires: expiryDate,
      httpOnly: true, // client-side js cannot access cookie info
      secure: true, // use HTTPS
    });

    res.send("You are now logged in!");
  } catch (err) {
    if (err.message === "Login failed") {
      err.statusCode = 400;
    }
    next(err);
  }
});
```

How to generate a good `JWT_SECRET_KEY`? Because we use HS256 algoithm for the signature, we should have a 256 bits key of 32 characters.

You can generate a good random 256 bits key (crypographically strong pseudorandom) with

```sh
node -e "console.log(require('crypto').randomBytes(256 / 8).toString('hex'));"
```

You can also generate a base64 key with:

```sh
node -e "console.log(require('crypto').randomBytes(256 / 8).toString('base64'));"
```

If you choose to use a base64 key, read the key into a Buffer using `Buffer.from(key, "base64")` and use it with `jwt.sign` and `jwt.verify`.

Save it in `.env` file and do not commit it. Remember to add the `.env` file to `.gitignore`.

## Security Concerns of using JWT

Read https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/ for more specific attacks.

### Protecting the secrets used for signing/verification of JWT tokens

On the server side, there is one secret that is used to sign/verify JWT tokens (If HS256 is used). Make sure this token is stored safely (in a file or database) that only the application itself has access.

If this secret is leaked, you have to change the secret, but that means all the tokens signed with the previous secret cannot be verified anymore. The users being affected have to do authentication again to acquire new tokens.

### Where to store JWT token on client/browser side?

If you use JWT, you need to decide where to store the tokens at browser side. (Unless you use 0Auth, which is a different story.) You have two possible choices:

- Storing JWT token in memory only
- Storing JWT token in cookies

There are limitations and concerns to be addressed for each storage choice:

- If you store JWT tokens in browser memory only, that token is lost whenever the user refresh the browser page (and they need to authenticate again with the server side)

- If you store JWT tokens in cookies, you need to worry about security attacks like [CSRF](https://github.com/pillarjs/understanding-csrf). The good news is, this CSRF attack can be resolved by using some middleware like [csurf](https://github.com/expressjs/csurf) on Express application, or setting the `SameSite=strict` flag in the session cookie (however, [some browsers don't support this SameSite attribute](https://caniuse.com/#feat=same-site-cookie-attribute)).

There are two other choices for storing JWT tokens but they are **not recommended**:

- You can use browser's [local storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- You can use browser's [session storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)

There are even more security concerns of storing the JWT token in browser's local storage or session storage. For example, here are [some issues for local storage](https://www.owasp.org/index.php/HTML5_Security_Cheat_Sheet#Local_Storage). JavaScript has access to the browser storage. If a hacker injects some malicious script in to a website you are viewing (XSS attack), the script can load the tokens from your browser's storage and send them to the hacker. Then hacker can use your tokens!

You can find some [comparison here](https://auth0.com/docs/security/store-tokens) on different choices of storing JWT tokens. [This article](https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage) also provides more details on why storing JWT tokens in cookies is better than storing them in browsers' storage.

### Don't store sensitive data in JWT tokens

Another important thing to remember is by default the content in JWT token is not encrypted. So don't put sensitive data into JWT tokens or you have to use JSON Web Encryption (JWE).

## Use HTTPS to protect JWT token during transmission

When a JWT token is issued by the server side and sent back to the browser side, the token is transmitted on the Internet. If the connection is HTTP, any computer in the middle of the network transmission can get a copy of the token. Then some hacker can impersonate as other people by stealing other's JWT tokens.
This kind of attack is called "man-in-the-middle" attack.
To prevent this kind of attack, HTTPS should be used to encrypt the request/response data during transmission.

## Setting expiration time for each JWT token

It's a good practice to set an expiration time for each JWT token when they are issued, and the server side application should check for expiration when it gets a JWT token.
There is no good way to revoke/invalidate a JWT token once it's issued
There are still some cases when you need to revoke/invalidate a JWT token before it's expired. For example, some employee leaves a company, then he should lose access to internal website of the company immediately. If the website is protected using JWT tokens, then the tokens issued to this ex-employee need to be revoked/invalidated immediately.

Unfortunately, there is no easy way to do that for JWT tokens. There are some solutions but the cost of implementing those solutions makes JWT less appealing.

For example, you can maintain a blacklist of users on the server side. The server side application needs to check if the user (identified by the JWT token) is in this black list before granting the user to access protected resources.

This solution works, however, if you do this, there is not much benefit of using JWT compared with session cookies.

## Clear session information when user logout

If you use JWT token for session tracking, all the session information is in the JWT token. When a user logout, your client side application needs to remove this token from its memory.
If the JWT token is saved in a cookie, the logout route handler on the server side needs to delete the cookie that stores JWT token upon user logout. That can be done via the response.clearCookie() provided by Express framework.

## Exercises

Add `user` model to your songs API and protect the routes for PUT and DELETE. You will need to login to PUT and DELETE any of the songs.
