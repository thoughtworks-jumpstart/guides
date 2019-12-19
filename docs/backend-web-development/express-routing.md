# Express.js routing

## What is routing?

Routing is a way to map requests to specific handlers depending on their URI and HTTP verb.

When using a browser to visit example.com/jumpstart, the raw HTTP request might look like this:

```
GET /jumpstart http/1.1
```

The HTTP request consist of a verb (GET), a URI (/jumpstart), and the HTTP version (1.1). When we are routing, we take the verb and the URI and map it to a request handler. In this case, when Express sees a GET request to /jumpstart, some code is run.

A browser (the client) can make a GET, POST, PUT or DELETE HTTP request for various URLs (e.g. GET http://localhost:3000/books or DELETE http://localhost:3000/students/42).

Refer to the script: [Express.js playground: express_basic_example_1](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_1.js)

## Request methods

A request method is derived from one of the HTTP methods like GET, POST and PUT etc.

Route that is defined for the GET method to the root of the app:

```js
// express_basic_example_1.js
app.get("/", (req, res) => {
  res.send("Welcome to my homepage");
});
```

The request handler (callback function) is called when a GET request is sent by the browser to the root `/`. In the request handler, "Welcome to my homepage" is sent as the response.

Route that is defined for the POST method to the root of the app:

```js
app.post("/", (req, res) => {
  res.send("POST request to the homepage");
});
```

## Route paths

Route paths, in combination with a request method, define the endpoints at which requests can be made. Route paths can be strings, string patterns, or regular expressions. We shall see more examples soon.

### Fixed route path

The request handler is called when a GET request is sent by the browser to the fixed route `/books`. In the request handler, "You requested a list of books...." is sent as the response.

```js
app.get("/books", (req, res) => {
  res.send("You requested a list of books....");
});
```

### Route parameters

We can grab data from the route using route parameters. Route parameters are named segments of the URL used to capture values at their specified position. 

GET a specific book with ID:

```js
app.get("/books/:bookId", (req, res) => {
  res.send(`You requested information on book ${req.params.bookId}`);
  // NOTE: the above line poses a security issue, we should always treat any user input as unsafe (see XSS attack)
});
```
This route will match paths `/books/1`, `/books/2` and so on. Will it match `/books/abc`? 

`bookId` is used to capture values at that position of the path. Thus `req.params` has a property called `bookId` storing `1` or `2` etc depending on the path.

### Regular expressions

This route path will match /books and /book.

```js
app.get('/books?', function (req, res) {
  res.send('You requested a list of books...')
})
```

This route path will match butterfly and dragonfly, but not butterflyman, dragonflyman, and so on.

```js
app.get(/.*fly$/, function (req, res) {
  res.send('/.*fly$/')
})
```

### Multiple request handlers

Refer to the script: [Express.js playground: express_basic_example_2](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_2.js).

More than one request handler can be defined for a route.

Request handler 1:

```js
// express_basic_example_2.js
const requestHandler1 = (req, res, next) => {
  res.write("Here is a list of students:\n");
  next();
};
```

Request handler 2:
```js
const requestHandler2 = (req, res) => {
  res.write("Jesstern\n");
  res.write("Elson\n");
  res.write("Mabel\n");
  res.write("Bye!\n");
  res.end();
};
```

The first request handler passes the request to the next request handler via `next()` call.

When we are not ready to send back the response yet, we use the `res.write()` method to update the response, instead of using the `res.send()` method.

If a request handler needs to send the response back to client, it should call the `res.end()` method.

You can call `app.METHOD()` multiple times:

```js
app.get(path, requestHandler1);
app.get(path, requestHandler2);
```

or you can declare the handler in one line:

```js
app.get("/students", requestHandler1, requestHandler2);
```

The handlers are executed in the same order as declaration. In the example above, requestHandler1 is called before requestHandler2.

If you send a request to http://localhost:3000/students, you should see the output of both route handlers, printed in the right sequence.


For more examples, see the [express docs](https://expressjs.com/tr/guide/routing.html).