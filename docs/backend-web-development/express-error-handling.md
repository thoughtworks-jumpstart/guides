# Express.js error handling

There are many possible errors that could occur when our code runs. For example, when searching for a user in a database, the database server could be down, or there could be network issues.

## Synchronous error handling

Refer to the script: [Express.js playground: error_handler_example_1](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/error_handler_example_1.js).

If you want to handle a synchronous error, you can throw the error in an Express.js handler function. If the code has no calls to any asyncthronous API (like `fs.readFile`), you can throw the error like this.

```js
app.get("/error", (req, res) => {
  throw new Error(" 😱! Error! Error!");
});
```

When an error is thrown, other handler functions (middleware or routes) are all skipped and the next error handler is called. If you did not write a custom error handler, Express.js will catch the error and handle it for you with a default error handler.

Express’s default error handler will:

- Set the HTTP Status to 500
- Sends a text response of the error and the error stack
- Logs the text response in the console

## Writing a custom error handler

They have a signature of:

```js
function(err, req, res, next)
```

While defining them, it is absolutely necessary to have all four params in the signature so that Express.js can differentiate them from other middleware. Even if you don’t need to use the next object, you must specify it to maintain the signature.

This is a custom error handler for _500 internal server error_. HTML is sent back as a response to the client. Being just like a middleware, they are registered to an _app_ instance using `app.use`.

Within an error handler, you typically need to do one of two things:

- call `res.send()` to generate a response, or
- call `next(err)` to pass the execution to the next error handler

```js
app.use((err, req, res, next) => {
  res.status(500);
  res.send(
    `Error: ${err} </br>
    Error stack: ${err.stack}`
  );
});
```

Since Express.js middleware run in the order that they are called, define error handler functions **last** so that they will be correctly called when there is an error.

Refer to the script: [Express.js playground: error_handler_example_2](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/error_handler_example_2.js).

We can define properties to the error object such as a `code` so that we can return the correct status code in the response header.

```js
app.get("/", (req, res) => {
  // synchronous error
  error = new Error(" 😱! Error! Error!");
  error.statusCode = 200;
  throw error;
});
```

Return the correct status code in the response header if `err.statusCode` is defined. Else we will default to the error code `500`.

```js
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500);
  res.send(
    `Error: ${err} </br>
    Error Status Code: ${err.statusCode} <br>
    Error stack: ${err.stack}`
  );
});
```

## Asynchronous error handling

Refer to the script: [Express.js playground: error_handler_example_3](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/error_handler_example_3.js).

If you call an asynchronous API in a route handler and you would like to handle errors returned/thrown by those asynchronous operations, you just need to call `next(err)` when some error happens. That is, you call the `next` callback (which is an argument of every middleware and route handler) with an instance of _Error_.

```js
app.get("/", (req, res, next) => {
  // assume some asynchronous error happens because of an network issue
  const err = new Error("Unexpected network error");
  next(err);
});
```

Let's add a route handler that will not be called because it will be skipped when the error is thrown.

```js
app.get("/", (req, res, next) => {
  console.log("You should not see this line in the console 😡");
});
```

Add a custom error handler that will catch the error and then pass it on to the next error handler.

```js
app.use((err, req, res, next) => {
  if (err.message === "Unexpected network error") {
    console.log("I don't know how to handle network error. Pass it on.");
    next(err);
  } else {
    console.log("Unknown error. Pass it on.");
    next(err);
  }
});
```

Add a final custom error handler that will return a response to the client.

```js
app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  res.status(err.statusCode).send(err.message);
});
```

An example of error handling with an asynchronous API is as follows:

```js
const fs = require("fs");

app.get("/", function (req, res, next) {
  fs.readFile("/file-does-not-exist", function (err, data) {
    if (err) {
      next(err);
    } else {
      res.send(data);
    }
  });
});
```

### Why do we need error handling middleware?

To see the value of having error handling middleware, imagine having the following code instead.

```js
const fs = require("fs");

app.get("/", function (req, res, next) {
  fs.readFile("/file-does-not-exist", function (err, data) {
    if (err) {
      res.status(500).json({ error: error.toString() });
    } else {
      res.send(data);
    }
  });
});
```

This is acceptable if you have one or two endpoints dealing with asynchronous functions, but chances are that you are going to have to maintain many more endpoints. You will find that this method is quickly not scalable. If it is suddenly decided that the _503 response code_ is more appropriate than _500 internal server error_, you're going to have to change that for every single endpoint.

How about responding to different types of error conditions? Error handlers can easily do that.

```js
const { AssertionError } = require("assert");
const { MongoError } = require("mongodb");

app.use(handleAssertionError(err, req, res, next) => {
  if (err instanceof AssertionError) {
    return res.status(400).json({
      type: "AssertionError",
      message: err.message,
    });
  }
  next(err);
});

app.use(handleDatabaseError(err, req, res, next) => {
  if (err instanceof MongoError) {
    return res.status(503).json({
      type: "MongoError",
      message: err.message,
    });
  }
  next(err);
});
```

Example is from http://thecodebarbarian.com/80-20-guide-to-express-error-handling.

You can see now that error handling middleware allows you to consolidate error handling logic.

## Best practices

As a best practice, we should always declare a default error handler for all requests, to ensure we handle all unforeseeable error scenarios. You will define error-handling middleware last, after other app.use() and routes calls.

## Layer stack

How does middleware, route, error handler, routers work together?

When we call `app.use` or `app.get` etc, we are adding a layer into the app or router.
Every app or router will have a layer stack - a stack of layers where every _Layer_ has its own _path_.

This layer can be a

- middleware
- route
- error handler
- another router

See this [Medium article](https://medium.com/@viral_shah/express-middlewares-demystified-f0c2c37ea6a1) if you are curious to understand how this layer stack works.
