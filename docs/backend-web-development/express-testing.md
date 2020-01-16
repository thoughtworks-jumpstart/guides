# Express.js testing

## Benefits of Tests

- Tests are a faster way to verify that your APIs are working correctly, as compared to Postman.
- Tests can act as a safety net when you are required to make changes to your API later.
- Most importantly, tests help to document a **contract** between your API and the clients accessing it

## What to test

What are our tests for our Express.js code going to test for?

We should write tests to ensure that our API are:

- returning the correct HTTP status code
- returning the correct response in the body
- for all of the routes defined

## About Integration Test

The type of tests we are going to write is called integration test.

Integration tests, as the name suggests, tests that the different layers of your application (e.g. app, router, request handlers, any middleware, models and database) works as expected when integrated together.

In contrast, there is another type of tests, called Unit Test, which focus on testing a module/class/function works alone.

## Tests for API

To verify that your API works as expected, usually you need to send some requests to a running instance of your application, and check the responses. This can be done manually using tools like Postman or Insomnia, but we prefer to automate those tests.

To automate those tests, the tests need to act as API consumers, issuing requests and verifying responses.

### Using Jest as a testing framework

#### Filename Conventions for Jest

Jest will look for test files at:

- Files with `.js`, `.jsx`, `.ts` and `.tsx` suffix in \_\_tests\_\_ folders.
- Files with `.test.js` suffix.
- Files with `.spec.js` suffix.
  The `.test.js` / `.spec.js` files (or the \_\_tests\_\_ folders) can be located at any depth under the src top level folder.

It is recommended to put the test files next to the code they are testing so that relative imports in the test appear shorter. Alternatively, you could put tests into the \_\_tests\_\_ folder.

Read more about this in the [Jest documentation](https://jestjs.io/docs/en/configuration#testregex-string--arraystring).

#### Asynchronous tests with Jest

We will like to send requests to test our API. Requests are asynchronous, therefore we need to have asynchronous tests.

How can we do this with Jest?

- Add the `async` keyword
- Call `done` when you’re done with your tests

```js
it("Async test should do nothing", async done => {
  // Do async operations here
  done();
});
```

### SuperTest

Previously, we mentioned that we will like to send requests to test our API. You can use libraries like [SuperTest](https://github.com/visionmedia/supertest).

### Installation

Install SuperTest and save it to your package.json file as a development dependency:

```sh
npm install supertest --save-dev
```

### Testing with Jest and SuperTest

SuperTest works without any framework but in our case we will use it along with Jest, since we are already using Jest as our testing framework.

#### Refactoring for tests

Before we can start testing with SuperTest, we need to refactor our code to allow our server to be started by SuperTest for each of the tests.

Currently, our app looks like this in many of the examples where the routes exist in the same file as the server.

```js
// testing_example_1.js
app.get("/", (req, res) => {
  res.send("Welcome to my homepage");
});

...

const server = app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

This doesn’t work when creating tests because it starts listening to one port. Once you create many test files, you might come across an error that says "port in use".

To solve this problem, you need to `export` the app without listening to it. Replace the `app.listen` code with `module.exports = app;`.

```js
// testing_example_1.js
module.exports = app;
```

Now how do we run the server for development or production purposes? (not testing)

Normally when we are exporting the app like above, we do this in a file called **app.js**. (for this example, we shall just use testing_example_1.js)

We create a new file called **index.js** to start the server and make the server listen to port 3000.
(for this example, the file shall be called testing_example_1_index.js)

We `require` the previous file to import the app.

```js
// testing_example_1_index.js
const app = require("./testing_example_1");
const PORT = 3000;

app.listen(PORT, () =>
  console.log(`Server running at http://localhost:${PORT}`)
);
```

Run `node testing_example_1_index.js` again to start your server. Ensure that it still works.

#### Writing tests

First we will have to import SuperTest and our app.

```js
// testing_example_1.test.js
const request = require("supertest");
const app = require("./testing_example_1");
```

Test that Jest works

```js
describe("testing_example_1", () => {
  it("Testing to see if Jest works", () => {
    expect(1).toBe(1);
  });
});
```

Let's add our first test for the GET "/" route! We shall write it in a Promises way...

```js
describe("testing_example_1", () => {
  it("GET / should respond with Welcome to my homepage", () => {
    return request(app)
      .get("/")
      .then(response => {
        expect(response.text).toEqual("Welcome to my homepage");
      });
  });
});
```

Oh no. Let's use async/await instead.

```js
describe("testing_example_1", () => {
  it("GET / should respond with Welcome to my homepage", async done => {
    const response = await request(app).get("/");
    expect(response.text).toEqual("Welcome to my homepage");
    done();
  });
});
```

Looks nicer! It passes with green.

What if we destructure the response to extract `text`?

```js
it("should respond correctly", async done => {
  const { text } = await request(app)
    .get("/")
    .expect(200);
  expect(text).toEqual("Welcome to my homepage");
  done();
});
```

`request(app)` passes the app to SuperTest. We are then able to send GET, POST, PUT, PATCH and DELETE requests.
`done` is a callback passed by Jest. Jest will wait until the done callback is called before finishing the test. Read more about it in the [Jest documentation](https://jestjs.io/docs/en/asynchronous#callbacks).
```
