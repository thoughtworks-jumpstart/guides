# Express.js routers

For a very small app, we can define all the API endpoints in one file, like what we did in the previous examples. However, as our application grows, it's a good idea to break the application into smaller modules. In terms of the API end points, it would be better to group API endpoints by their nature.

Use the express.Router class to create modular, mountable route handlers.Each Router is like a mini-app or subapplication, which means it can contain its own API endpoints, together with many other things such as middleware and error handlers, as you will notice later.

## Example

You can mount them to an app. For example, a router can be mounted to the `/empty` route of an app.

```js
const router = express.Router();

router.get("/", (res, req) => res.end());
router.post("/", (res, req) => res.end());

app.use("/empty", router);
```

## Implementation

It is extremely simple to create a router.

```js
const router = express.Router();
```

The difficult part is deciding / designing what to put in it.

For example, if we are implementing an API for a library system, the API endpoint for users and books can be put into their own routers / modules. We can refactor our basic example to use routers.

### Basic organisation of routes

Refer to the script: [Express.js playground: express_basic_example_with_router](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/express_basic_example_with_router.js).

Firstly, create a router file for the users and shift routes related to users there.

```js
// users.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res, next) => {
  res.send("You requested a list of users....");
});

router.get("/:userId", (req, res, next) => {
  res.send(`You request information on user ${req.params.userId}`);
});

module.exports = router;
```

Create a router file for the books and shift routes related to books there.

```js
// books.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res, next) => {
  res.send("You requested a list of books....");
});

router.get("/:bookId", (req, res, next) => {
  res.send(`You request information on book ${req.params.bookId}`);
});

module.exports = router;
```

Setup the routes for your app by using the routers.

```js
// express_basic_example_with_router.js
const express = require("express");
const app = express();
const PORT = 3000;

const booksRouter = require("./books");
const usersRouter = require("./users");

app.use("/books", booksRouter);
app.use("/users", usersRouter);

const server = app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

It is convention to put the routers into a `routes` folder, but Express.js will allow you to have any file structure that you want.

```js
const booksRouter = require("./routes/books");
const usersRouter = require("./routes/users");
```

### Using routers for API versioning

Refer to the script: [Express.js playground: library.js](https://github.com/thoughtworks-jumpstart/express-playground/blob/master/library.js).

Imagine having a famous API that many applications depend on. You feel that it is getting old, and you will like to update it. However, you are unable to change the API because people are still using your API and you cannot make breaking changes or else their applications will not work.

There are various ways to deal with different versions as we have briefly mentioned in the [REST and CRUD topic](backend-web-development/rest-api). A simple solution (but might not be the best) is to add version information to your API.

For example, a request that wants to use version 1 of your library API might come to this URL:

```
/v1/ library
```

A request that wants to use version 2 of your library API might come to this URL:

```
/v2/ library
```

Create a new file `library_v1.js` for version 1 of your API.

```js
// library_v1
const express = require("express");
const api = express.Router();
const PORT = 3000;

const booksRouter = require("./routes/books");
const usersRouter = require("./routes/users");

api.use("/books", booksRouter);
api.use("/users", usersRouter);

api.get("/", (req, res) => {
  res.send("Version 1 of library API");
});

module.exports = api;
```

How has the file changed? Notice that the api is a router instead of a full app now. We then export the api router.

Create a new file `library.js` for the main app code. We will use the version 1 api router in the app.

```js
// library.js
const express = require("express");
const apiVersion1 = require("./library_v1");
const app = express();
const PORT = 3000;

app.use("/v1", apiVersion1);

const server = app.listen(PORT, () => {
  console.log(`Library API running on http://localhost:${PORT}`);
});
```

When the time comes and you want to implement version 2 of your api, you can create another router.

```js
// library_v2
const express = require("express");
const api = express.Router();
const PORT = 3000;

const booksRouter = require("./routes/books");
const usersRouter = require("./routes/users");

api.use("/books", booksRouter);
api.use("/users", usersRouter);

api.get("/", (req, res) => {
  res.send("Version 2 of library API");
});

module.exports = api;
```

Use the version 2 API in your app `library.js`.

```js
//library.js
const express = require("express");
const apiVersion1 = require("./library_v1");
const apiVersion2 = require("./library_v2");
const app = express();
const PORT = 3000;

app.use("/v1", apiVersion1);
app.use("/v2", apiVersion2);

const server = app.listen(PORT, () => {
  console.log(`Library API running on http://localhost:${PORT}`);
});
```

Now we can support both API versions!

What do you see when you go to http://localhost:3000/v1 on the browser?
How about http://localhost:3000/v2 ?

## Exercises

Now that we have both movies and songs routes on the same App.js file, we should split them into a movies router and songs router.

Make sure that the tests are passing as you refactor the routes.
After refactoring the code to their individual routers, we shall refactor the tests to their individual test files too.
