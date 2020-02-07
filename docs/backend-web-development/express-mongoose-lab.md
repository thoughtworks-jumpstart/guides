## Pokemon

We will be building an API for returning pokemon data with Mongoose.

## Planning the CRUD API

Lab: Build a basic CRUD API for the pokemon list (Create / Read / Update / Delete)

Requirements
In this lab we will implement a basic CRUD API in Express for pokemon list with the below 8 routes:

We are creating an API to interact with the resources on the server.

#### 0. Get API endpoints

Route: GET /
HTTP Response status code: 200  
Expected response:

```json
{
  "0": "GET    /",
  "1": "GET   /pokemons",
  "2": "GET   /pokemons?name=pokemonNameNotExact",
  "3": "POST    /pokemons",
  "4": "GET /pokemons/:id",
  "5": "PUT /pokemons/:id",
  "6": "PATCH /pokemons/:id",
  "7": "DELETE /pokemons/:id"
}
```

Notice the plural form. We have 8 endpoints.

Pokemon looks like this:
Note the slight change in schema, we have id now!

```js
const pokemonData = [
  {
    id: 1,
    name: "Pikachu",
    japaneseName: "ピカチュウ",
    baseHP: 35,
    category: "Mouse Pokemon",
  },
  {
    id: 2,
    name: "Squirtle",
    japaneseName: "ゼニガメ",
    baseHP: 44,
    category: "Tiny Turtle Pokemon",
  },
];
```

#### 1. Find all pokemon

Route: GET /pokemons
HTTP Response status code: 200

#### 2. Filter pokemon by name (not exact match, use regex)

Route: GET /pokemons?name=pokemonNameNotExact
HTTP Response status code: 200

#### 3. Add pokemon

Route: POST /pokemons
HTTP Response status code: 201

#### 4. Get pokemon by id

Route: GET /pokemons/:id
HTTP Response status code: 200

#### 5. Replace pokemon with id

Route: PUT /pokemons/:id
HTTP Response status code: 200

`findOneAndReplace`
Respond with new pokemon

#### 6. Update pokemon with id

Route: PATCH /pokemons/:id
HTTP Response status code: 200

`findOneAndUpdate`
Respond with updated pokemon

#### 7. Delete pokemon with id

Route: DELETE /pokemons/:id
HTTP Response: 200

Respond with deleted pokemon

Setup a new Github project and install Express.js

## Folder structure

- package.json
- package-lock.json
- src
  - controllers
  - models
  - routes
  - utils
  - app.js
  - index.js
- \_\_tests\_\_

## Routing

Lab: Add basic routes

## Error

Lab: Integrate a default error handler middleware for your songs routes

## Mongo and Mongoose

### Connect to database

```js
const mongoose = require("mongoose");

const mongoOptions = {
  useNewUrlParser: true, // prevent deprecation warnings
  useUnifiedTopology: true,
  useFindAndModify: false, // For find one and update
  useCreateIndex: true, // for creating index with unique
};

const dbName = "expressPokemon";
const dbUrl = process.env.MONGO_URI || "mongodb://localhost:27017/" + dbName;
mongoose.connect(dbUrl, mongoOptions);
const db = mongoose.connection;

db.on("error", console.error.bind(console, "connection error:"));
db.once("open", () => {
  console.log("connected to mongodb");
});
```

### Create schema, validate, create model

Remember a pokemon looks like this

```json
{
  "id": 1,
  "name": "Pikachu",
  "japaneseName": "ピカチュウ",
  "baseHP": 35,
  "category": "Mouse Pokemon"
}
```

## Testing

Lab: Implement route tests with Jest and Supertest (with database) and test for error too

## More routing

Lab: Integrate app.param() middleware to find pokemon from id

## Routers

Lab: Integrate Express routers to organise your songs routes

## Add authentication to protect routes

cookie parser
cors options
jwt

```js
const protectRoute = (req, res, next) => {
  try {
    if (!req.cookies.token) {
      throw new Error("You are not authorized");
    }
    req.user = jwt.verify(req.cookies.token, process.env.JWT_SECRET_KEY);
    next();
  } catch (err) {
    res.status(401).end();
  }
};
```

## Add tests for protected routes

jest.mock("jsonwebtoken");

## Express to production
