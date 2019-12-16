# Express.js routing

## What is routing?

Routing is a way to map requests to specific handlers depending on their URL and HTTP verb.

When using a browser to visit example.com/jumpstart, the raw HTTP request might look like this:

```
GET /jumpstart http/1.1
```

The HTTP request consist of a verb (GET), a URI (/jumpstart), and the HTTP version (1.1). When we are routing, we take the verb and the URI and map it to a request handler. In this case, when Express sees a GET request to /jumpstart, some code is run.

## Simple routing example

Refer to the script: [Express.js playground: express_basic_example_1](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_1.js)

### Request to the root

When a GET request is sent to the root `/`, "Welcome to my homepage" is the response.

```js
app.get("/", (req, res) => {
  res.end("Welcome to my homepage");
});
```

### Matching fixed routes

The request handler (callback function) is then called when a request to /books comes in. Thus, when a GET request is sent to /books, a message is sent back in the response.

```js
app.get("/books", (req, res) => {
  res.send("You requested a list of books....");
});
```

### Matching complex routes

We can grab data from routes using request parameters.
GET a specific book with ID:

```js
app.get("/books/:bookId", (req, res) => {
  res.send(`You requested information on book ${req.params.bookId}`);
  // this poses a security issue
});
```

Now `req.params` has a property called `bookId`.
