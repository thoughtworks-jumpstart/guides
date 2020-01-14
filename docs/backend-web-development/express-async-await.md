# Express.js async await

## Why async await?

We often need to implement some asynchronous operations in an API. As we learnt before in the JavaScript lessons, the common choices for implementing asynchronous logic are:

- callbacks
- promises
- async/await

We are going to learn how to use async/await feature in Express.js to make async code easier to manage.

Why is async/await better than callbacks and promises? It is so that we don't suffer from callback hell and will also be able to write asynchronous code in an synchronous way. See [tylermcginnis's callbacks, promises and async/await tutorial for more information](https://tylermcginnis.com/async-javascript-from-callbacks-to-promises-to-async-await/).

Note: async/await feature is supported by Node since version 7.6.0. If you are using an older version of Node, you cannot use it without transpilers like Babel.

## Asynchronous route handlers

We have been writing route handlers in an _synchronous_ way. This should be familiar to you.

```js
app.get("/", (req, res, next) => {
  res.send("Welcome to my homepage");
});
```

This is not ideal as synchronous code is blocking. eg: when if we connect to a DB or other I/O task like reading a file from disk, synchronous code blocks other requests until it has completed its execution of the task. This will slow down the performance of your server.

Our route handlers are often _asynchronous_ so that while there is an I/O operation ongoing, Express can still handle other requests.

Refer to the script: [Express.js playground: async_await_example](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/async_await_example.js).

The following is an example of an asynchronous route handler. Note that it makes use of the async/await feature in Express.js.

```js
// this function simulates one asynchronous operation,
// e.g. loading user profile from database after 10ms
const loadUserProfileFromDB = userName => {
  return new Promise(resolve => {
    setTimeout(() => resolve({ name: userName, gender: "M" }), 10);
  });
};

// the async function using async/await
const getUserProfile = async (req, res, next) => {
  const userName = req.params.userName;
  const userProfile = await loadUserProfileFromDB(userName);
  res.send(userProfile);
};

// register the asynchronous handler function to a path
app.get("/users/:userName", getUserProfile);
```

Try going to http://localhost:3000/users/babel. The correct user profile should be returned by our fake database.

## Dealing with errors

What if there is a network error or some other error thrown by the asynchronous operation?

Imagine we would like to load a blog entry from our database. However, there is a network connection error!

```js
// this function simulates one asynchronous operation that generate errors
const loadBlogPostFromDB = postId => {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error("Network Connection Error")), 10);
  });
};
```

The previous way of using `await` for loading the blog entry from the database will look like this:

```js
const getBlogPost = async (req, res, next) => {
  const postId = req.params.postId;
  // note: this line below would throw error
  const post = await loadBlogPostFromDB(postId);
  res.send(post);
};
```

The above will result in a `UnhandledPromiseRejectionWarning`.

```sh
(node:25429) UnhandledPromiseRejectionWarning: Error: Network Connection Error
    at Timeout.setTimeout [as _onTimeout] (/Users/.../express-playground/async_await_example.js:36:29)
    at ontimeout (timers.js:436:11)
    at tryOnTimeout (timers.js:300:5)
    at listOnTimeout (timers.js:263:5)
    at Timer.processTimers (timers.js:223:10)
(node:25429) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
```

We will need to put it into a _try/catch block_ to catch the error and pass it to error handlers.

Notice that we call `next(err)` instead of immediately sending an error response back to th client.
As in the [error handling examples](express-error-handling.md) as shown previously, we should make use of error handling middleware so that our error handling code is easier to manage.

```js
const getBlogPost = async (req, res, next) => {
  const postId = req.params.postId;
  try {
    const post = await loadBlogPostFromDB(postId);
    res.send(post);
  } catch (err) {
    next(err);
  }
};
```

We have to wrap every async function into it’s own try/catch block, handle the error and pass the error to the `next` callback so that we can trigger the error handling middleware. You might notice that this style of using try-catch in every route handler quickly becomes difficult to manage as you increase the number of routes.

### Wrap async function for errors

We can wrap our async function in another function so that we can refactor to make it cleaner.

```js
const wrapAsync = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(err => next(err));
};
```

We use `Promise.resolve` to ensure we are always dealing with a promise when we use `.catch` on it. Read more about `Promise.resolve` on [JavaScript Info](https://javascript.info/promise-api).

Read more about the wrapping async functions on [Stack Overflow](https://stackoverflow.com/questions/51391080/handling-errors-in-express-async-middleware/51391081) and [the 80/20 guide to Express Error Handling](http://thecodebarbarian.com/80-20-guide-to-express-error-handling).
