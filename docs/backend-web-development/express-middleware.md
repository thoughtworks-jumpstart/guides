# Express.js middleware

Middlewares are all the functions that are invoked by the Express.js routing layer on the request before the final handler function is called. They sit in the middle between a raw request and the final intended route. We often refer to these functions as the middleware stack since they are always invoked in the order they are added.

The following diagram shows how the middlewares work together in a _pipeline_ like water flowing in a pipe:

<img src="backend-web-development/_media/middleware.png" alt="how middleware work together" width="700"/>

(image source: https://manuel-rauber.com)

You could also imagine that it is a factory. For the request "assembly line", the request is passed from one middleware to another before it reaches the final handler function where the response is finally sent back to the client.

## Example

The request object, response object and the `next` function are passed to each middleware function. To move on to the `next` function to be run in the stack (could be a middleware function or handler function), the `next` function needs to be called.

```js
app.use(function(req, res, next) {
  console.log("Method: " + req.method);
  console.log("Path: " + req.url);
  next();
}
```

## Why do we need middlewares?

Middlewares allow us to put logic with cross-cutting concerns (i.e. the logic that can be shared by multiple route handlers) into a common place and avoid duplicating the same logic in each route handler.
Examples of cross cutting concerns include:

- parsing requests
- logging request details
- authenticating the client sending the request

Middleware also give you a chance to do some pre-processing on a request before the handler function handle it. For instance, after a user has logged in, you could fetch their user details from a database, and then store those details in res.user.

## Implementation

Middleware is simply a function that takes three arguments: a request object, a response object and a `next()` function.

```js
function(req, res, next) {
  // do something useful here

  // don't forget to call next
  next();
}
```

It could also look like this:

```js
const middlewareArrow = (req, res, next) => {
  console.log("I am a middleware arrow function!");
  next();
};
```

The `next` argument is a callback function for each middleware. Calling it will tell the Express framework that "Hey, my job is done. Now pass the request to the next middleware or handler function that should handle this request."

Remember to call the next function, otherwise Express.js will keep waiting for the call to next(), and the client would not receive any response.

Middleware can do all of the following though it is optional:

- Execute any code
- Make changes to the request and the response objects
- End the request-response cycle (by calling `res.send`, for example)
- Call the next middleware function in the stack

## Using a middleware function

### Application-level middleware

Refer to the script: [Express.js playground: middleware_example_1](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/middleware_example_1.js).

Add a middleware function that will be called for all routes. It has no mount path.

```js
app.use(function (req, res, next) {
  console.log("common middleware function was called!");
  next();
});
```

Add a middleware function that will only be called if a request goes to the `/users` route.

```js
app.use("/users", function (req, res, next) {
  console.log("middleware function for /users was called!");
  next();
});
```

Add a middleware function that will only be called if a POST request goes to the `/users` route.

```js
app.post("/users", function (req, res, next) {
  console.log("second middleware function for /users was called!");
  next();
});
```

Add a middleware function that will only be called if a GET request goes to the `/users` route.

```js
app.get("/users", function (req, res, next) {
  console.log("third middleware function for /users was called!");
  next();
});
```

Refer to the script: [Express.js playground: middleware_example_2](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/middleware_example_2.js).

Add a middleware function that is a named arrow function.

```js
const arrowFunction = (req, res, next) => {
  console.log("I am an arrow function!");
  next();
};

app.use(arrowFunction);
```

Add a middleware function that is returned by another function and checks querystrings:

```js
const checkColor = () => {
  return (req, res, next) => {
    if (req.query.color) {
      res.send("I have blue!");
    } else {
      next();
    }
  };
};

app.use(checkColor());
```

Send a GET request to http://localhost:3000/users?color=blue or http://localhost:3000/books?color=blue. Is there a difference? How about http://localhost:3000/users?color=red ?

### The order of middleware loading

As mentioned earlier, middleware is executed in a _pipeline_, the order that the middleware functions are added matters.

<img src="backend-web-development/_media/middleware-order.png" alt="order of middleware" width="700"/>

(image source: https://developer.okta.com/blog/2018/09/13/build-and-understand-express-middleware-through-examples)

If the third middleware is added before the first middleware, the third middleware will be called first.

### Add a middleware that checks for JSON POST requests

Refer to the script: [Express.js playground: middleware_example_3](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/middleware_example_3.js).

We can create a middleware function that checks the request header `Content-Type` to see if the request is sending JSON.

```js
const requireJsonContent = (req, res, next) => {
  if (req.headers["content-type"] !== "application/json") {
    res.status(400).send("Server wants application/json!");
  } else {
    next();
  }
};
```

Add the above middleware function to the POST route

```js
app.post("/", requireJsonContent, (req, res, next) => {
  res.send("Thanks for the JSON!");
});
```

Run the server. Open Postman and create a POST request to http://localhost:3000. Don’t set any headers nor send anything in the request body. What is the response of the server?

<img src="backend-web-development/_media/middleware-POST-without-json-response.png" alt="the response from a POST without json" width="800"/>

You could add the Content-Type header with a value of application/json and make a POST request again. Alternatively, you could add some JSON content to the body of the request. Postman will automatically set the header.

<img src="backend-web-development/_media/middleware-POST-with-json-request.png" alt="the response from a POST without json" width="800"/>

Now the server will thank you for your json request.

<img src="backend-web-development/_media/middleware-POST-with-json-response.png" alt="the response from a POST without json" width="800"/>

### Router-level middleware (Optional)

You are also able to add middleware on the router level instead of the application level.

Add a middleware function that is executed for every request to the router. It has no mount path.

```js
var router = express.Router();

router.use((req, res, next) => {
  console.log("I am middleware for the router");
  next();
});
```
