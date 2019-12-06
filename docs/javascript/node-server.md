# Create a Node.js server

Refer to the github repository: [A simple server written in Node.js](https://github.com/thoughtworks-jumpstart/simple-node-server/blob/master/index.js)

## Write a simple node server

You will require a built-in module of Node.js `http` which allows Node.js to transfer data over the Hyper Text Transfer Protocol (HTTP).

Note that this does not use https by default (a security issue!).

```js
const http = require("http");
const PORT = 3000;

const server = http.createServer();
```

To know more details, check the official Node.js docs about the anatomy of an [HTTP transaction](https://nodejs.org/es/docs/guides/anatomy-of-an-http-transaction/).
