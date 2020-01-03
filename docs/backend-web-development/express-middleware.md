# Express.js middleware

Middleware is all the functions that are invoked by the Express.js routing layer before your final handler function is called. Thus they sit in the middle between a raw request and the final intended route. We often refer to these functions as the middleware stack since they are always invoked in the order they are added.
The following diagram shows how the middlewares work together:

## Why do we need middlewares?

Middlewares allow us to put logic with cross-cutting concerns (i.e. the logic that can be shared by multiple route handlers) into a common place and avoid duplicating the same logic in each route handler.
Examples of cross cutting concerns include:

- parsing requests
- logging request details
- authenticating the client sending the request

Middleware also give you a chance to do some pre-processing on a request before the handler function handle it. For instance, after a user has logged in, you could fetch their user details from a database, and then store those details in res.user.

## Implementation

```js
function(req, res, next) {
  // do something useful here

  // don't forget to call next
  next();
}
```

We need to pay special attention to the next argument. This is a callback function for each middleware
It's critical that you don't forget calling the next function after a middleware finishes its task. Otherwise, the Express Framework will keep waiting for the call to next(), and the API client side would not receive any response!
