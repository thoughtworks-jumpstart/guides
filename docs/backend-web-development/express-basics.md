# Express.js basics

Express.js framework is built on top of Node.js for simpler web server development. Express.js comes with parsing, security, user sessions, error handling etc, that are not handled by Node.js

## Installing Express.js

```
npm install express
```

## Features of Express.js

- Routing helps to split your app into smaller functions that are called when user visits a resource (like a specific URL)
- Routers can further break large apps into smaller subapplications
- Middleware functions help to process a request before it is passed on to the final handler functions

## Your first Express.js app

```js
const express = require("express");
const app = express();
const PORT = 3000;
```

You created your first Express.js app!
When express() is called in app.js, an app object is returned. Think of an app object as an Express application.

## Starting the server

```js
const server = app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}...`);
});
```

## Routing

Routing is to add a handler function (a callback function) that is called when a user visit a particular route, for example the homepage `/` or `/books`. When you listen for connections on a route in Express, the handler function will be invoked when a request comes in.

The request object and response object are passed to the handler function when the handler function is called.

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

Routing is described in detail on another page.

## Request object

The Request object (req) passed to a callback function holds all the HTTP request information that was sent to your server like query strings.

It starts off as an instance of _http.IncomingMessage_, a core Node object. Express then extends the object to add further functionality.

These are the commonly used properties of the request object:

- `req.params`
  Added by Express. An array containing named route parameters. This will be further explained below.

- `req.query`
  Added by Express. An object containing querystring parameters as name/value pairs. This will also be further explained below.

- `req.url`
  The request URL string.

- `req.method`
  The HTTP method (GET, POST, PUT, DELETE).

These could be used for debugging.

```js
console.log("Method: " + req.method);
console.log("Path: " + req.url);
```

You are unable to access the body of a request with `req.body` if the body is not parsed yet. This will be further explained in the [parsing request body](express-parsing-request-body) page.

## Response object

The Response object (res) passed to a callback function holds information that your server will respond with when the connection is ended with the client.

It starts off as an instance of _http.ServerResponse_, a core Node object. The res object is an enhanced version of the response object found in Node.js.

These are the commonly used properties or methods of the response object:

- `res.send()`
  Added by Express. Use it to send a response back to the client.

- `res.json()`
  Added by Express. Use it to send a JSON response back to the client.

- `res.status(statusCode)`
  Added by Express. Use it to set the status code of the response.

- `res.sendStatus(statusCode)`
  Added by Express. Shortcut for the above and sending the string representation of the status code as the response body.

- `res.end()`
  Send an empty response back to the client without any body.

## Middleware

Middleware functions are run by Express.js on the request before passing it on to the final handler function.

The request object, response object and the `next` function are passed to each middleware function. To move on to the `next` function to be run in the stack (could be a middleware function or handler function), the `next` function needs to be called.

```js
app.use(function(req, res, next) {
  console.log("Method: " + req.method);
  console.log("Path: " + req.url);
  next();
}
```

Middleware is described in detail on another page.

## Routers

From Express.js [documentation for Router](https://expressjs.com/en/guide/routing.html#express-router),

> Use the express.Router class to create modular, mountable route handlers. A Router instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.

In other words, routers are just like the root level app you created using `express()` so much so that they are mini-apps.

You can mount them to an app. For example, a router can be mounted to the `/empty` route of an app.

```js
const router = express.Router();

router.get("/", (res, req) => res.end());
router.post("/", (res, req) => res.end());

app.use("/empty", router);
```

## Layer stack

How does all these work together?

When we call `app.use` or `app.get` etc, we are adding a layer into the app or router.
Every app or router will have a layer stack - a stack of layers where every _Layer_ has its own _path_.

This layer can be a

- middleware
- route
- error handler
- another router

See this [Medium article](https://medium.com/@viral_shah/express-middlewares-demystified-f0c2c37ea6a1) if you are curious to understand how this layer stack works.

## Testing the server

### cURL

Client URL
cURL is by default already installed in Mac OS. For Windows, you can download cURL separately.
You can send different HTTP requests.

For example, let's send a POST request to https://localhost:3000/users with a JSON message body.

```sh
curl --request POST \
  --url https://localhost:3000/users \
  --header 'content-type: application/json' \
  --data '{
    "username": "babel",
}'
```

Alternatives to the flags:

```sh
curl -X POST \
  http://localhost:3000/users \
  -H 'content-type: application/json' \
  -d '{
    "username": "babel"
}'
```

### API testing tool

Instead of cURL, you can use an API testing tool like Postman or Insomnia. You can even save your requests and organise them into folders. Using those tools, compose a request to "http://localhost:port/" where `port` is the port that your server is listening to.
