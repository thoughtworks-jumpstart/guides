# Jest

## Testing with Jest

Here are the commonly used JavaScript test runner libraries: **jest**, **jasmine**, **mocha**. They are very similar in their functionality and API design, so if you master one, chances are - learning the others won't be difficult.

In this chapter, we will learn **jest** because:

- jest has feature-parity with other test runners (e.g. mocha, jasmine)
- jest is fast
- minimal configuration is required

## Installation

Project-specific installation

- Install jest in the project: `npm install --save-dev jest`
- Add the test script to package.json:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  ...
  "scripts": {
  "test": "jest",
  "test:watch": "jest --watch"
}
```

- Run tests with `npm test`

## Writing tests and assertions

### Create a test file

Naming convention:
For a `someName.js` file, we have `someName.test.js` test file.

### Defining a test with the test() function:

```js
test("a descriptive message to describe this test", () => {
  // this is an anonymous callback function where we will specify our assertions
});
```

```js
test("2 + 2 should return 4", () => {
  expect(2 + 2).toEqual(4);
});
```

### Another way of defining a test, using the it() function

```js
it("should return 4 when 2 + 2", () => {
  expect(2 + 2).toEqual(4);
});
```

The naming of a test should read like normal english: "it should return 4 when 2 + 2"

### Grouping related tests into a block

```js
describe("Addition", () => {
  it("should return 4 when 2 + 2", () => {
    expect(2 + 2).toEqual(4);
  });

  it("should return 1 when 2 + -1", () => {
    expect(2 + -1).toEqual(1);
  });

  // more tests related to Addition
});

describe("Subtraction", () => {
  it("should return 0 when 2 - 2", () => {
    expect(2 - 2).toEqual(0);
  });

  it("should return -1 when 2 - 3", () => {
    expect(2 - 3).toEqual(-1);
  });

  // more tests related to Subtraction
});
```

The naming of a test should read like normal english: "Addition, it should return 4 when 2 + 2"

Pro-tip:
When writing the first test in the new project, start with a simple assertion as a 'smoke' test, just to verify that everything's working. Once it works, you can replace it a real test

```js
it("should work", () => {
  expect(1).toEqual(1);
});
```

### Assertion syntax

#### Basic matcher methods

expect() returns an 'expectation' object, which has several matcher methods (see full list of matcher methods on the Jest expect reference)
`expect(actual).toEqual(someValue)`
`expect(actual).toHaveLength(number)`

Some other matcher methods:
`expect(actual).not.toEqual(someValue)` & `expect(actual).not.toBe(someObject)`
`expect(actual).toBeGreaterThan(number)`
`expect(actual).toBeLessThan(number)`
`expect(actual).toContain(item)`
`expect(actual).toContainEqual(item)`
`expect(actual).toHaveProperty(keyPath, value)`
`expect(actual).toBe(someObject)`

Note: toBe() uses Object.is to test object equality. If you want to check the value of an object, use toEqual() instead:

Example:

```js
it("should use toEqual() when checking object value equality", () => {
  let dog1 = {
    name: "dog",
  };
  let dog2 = {
    name: "dog",
  };
  expect(dog1).toEqual(dog2);
  // expect(dog1).toBe(dog2) // this will fail
});
```

Truthiness tests:

```js
it("null", function () {
  const n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
  expect(n).not.toBeTruthy();
  expect(n).toBeFalsy();
});
```

## VS Code Extension for snippets

Install Jest Snippets (andys8.jest-snippets). This will be handy.

Open command palette: `Shift + Cmd + P`

Type **install extensions**.
Search for **Jest Snippets**.

Install and reload

Start using the autocomplete feature in your test file.

See list of commands here: https://github.com/andys8/vscode-jest-snippets

Help: VS Code does not automatically show the choices for expect()

In VS Code, if you type `expect(something).`, then you should see a pop up of choices for what to expect (e.g. toEqual, toBeTrue). If you don't see this list, that means your project is not configured properly and VS Code does not know what to show.

To fix this, run the command in your project directory `npm install add -D @types/jest`
