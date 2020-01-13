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
app.use(function(err, req, res, next) {
  res.status(500);
  res.send(
    `Error: ${err} </br>
    Error stack: ${err.stack}`
  );
});
```

Refer to the script: [Express.js playground: error_handler_example_2](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/error_handler_example_2.js).

We can define properties to the error object such as a `code` so that we can return the correct status code in the response header.

```js
app.get("/", (req, res) => {
  // synchronous error
  error = new Error(" 😱! Error! Error!");
  error.code = 200;
  throw error;
});
```

Return the correct status code in the response header if `err.code` is defined. Else we will default to the error code `500`.

```js
app.use(function(err, req, res, next) {
  res.status(err.code || 500);
  res.send(
    `Error: ${err} </br>
    Error Status Code: ${err.code} <br>
    Error stack: ${err.stack}`
  );
});
```

## Asynchronous error handling

Refer to the script: [Express.js playground: error_handler_example_3](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/error_handler_example_3.js).

If you call an asynchronous API in a route handler and you would like to handle errors returned/thrown by those asynchronous operations, you just need to call `next(err)` when some error happens. That is, you call the `next` callback (which is an argument of every middleware and route handler) with an instance of _Error_.

```js
app.get("/", function(req, res, next) {
  // assume some asynchronous error happens because of an network issue
  const err = new Error("Unexpected network error");
  next(err);
});
```

Let's add a route handler that will not be called because it will be skipped when the error is thrown.

```js
app.get("/", function(req, res, next) {
  console.log("You should not see this line in the console 😡");
});
```

Add a custom error handler that will catch the error and then pass it on to the next error handler.

```js
app.use(function(err, req, res, next) {
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
app.use(function(err, req, res, next) {
  res.status(500);
  res.send({ error: "internal server error" });
});
```

A realistic example of error handling with an asynchronous API is as follows:

```js
app.get("/", function(req, res, next) {
  fs.readFile("/file-does-not-exist", function(err, data) {
    if (err) {
      next(err);
    } else {
      res.send(data);
    }
  });
});
```

## Best practices

As a best practice, we should always declare a default error handler for all requests, to ensure we handle all unforeseeable error scenarios. You will define error-handling middleware last, after other app.use() and routes calls.
