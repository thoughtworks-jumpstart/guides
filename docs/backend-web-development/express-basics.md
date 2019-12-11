# Express.js basics

Express.js framework is built on top of Node.js for simpler web server development. Express.js comes with parsing, security, user sessions, error handling etc, that are not handled by Node.js

## Installing Express.js

```
npm install express
```

## Features of Express.js

- Uses middleware to break your app into smaller bits of behaviour
- Routing helps to split your app into smaller functions that are called when user visits a resource (like a specific URL)
- Routers can break large apps into smaller subapplications

### Starting the server

```js
const server = app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}...`);
});
```

### Routing with route handlers

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

### Middleware

```js
app.use(function(req, res, next) {
  console.log("Method: " + req.method);
  console.log("Path: " + res.url);
  next();
}
```

## Testing the server with an API testing tool

To see it working, you need to use an API testing tool like Postman or Insomnia.
Using those tools, compose a request to "http://localhost:port/" where `port` is the port that your server is listening to.
