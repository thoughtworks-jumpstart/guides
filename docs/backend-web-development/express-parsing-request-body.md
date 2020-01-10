# Express.js parsing request body

One of the very important middlewares is the one for parsing request body. It is built-in into Express.js.

Note that it used to be a seperate middleware called body-parser but it has been re-added into Express.js as a built-in middleware.

## Parsing json body

Remember that `req.body` is undefined because we have not parsed the message body of requests coming into our server?

Let's now parse the message body as JSON.

Add the JSON body parsing middleware on the application level.

```js
app.use(express.json());
```

`express.json()` returns the middleware that parses JSON (a function).

```js
typeof express.json(); // 'function'
```

The request body of any request will be parsed by the middleware and placed in `req.body` before it is passed to the handler functions.

```js
app.post("/users", (req, res) => {
  const userName = req.body.username;
  res.send(`You would like to create a user with username: ${userName}.`);
});
```

Use Postman to send a POST request message with a JSON message body to your server at the `/users` route. Notice that there will be a very ugly error if you send invalid JSON or something that is not JSON.

```json
{
  "username": "babel"
}
```

<img src="backend-web-development/_media/parsing-json-request-body.png" alt="parsing a json request message body with postman" width="700"/>

## Parsing URL encoded form body

To parse HTML forms, we can use the urlencoded body parsing middleware.

```js
app.use(express.urlencoded({ extended: false }));
```

Read https://codewithhugo.com/parse-express-json-form-body/ for more information.
