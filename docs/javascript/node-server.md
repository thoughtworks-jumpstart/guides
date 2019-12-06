# Create a Node.js server

Refer to the github repository: [A simple server written in Node.js](https://github.com/thoughtworks-jumpstart/simple-node-server/blob/master/index.js)

## Write a simple node server

You will require a built-in module of Node.js `http` which allows Node.js to transfer data over the Hyper Text Transfer Protocol (HTTP). Import this into the file.

Note that this does not use _https_ by default (a security issue!).

```js
// index.js
const http = require("http");
const PORT = 3000;

const server = http.createServer();
```

We shall later make the server listen to port 3000.

Add a function that is called every time a HTTP request is made to the server. It is called the request handler.

```js
server.on('request', (req, res) => {
console.log('Method', req.method)
console.log('Path', req.url)
```

Node.js uses event emitters and thus you can add a callback function (a listener) to the `request` event.
To know why Node.js use event emitters with the HTTP server, [read more here](https://codeburst.io/event-emitters-and-listeners-in-javascript-9cf0c639fd63).

Continue adding code to this function.
If a user accesses the url `/books` (a GET request is made), display "Here are the books 📖" to the user

```js
if (req.url === "/books" && req.method === "GET") {
  res.end("Here are the books 📖");
}
```

Allow a user to create a new book when a POST request is made to `/books`

```js
else if (req.url === '/books' && req.method === 'POST') {
res.end('You have created a new book')
}
```

Display a default message if other urls are accessed

```js
else {
res.end('Thank you for visiting the server')
}
})
```

To serve requests, the server needs to listen to a specific port number. Listen to `PORT` number that was set earlier.

```js
server.listen(PORT, () => {
  console.log("Server is now listening on PORT", PORT);
});
```

You could also use a shorthand for adding the request handler:

```js
const server = http.createServer((req, res) => {
  // put the above request handling code here
});
```

To know more details, check the official Node.js docs about the anatomy of an [HTTP transaction](https://nodejs.org/es/docs/guides/anatomy-of-an-http-transaction/).
