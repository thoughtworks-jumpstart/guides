# Creating a simple web server with Express.js

Refer to the script: [Express.js playground: helloworld.js](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/helloworld.js)

Import Express.js package and create a new Express application using `express()`, putting it into the `app` variable. We will use the const `PORT` later to listen to port 3000.

```js
const express = require("express");
const app = express();
const PORT = 3000;
```

## GET request method

When loading the app (sending a GET request) at the root URL `/`, the server should send a response saying "Hello World!". Define a route like this:

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

You have just defined a route to the root of the app! Let's start the server by making it listen at port 3000.

```js
const server = app.listen(PORT, () => {
  console.log(`Express app started on port ${PORT}...`);
});
```

Use an API testing tool like Postman to test the route you just created.

### Other request methods

When making a POST, PUT or DELETE request at the root URL `/`, the server should send a response.

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
