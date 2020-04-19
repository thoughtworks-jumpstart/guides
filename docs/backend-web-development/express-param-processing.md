# Express.js param processing

If we want to process parameters (those in the `req.params` object) before calling the handler function, we can add callback functions that will be called when a request comes to the server with a certain parameter.

## Callback function

The callback function that we will provide will look something like this.

```js
    function(req, res, next, parameterVar) {
        // do something with parameterVar
    }
```

It looks very similar with the functions we have been dealing with previously with one difference - there is a parameter variable passed to the callback function.

## Application-level

To process parameters on an application level, use `app.param`.

### Implementation

For example, if we have a route that looks like this:

```js
app.get("/users/:userId"...
```

We want to process the parameter `userId` before it is passed to this handler function.

```js
app.param("userId", (req, res, next, userId) => {
  // do whatever you like with the userId, e.g. looking up the user profile in database
  const userName = "Tom";
  const user = { userId, userName };

  console.log(`req.user is originally: ${req.user}`); //undefined
  // you can put user object inside the request object
  req.user = user;
  console.log(`req.user is now: ${req.user}`); // [object Object]

  next();
});
```

Notice that the value of the userId param is placed in the `userId` variable.

Now this handler function will be able to access the req.user that was created in the previous handler function.

```js
app.get("/users/:userId", (req, res) => {
  const user = req.user;
  res.send(
    `Got the request for user ${user.userName} whose id is ${user.userId}`
  );
});
```

## Router-level

Note that the routers will not inherit the callbacks we add using `app.param`.

Quoting the Express.js docs on `app.param`:

> Param callback functions are local to the router on which they are defined. They are not inherited by mounted apps or routers. Hence, param callbacks defined on app will be triggered only by route parameters defined on app routes.

To define the parameter callbacks on a router instead, use `router.param`.

## Exercises

Refactor the previous songs route to use `app.param` for the `id` parameter.
Find the song of that particular id and put the song in the request object as a property of the request object.
