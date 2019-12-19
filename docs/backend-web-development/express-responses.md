# Express.js responses

The request handler usually needs to call a method on the response object to send back some responses.

#### res.send()

In the routing example, we used `res.send()` to send a string as a response and to close the connection.

```js
res.send("Welcome to my homepage");
```

If you pass in a string , it sets the `Content-Type` header to text/html.

If you pass in an object or an array, it sets the application/json `Content-Type` header, and parses that parameter into JSON.

`res.send()` automatically sets the `Content-Length` HTTP response header and closes the connection. You should only call `res.send()` once.

### res.write()
In the multiple request handler routing example, we used `res.write()` to update the response before sending the response back to the client.

```js
res.write("Here is a list of students:\n");
```

`res.write()` allows you to provide successive parts of the response body and can be called multiple times.

```js
response.write('<html>');
response.write('<body>');
response.write('<h1>Hello, World!</h1>');
response.write('</body>');
response.write('</html>');
response.end();
```

See [nodejs docs](https://nodejs.org/en/docs/guides/anatomy-of-an-http-transaction/#sending-response-body) for more information.

### What's the difference between res.send() and res.write() API?

The major difference is the `res.send()` API automatically set `Content-Type` in response header. That's handy sometimes, but if you call `res.send()` API multiple times, you will get error like "Can't set headers after they are sent." You can also imagine `res.send()` as almost equivalent to `res.write()` + `res.end()`.

To avoid that issue, you can set `Content-Type` response header by yourself, call `res.write()` multiple times, and call `res.end()` at last.

See [this stackoverflow response](https://stackoverflow.com/questions/44692048/what-is-the-difference-between-res-send-and-res-write-in-express) for more information.

## Setting status code

Sets the response HTTP status code to 200 and calls the `res.send()` method to send the corresponding string message.

```js
res.status(200).send("OK");
```

This can be simplified with `sendStatus(statusCode)` which will send the default string message of the status code for you.

```js
res.sendStatus(200);
```

See [Express.js api docs](https://expressjs.com/en/api.html#res.sendStatus) for more info.