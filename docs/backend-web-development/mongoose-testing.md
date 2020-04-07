# Mongoose testing

## Mock database vs real database

If we are writing unit tests, by definition we need to mock the interaction with database and we don't depend on a real database.

For integration test/contract test (e.g. on web service build using Express + MongoDB), we need to make the server up and running before we can send requests to it. Most of the time, that would require us to have a real database.

If somehow your tests need to depend on a real database, you need to make sure each test case has a clean database to start with. One solution is to set up and tear down all the collections in the database is necessary for ensuring there are no side effects between unit tests. In practice, this means a `beforeEach()` where you reconnect to the database and drop all collections, and an `afterEach()` where you disconnect from the database.

Another solution is to set up an **in-memory database** for each test case programmatically to avoid some of the issues with setting up a real database and sharing one database with all tests.

## MongoDB memory server

To write API tests for an Express app that uses MongoDB as the database, we are going to use a library called **mongodb-memory-server**. It spins up an in-memory instance of MongoDB, which is faster than running a separate MongoDB instance.

The library helps to give us a clean/empty database for each test case, so that the test cases do not interfere with each other (e.g. if a test case fail and leave some garbage data in its copy of database, that failure will not affect other test cases because each test case starts with a clean database).

Thus along with jest and supertest, we will need mongodb-memory-server.

```sh
npm install mongodb-memory-server --save-dev
```

Here is an example on using it with Jest: https://github.com/nodkz/mongodb-memory-server#simple-jest-test-example

before writing any test...

```js
//pokemons.route.test.js
const request = require("supertest");
const app = require("../../src/app");
const Pokemon = require("../../src/models/pokemon.model");
const mongoose = require("mongoose");
const { MongoMemoryServer } = require("mongodb-memory-server");

mongoose.set("useNewUrlParser", true);
mongoose.set("useFindAndModify", false);
mongoose.set("useCreateIndex", true);
mongoose.set("useUnifiedTopology", true);

describe("pokemons", () => {
  let mongoServer;
  beforeAll(async () => {
    try {
      mongoServer = new MongoMemoryServer();
      const mongoUri = await mongoServer.getConnectionString();
      await mongoose.connect(mongoUri);
    } catch (err) {
      console.error(err);
    }
  });

  afterAll(async () => {
    await mongoose.disconnect();
    await mongoServer.stop();
  });

  beforeEach(async () => {
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
    await Pokemon.create(pokemonData);
  });

  afterEach(async () => {
    await Pokemon.deleteMany();
  });

```

If you try testing with mongoose now, you will see the following warning

```sh
Mongoose: looks like you're trying to test a Mongoose app with Jest's default jsdom test environment. Please make sure you read Mongoose's docs on configuring Jest to test Node.js apps: http://mongoosejs.com/docs/jest.html
```

Add this to package.json

```json
//package.json
"jest": {
    "testEnvironment": "node"
  }
```

First test for GET `/pokemons`

```js
describe("/pokemons", () => {
  it("GET should respond with all pokemons", async () => {
    const expectedPokemonData = [
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

    const { body: actualPokemons } = await request(app)
      .get("/pokemons")
      .expect(200);
    expect(actualPokemons).toMatchObject(expectedPokemonData);
  });
});
```

For testing database, very often we will use `toMatchObject`.
This is because the database objects have extra attributes `_id` and `_v`.

POST

```js
router.post("/", async (req, res, next) => {
  try {
    const pokemon = new Pokemon(req.body);
    await Pokemon.init(); // make sure indexes are done building
    const newPokemon = await pokemon.save();
    res.status(201).send(newPokemon);
  } catch (err) {
    if (err.name === "ValidationError") {
      err.status = 400;
    }
    next(err);
  }
});
```

https://stackoverflow.com/questions/45692456/whats-the-difference-between-tomatchobject-and-objectcontaining

https://formidable.com/blog/2019/fast-node-testing-mongodb/

## Another strategy for doing integration testing with database

In the example above, we make use of the mongodb-memory-server to automatically give us a fresh database in each test case.

Without that library, another possible solution is to explicitly delete all data in the test database after each test finishes running. That's a bit tedious but still works. Here is a [sample project showing this approach](https://github.com/thoughtworks-jumpstart/express-blog-api-mongoose-and-tests). Check out the test cases in tests/integration-tests to see the sample tests.
