# Creating a simple web server with Express.js

Refer to the script: [Express.js playground: helloworld.js](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/helloworld.js)

Import Express.js package and create a new Express application using `express()`, putting it into the `app` variable. We will use the const `PORT` later to listen to port 3000.

```js
const express = require("express");
const app = express();
const PORT = 3000;
```

## Routing

When loading the app (sending a GET request) at the root URL `/`, the server should send a response saying "Hello World!". Define a route like this:

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

The server will start to listen at port 3000.

```js
const server = app.listen(PORT, () => {
  console.log(`Express app started on port ${PORT}...`);
});
```

Other CRUD requests:

```js
app.post("/", (req, res) => {
  res.send(`Hello, i got a ${req.method} request`);
});

app.put("/", (req, res) => {
  res.send(`Hello, i got a ${req.method} request`);
});

app.delete("/", (req, res) => {
  res.send(`Hello, i got a ${req.method} request`);
});
```
