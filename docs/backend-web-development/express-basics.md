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

### Starting the server

```js
const server = app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}...`);
});
```

### Routing

Routing is to add a handler function that is called when a user visit a particular route, for example the homepage `/` or `/books`. When you listen for connections on a route in Express, the handler function (callback function) will be invoked when a request comes in.

The request object and response object are passed to the handler function when the handler function is called.

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

### Middleware

Middleware functions are run by Express.js on the request before passing it on to the final handler function.

The request object, response object and the `next` function are passed to each middleware function. To move on to the `next` function to be run in the stack (could be a middleware function or handler function), the `next` function needs to be called.

```js
app.use(function(req, res, next) {
  console.log("Method: " + req.method);
  console.log("Path: " + req.url);
  next();
}
```

## Testing the server with an API testing tool

To see it working, you need to use an API testing tool like Postman or Insomnia.
Using those tools, compose a request to "http://localhost:port/" where `port` is the port that your server is listening to.
