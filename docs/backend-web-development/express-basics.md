# Express.js basics

Express.js framework is built on top of Node.js for simpler web server development. Express.js comes with parsing, security, user sessions, error handling etc, that are not handled by Node.js

## Installing Express.js

```
npm install express
```

## Features of Express.js

- Routing helps to split your app into smaller functions that are called when user visits a resource (like a specific URL)
- Routers can further break large apps into smaller subapplications
- Middleware functions help to process a request before it is passed on to the final handler functions

## Your first Express.js app

```js
const express = require("express");
const app = express();
const PORT = 3000;
```

You created your first Express.js app!
When express() is called in app.js, an app object is returned. Think of an app object as an Express application.

## Starting the server

```js
const server = app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}...`);
});
```

## Testing the server

### cURL

Client URL
cURL is by default already installed in Mac OS. For Windows, you can download cURL separately.
You can send different HTTP requests.

For example, let's send a POST request to https://localhost:3000/users with a JSON message body.

```sh
curl --request POST \
  --url https://localhost:3000/users \
  --header 'content-type: application/json' \
  --data '{
    "username": "babel",
}'
```

Alternatives to the flags:

```sh
curl -X POST \
  http://localhost:3000/users \
  -H 'content-type: application/json' \
  -d '{
    "username": "babel"
}'
```

### API testing tool

Instead of cURL, you can use an API testing tool like Postman or Insomnia. You can even save your requests and organise them into folders. Using those tools, compose a request to "http://localhost:port/" where `port` is the port that your server is listening to.
