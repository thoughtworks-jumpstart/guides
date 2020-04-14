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
  console.log(`Express app started on http://localhost:${PORT}`);
});
```

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

## Testing the server

### cURL

Client URL
cURL is by default already installed in Mac OS. For Windows, you can download cURL separately.
You can send different HTTP requests.

For example, let's send a POST request to https://localhost:3000/ with a JSON message body.

```sh
curl --request POST \
  --url https://localhost:3000/ \
  --header 'content-type: application/json' \
  --data '{
    "username": "babel",
}'
```

Alternatives to the flags:

```sh
curl -X POST \
  http://localhost:3000/ \
  -H 'content-type: application/json' \
  -d '{
    "username": "babel"
}'
```

### API testing tool

Instead of cURL, you can use an API testing tool like Postman or Insomnia. You can even save your requests and organise them into folders. Using those tools, compose a request to "http://localhost:port/" where `port` is the port that your server is listening to.

## Exercises

1. Create a new Node.js project
2. Install express
3. Create your first server
4. Start and leave your server running
5. Use cURL and Postman to test your server, send GET, POST, PUT, DELETE requests
