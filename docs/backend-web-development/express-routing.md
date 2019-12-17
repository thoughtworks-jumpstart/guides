# Express.js routing

## What is routing?

Routing is a way to map requests to specific handlers depending on their URI and HTTP verb.

When using a browser to visit example.com/jumpstart, the raw HTTP request might look like this:

```
GET /jumpstart http/1.1
```

The HTTP request consist of a verb (GET), a URI (/jumpstart), and the HTTP version (1.1). When we are routing, we take the verb and the URI and map it to a request handler. In this case, when Express sees a GET request to /jumpstart, some code is run.

A browser (the client) can make a GET, POST, PUT or DELETE HTTP request for various URLs (e.g. GET http://localhost:3000/books or DELETE http://localhost:3000/students/42).

## Simple routing example

Refer to the script: [Express.js playground: express_basic_example_1](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_1.js)

### Request to the root

When a GET request is sent to the root `/`, "Welcome to my homepage" is the response.

```js
// express_basic_example_1.js
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
  // NOTE: the above line poses a security issue, we should always treat any user input as unsafe (see XSS attack)
});
```

Now `req.params` has a property called `bookId`.

### Multiple request handlers

Refer to the script: [Express.js playground: express_basic_example_2](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_2.js)

More than one request handler can be defined for a route.

```js
// express_basic_example_2.js
const requestHandler1 = (req, res, next) => {
  res.write("Here is a list of students:\n");
  next();
};
```

```js
const requestHandler2 = (req, res) => {
  res.write("Jesstern\n");
  res.write("Elson\n");
  res.write("Mabel\n");
  res.write("Bye!\n");
  res.end();
};
```

The first request handler passes the request to the next request handler via `next()` call
When we are not ready to send back the response yet, we use the `res.write()` method to update the response, instead of using the `res.send()` method.

If a request handler needs to send the response back to client, it should call the `res.end()` method.

You can call app.METHOD multiple times:

```js
app.get(path, requestHandler1);
app.get(path, requestHandler2);
```

or you can declare the handler in one line:

```js
app.get("/students", requestHandler1, requestHandler2);
```

The handlers are executed in the same order as declaration. In the example above, requestHandler1 is called before requestHandler2.

If you send a request to `http://localhost:3000/students", you should see the output of both route handlers, printed in the right sequence.

## Tracking HTTP requests

Using developer tools on a browser, we can track and view the HTTP requests made on a website.

For example, when we visit http://localhost:3000/books, we could use the Network developer tool to view the GET request made.

<img src="../_media/get-request.png" alt="get request on /books uri" width="600"/>
