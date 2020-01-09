# Express.js parsing request body

One of the very important middlewares is the one for parsing request body. It is built-in into Express.js.

Note that it used to be a seperate middleware called body-parser but it has been re-added into Express.js as a built-in middleware.

## Parsing json body

Remember that `req.body` is undefined because we have not parsed the message body of requests coming into our server?

Add the body parsing JSON middleware on the application level.

```js
app.use(express.json());
```

The request body of any request will be parsed by the middleware and placed in `req.body` before it is passed to the handler functions.

```js
app.post("/users", (req, res) => {
  const userName = req.body.name;
  res.send(`${userName} is created.`);
});
```
