# Express.js parsing request body

One of the very important middlewares is the one for parsing request body. It is built-in into Express.js. This is often forgotten and the cause of bugs.

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

It is important to `app.use` the json parsing middleware **before** the handler functions. The order of adding middleware matters.

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

## Exercises

Add more API endpoints to the previous songs API that we had:

Add middleware for parsing JSON request body.

1. Route: POST /songs

HTTP Response status code: 201

Add a new song with id, and return new song

JSON to include in the POST message body:

```json
{
  "name": "someSongName",
  "artist": "someSongArtist"
}
```

Expected response:

```json
{
  "id": 1,
  "name": "someSongName",
  "artist": "someSongArtist"
}
```

2. Route: GET /songs/:id

HTTP Response status code: 200

Return a song with id using route parameters.

if route path is /songs/1, expected response is:

```json
{
  "id": 1,
  "name": "someSongName",
  "artist": "someSongArtist"
}
```

3. Route: PUT /songs/:id

HTTP Response status code: 200

Replace a song with id, and return modified song

JSON to include in the PUT message body:

```json
{
  "name": "newSongName",
  "artist": "newSongArtist"
}
```

If the route path is /songs/1:

Expected response:

```json
{
  "id": 1,
  "name": "newSongName",
  "artist": "newSongArtist"
}
```

4. Route: DELETE /songs/:id

HTTP Response status code: 200

delete a song with id, and return deleted song

if route path is /songs/1, expected response is:

```json
{
  "id": 1,
  "name": "someSongName",
  "artist": "someSongArtist"
}
```
