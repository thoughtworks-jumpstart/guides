# Express.js basics

- Installing Express.js
- Features of Express.js
- Your first Express.js app
- Start Express.js server

Express.js framework is built on top of Node.js for simpler web server development. Express.js comes with parsing, security, user sessions, error handling etc, that are not handled by Node.js. If you use Node.js, you will have to constantly reimplement all these.

It is minimalistic, does not restrict you to any design patterns or file structure, and often requires you to install more packages to add functionality.

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
