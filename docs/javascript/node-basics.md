# Node.js basics

> I/O is being handled incorrectly, blocking the entire process due to synchronous programming... The right way to handle several concurrent connections is to have a single-thread, an event loop and non-blocking I/Os.

-- Ryan Dahl, Node.js Presentation, JSConf 2009

Node.js is a platform for building fast and scalable server applications using JavaScript.

It runs JavaScript applications on the V8 JavaScript runtime engine. V8 engine takes your JavaScript code and converts it into machine code. Thus, Node.js makes it possible to run JavaScript on our machines rather than just on websites in browsers.

## Why use Node.js?

For server side applications, it's a good idea to use NodeJS when you know the application is I/O bound.

For example,

- Building dynamic websites
- Building a blog system
- Build a CRUD style REST API

Don't use NodeJS to build your server side application under the following cases:

- The application is CPU bound (i.e. it needs to do heavy computation)
- The application is memory bound (e.g. it takes to loads several GBs of data into memory)

Many of the frontend developer tools (e.g. webpack, gulp, etc) are built on top of NodeJS.

## Node.js architecture

<img src="../_media/nodejs.jpg" alt="node.js architecture" width="400"/>

- libuv: the low-level I/O engine of Node.js. it encapsulate the details of different I/O APIs on various operating systems
- V8: the JavaScript engine originally developed by Google for the Chrome browser
- bindings: responsible for wrapping and exposing libuv and other low-level functionality to JavaScript
- node-core, a core JavaScript library that implements the high-level Node.js API

## Executing JavaScript

You can execute javascript scripts with `node` like this:

```
node app.js
```

## Node modules

Node.js uses the CommonJS module system.
A Node module is a reusable block of code.

Node.js has a set of built-in modules which you can use without any further installation.

You can write your own modules too!

It uses `require` and `module.exports`.

## Package management

Node.js by default uses `npm`, short for Node Package Manager, for managing packages.

## More resources

- [History of Node.js](https://blog.risingstack.com/history-of-node-js/)
