# Create a Node.js server

Refer to the github repository: [A simple server written in Node.js](https://github.com/thoughtworks-jumpstart/simple-node-server/blob/master/index.js)

## Write a simple node server

You will require a built-in module of Node.js `http` which allows Node.js to transfer data over the Hyper Text Transfer Protocol (HTTP). Import this into the file.

Note that this does not use _https_ by default (a security issue!). You can decide to use HTTPS later.

```js
// index.js
const http = require("http");
const PORT = 3000;

const server = http.createServer();
```

We shall later make the server listen to port 3000.

Add a function that is called every time a HTTP request is made to the server. It is called the request handler.
It takes two arguments, `req` is an object that represents the request and `res` is an object that represents the response.

```js
// index.js
server.on('request', (req, res) => {
console.log('Method', req.method)
console.log('Path', req.url)
```

Node.js uses event emitters and thus you can add a callback function (a listener) to the `request` event.
To know why Node.js use event emitters with the HTTP server, [read more here](https://codeburst.io/event-emitters-and-listeners-in-javascript-9cf0c639fd63).

Continue adding code to this function.
If a user accesses the url `/books` (a GET request is made), display "Here are the books 📖" to the user

```js
// index.js
if (req.url === "/books" && req.method === "GET") {
  res.end("Here are the books 📖");
}
```

Allow a user to create a new book when a POST request is made to `/books`

```js
// index.js
else if (req.url === '/books' && req.method === 'POST') {
res.end('You have created a new book')
}
```

Display a default message if other urls are accessed

```js
// index.js
else {
res.end('Thank you for visiting the server')
}
})
```

To serve requests, the server needs to listen to a specific port number. Listen to `PORT` number that was set earlier.

```js
// index.js
server.listen(PORT, () => {
  console.log("Server is now listening on PORT", PORT);
});
```

You could also use a shorthand for adding the request handler:

```js
// index.js
const server = http.createServer((req, res) => {
  // put the above request handling code here
});
```

Run the server:

```
node index.js
```

Try visiting http://localhost:3000 and you will see "Thank you for visiting the server".
Try other URLs like http://localhost:3000/hello or http://localhost:3000/bye. Does the console output change? Does the page looks the same or different?

Now try visiting http://localhost:3000/books.

To make a POST request to http://localhost:3000/books, try using a API tester tool like Postman.

To know more details, check the official Node.js docs about the anatomy of an [HTTP transaction](https://nodejs.org/es/docs/guides/anatomy-of-an-http-transaction/).

## Create Node.js servers using Express.js

Node.js is a low-level, I/O framework. It does not come with security, user sessions, error handling etc. This is where the Express.js framework comes in. Express.js is built on top of Node.js to make the creation of Node.js servers a lot simpler. We will discover this when we create the same simple server in Express.js.
