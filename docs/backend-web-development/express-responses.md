# Express.js responses

- Ending response with res.end()
- Sending back response with res.send() and res.json()
- Writing to res.write() and comparing to res.send()
- Setting status code of response

The handler function or middleware function usually needs to call a method on the response object (res) to modify and send back a response.

## Ending a response

We have ended a response by using `res.end` when we were creating a basic Node.js server previously without Express.js.

```js
res.status(404).end();
```

We will use `end()` to end the response without providing any data.

Why do we need to end a response? We need to close the HTTP connection or else the connection will be kept alive until the end of the server timeout. Read about [Concurrent HTTP connections in Node.js](https://blog.fullstacktraining.com/concurrent-http-connections-in-node-js/) for more info.

## Sending responses

By calling the `res.send()` method, we can send a response.

The signature of the method is `res.send([body])` where the body can be a Buffer, String, an Object or an Array.

The `Content-Type` header is automatically set based on the type of the body passed to the method. If you pass in a string , it sets the `Content-Type` header to text/html. If you pass in an object or an array, it sets the header to application/json, and parses that parameter into JSON.

### Sending a string response

In the previous routing example, we already used `res.send()` to send a string as a response and to close the connection.

```js
res.send("Welcome to my homepage");
```

`res.send()` automatically sets the `Content-Type` and `Content-Length` HTTP response headers and closes the connection. You should only call `res.send()` once.

### Sending a JSON response

In the previous routing example. we have also already used `res.json()` to send JSON to the client as a response and to close the connection.

It is different from `res.send()` because it respects the JSON application settings such as `json spaces` and `json replacer`.

It can accept an object or array, and converts it to JSON before sending it:

```js
res.json({ username: "Flavio" });
res.status(500).json({ error: "message" });
```

It will also convert _null_ and _undefined_ which are not valid JSON to JSON.

```js
res.json(null);
```

Read [this blog at Fullstacktraining](https://blog.fullstacktraining.com/res-json-vs-res-send-vs-res-end-in-express/) for more information.

## Writing parts of response body

In the multiple handler functions routing example, we used `res.write()` to update the response before sending the response back to the client. Normally, you will not need to use this.

```js
res.write("Here is a list of students:\n");
```

`res.write()` allows you to provide successive parts of the response body and can be called multiple times.

```js
response.write("<html>");
response.write("<body>");
response.write("<h1>Hello, World!</h1>");
response.write("</body>");
response.write("</html>");
response.end();
```

See [nodejs docs](https://nodejs.org/en/docs/guides/anatomy-of-an-http-transaction/#sending-response-body) for more information.

### What's the difference between res.send() and res.write() API?

The major difference is the `res.send()` API automatically set `Content-Type` in response header. That's handy sometimes, but if you call `res.send()` API multiple times, you will get error like "Can't set headers after they are sent." You can also imagine `res.send()` as almost equivalent to `res.write()` + `res.end()`.

To avoid that issue, you can set `Content-Type` response header by yourself, call `res.write()` multiple times, and call `res.end()` at last.

See [this stackoverflow response](https://stackoverflow.com/questions/44692048/what-is-the-difference-between-res-send-and-res-write-in-express) for more information.

## Setting HTTP response status code

Set the response HTTP status code to 200 and call the `res.send()` method to send the corresponding string message.

```js
res.status(200).send("OK");
```

This can be simplified with `sendStatus(statusCode)` which will send the default string message of the status code for you.

```js
res.sendStatus(200);
// === res.status(200).send('OK')

res.sendStatus(403);
// === res.status(403).send('Forbidden')

res.sendStatus(404);
// === res.status(404).send('Not Found')

res.sendStatus(500);
// === res.status(500).send('Internal Server Error')
```

See [Express.js api docs](https://expressjs.com/en/api.html#res.sendStatus) for more info.

## Should we implement many status codes?

No. The more status codes your API deals with, the more status codes you have to maintain. Your clients (possibly your frontend) will also have to maintain more code to react accordingly to these status codes. Use the status codes only as needed.

## Exercises

Implement an API with the following endpoint:

1. Route: GET /songs
   HTTP Response status code: 200

Expected response:

if empty:

```json
[]
```

if not empty, we will get an array of song objects in JSON.

```json
[
  {
    "name": "someSongName",
    "artist": "someSongArtist"
  },
  {
    "name": "anotherSongName",
    "artist": "anotherArtist"
  }
]
```
