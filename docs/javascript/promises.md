# Promises

## What is a Promise?

The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.

In its most basic terms, a promise is an object that defines a method called then. The promise object represents a value that may be available some time in the future. It greatly simplifies asynchronous logic in JavaScript.

## Promise vs Callback

Before we discuss Promise in more details, I would like you to see how it's used.

In the example below, we implement some logic to handle user logins to a server side application. In this login process, there are three asynchronous tasks, and each task depends on the previous task (i.e. the output of one task becomes the input of the next task.)

- A user logins with a user name and password.
- We search the database to get more information about this user.
- We update the user's profile information and save it back to the database.

![asynchronous tasks in a chain](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LNYRzjqNI4689hrSMPs%2F-LNYS2uj8VOcM1Ubx87f%2Fasync-task.png?generation=1538193119057805&alt=media)

(asynchronous tasks in a chain)

If these tasks were synchronous tasks (i.e. if they do not involve any asynchronous operations), we could have written the codes like below:

```js
let user = User.login("user", "pass");
let results = query.find(user);
let result = results[0].save({ key: value });
// make use of the result
```

However, suppose the 3 tasks (login/find/save) are all asynchronous tasks, you cannot get the results right away after start executing a task. You have to provide some callbacks to process the results becomes ready.

The nested callbacks easily leads to the callback-hell approach:

```js
User.login("user", "pass", {
  success: function (user) {
    query.find({
      success: function (results) {
        results[0].save(
          { key: value },
          {
            success: function (result) {
              // the object was saved
            },
          }
        );
      },
    });
  },
});
```

Compare that with the following code with the much more elegant Promise workflow, with first-class error handling:

```js
User.login('user', 'pass')
  .then(function (user) { return query.find(user); })
  .then(function (results) { return results[0].save({ key: value }); })
  .then(function (result) { // the object was saved })
  .catch(function (err) { // an error happened somewhere in the process });
```

Note: you still need to use callbacks with Promise. However, now the callbacks are not nested within each other anymore! :)

# Promise API (in ES6)

## Mother's day promise example

It's Mother's day this Friday.

### Creation of a promise and how to resolve

On Monday, I happily told her: "I will have dinner with you on Friday".
This is a **promise**.

Before Friday, this promise is in **pending** state, and on Friday, it will be **resolved**.

At that time, there are two possible outcomes:

- I book a nice restaurant and have dinner with my mother. In that case, the promise is **fulfilled**.
- I got an urgent meeting on Friday evening, so I have to cancel the dinner and tell her I can't have dinner with her. In this case, the promise becomes **rejected**.

### Reaction to a promise

Knowing that I don't always keep my promise (:( oh no ), my mother has the following plans:

- If the promise is **fulfilled**, she will buy me a gift in return.
- If the promise is **rejected**, she will complain to my father.

On friday, she will observe the outcome of the promise and take action accordingly.

### Properties of a promise

One important property about the promise is that it **can only be either fulfilled or rejected**. There is no 3rd outcome, and it cannot be both fulfilled and rejected at the same time.

Another important property about this promise is, after this Friday, once it's resolved, it will become either "fulfilled" or "rejected" and it would **remain in that resolved state forever**. Even if I look back at this promise after one year, the fact that I keep it (or I don't) does not change after this Friday.

### Two parties / roles to a promise

It also worths highlighting that there are two parties/roles in this story:
One party that creates a promise and finally decides when to resolve the promise, and if the promise should be fulfilled or rejected. That's me when I make the promise to my mother.

The other party that receives the promise can register their action plans and wait to be notified on the outcome, just like my mother. The recipient/observer of a promise have no influence on whether the promise is fulfilled or rejected, nor can they decide when the promise would be resolved.

We will first learn:

- How to create a new Promise
- How to notify the observers of the Promise when it's resolved (either fulfilled or rejected)

## Creating a new Promise

```js
const dinnerPromise = new Promise(function (fulfill, reject) {
  const waitTillThisFriday = 5000; // 5 days in 5 secs?
  setTimeout(() => {
    try {
      finishMyWorkOnFriday();
      const restaurant = bookRestaurant();
      const flower = buyFlower();
      fulfill({ restaurant, flower });
    } catch (badNews) {
      reject(badNews);
    }
  }, waitTillThisFriday);
});
```

### How is the promise resolved?

Note that this Promise constructor takes in one callback function (called executor), which takes in two arguments: fulfill and reject.

The executor function basically does some asynchronous task, and invoke either **fulfill** or **reject** based on the success/failure of that asynchronous task. When one of those callbacks is called, the promise is changed from **pending** state to either **fulfilled** state, or **rejected** state.

If there are any error thrown from the executor function, the promise is also **rejected** automatically.

### What is this fulfill and reject parameter?

Those two parameters are callbacks supplied by the Promise library. If you are interested, you can have a look at the source codes of Promise constructor to see how it creates the fulfill/reject callback and supplies it to the executor function.

Since these two callbacks are supplied by the Promise library, you don't need to implement these two functions, your job is to implement the executor function and call fulfill/reject at a proper time!

### Passing additional information when Promise is resolved/rejected

Note that we can pass some parameters to the fulfill callback. Those parameters would be available to those who subscribe to the success resolution event of the promise.

We can also pass the error to the reject callback, which is then available to those who subscribe to the error notification of the promise.

### The executor function should not take long to finish

Note that this Promise constructor calls the executor function right away (before the Promise constructor function returns). So typically you should not call any blocking API inside the executor function implementation. Otherwise, the Promise constructor call would be blocked.

## Observe the outcome of Promise

Now let's see how one can handle the outcome of a promise.

### Register your action plan with `then` and `catch` function

Once you get hold of an instance of Promise, you can register your interests on the outcome via the `then` function available on the promise object.

```js
const onFulfilled = ({restaurant, flower}) => {...}
const onRejected = (badNews) => {...}

dinnerPromise.then(onFulfilled, onRejected);
```

The then function takes two arguments:

- The first argument is called `onFulfilled`, which is a callback function to handle the case when the promise is resolved. Note that the onFulfilled function receives the same list of arguments as received by the fulfill function in executor function.

- The second argument is called `onRejected`, which is a callback function to handle the case when the promise is rejected. The main purpose of this callback function is to handle the error scenario and bring the system back to normal state. Note that the onRejected function receives the same list of arguments as received by the reject function in executor function.

#### Alternative syntax

Although the `then` function can take in the `onRejected` event handler, sometimes people prefer to call then function with one argument only. In that case, the reject event needs to be caught by a call to catch function. e.g.

```js
dinnerPromise.then(onFulfilled).catch(onRejected);
```

### Calls on Promise API can be chained

You may notice that in the code example above the calls on the promise instance are chained. We call `catch` function on the object returned from the `then` function. Why can we do it this way?

That's because all methods supported by this promise object returns another promise as return value. Specifically, those methods (e.g. `then` and `catch`) all behave in this way:

- If it finds a proper handler for the fulfill or reject event, it return a new promise which eventually resolves to the return value of the handler.
- If it does not find a proper handler for the fulfill or reject event (e.g. when a promise is rejected and a call to then function does not supply onRejected callback), the method would return the original promise.

### Treating a Promise as a gift box

Okay, this sounds pretty abstract. I have another mental model for you.
If you receive Promise object, you can think of a promise as a gift box.

It's wrapped nicely, and you don't know what's inside. All you can see is there are two LED lights on the surface:

- A green LED light
- A red LED light

![promise as a gift box](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LNZWCrS7GFDj-ThaD9z%2F-LNZWHSIaNYRUhsU59s9%2Fpromise-gift-box.png?generation=1538211005440007&alt=media)

When the promise is in pending state, neither LED light flashes.

When the promise is in **fulfilled** state, the green LED light flashes. There might be some value displayed on the panel, which reveals the secret hidden in the box.

When the promise is in **rejected** state, the red LED light flashes. There might be some value displayed on the panel, which shows some error has happened.

#### calling .then or .catch is like another gift box

When you call `.then` or `.catch` on the promise, you actually create another gift box like the picture shown below:

![another gift box](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LNZWCrS7GFDj-ThaD9z%2F-LNZWHSLuQQsXwHpU2js%2Fpromise-gift-box-chaining.png?generation=1538211005490233&alt=media)

What the picture above shows are:

When you call a then or catch method on a Promise, the function returns a new instance of Promise object (i.e. a new gift box is created).

On the newly created gift box, there are also two LED lights, a green one and a red one.

The green LED light of the second gift box would flash under one of the three conditions:

- If the green LED light of the first gift box flashes, and there is no `onFulfilled` handler in the second gift box, or
- If the green LED light of the first gift box flashes, and the `onFulfilled` handler run to completion with no problem, or
- If the red LED light of the first gift box flashes, and the `onRejected` handler run to completion with no problem.

You might wonder why the successful running of the `onRejected` handler lead to the green LED light of the second gift box to flash. Remember, the purpose of the `onRejected` handler is to **handle the error** and bring the system back to normal state. After the erratic behavior is detected and corrected by the `onRejected` handler, the error is fixed and the green LED light of the second box should flash.

The red LED light of the new box would flash under one of the three conditions:

- If the green LED light of the first gift box flashes, and the onFulfilled handler throws error during execution, or
- If the red LED light of the first gift box flashes, and there is no onRejected handler in the second gift box, or
- If the red LED light of the first gift box flashes, and the onRejected handler throws error during execution.

Can the onFulfilled and onRejected handles **throw error**? Yes!
I can give you two examples here:

- The onFulfilled handler may return another promise object, which is eventually rejected. In that case, this is considered as an error situation.
- The onRejected handler may encounter an error that it does not know how to handle, so it just throws it again and wishes another error handler can catch and handle it.

In either case, the red LED light of the second box would flash.

Note that the onRejected handler is optional when you call `then()`. If that is not specified, then the error reported by the first box cause the red LED light of the second box to flash right away.

Now you have a new gift box, you can pass it on to your friend, and similarly, they can create their own gift box by wrapping around your gift box, which result in a chain of gift boxes:

![chain of gift boxes](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LNZWCrS7GFDj-ThaD9z%2F-LNZWHSPaQmxKXWyWJ_t%2Fpromise-gift-box-chaining-2.png?generation=1538211005175237&alt=media)

(chain of gift boxes)

## Promise chaining example with promise, `then` and `catch`

Let's look at another Promise chaining example using ES6 Promise API.
In the example below, we try to retrieve the skills of a user. There are two asynchronous operations involved:

1. find users by calling the get() API, which returns a Promise.
2. get a user's meta data by calling getMetaDataFor(), which also returns a Promise.

Since the second asynchronous operation depends on the output of the first asynchronous operation, we can chain them up using the Promise API.

```js
function getUserSkills(userId) {
  return users
    .get(userId)
    .then((user) => {
      console.log(`Got user ${JSON.stringify(user)}`);
      return users.getMetaDataFor(user);
    })
    .then((userMetaData) => {
      console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
      return userMetaData.skills;
    })
    .catch((error) => console.error(error));
}
```

If you find the codes above hard to understand, here is another version that written in synchronous/blocking style which you may be more familiar with:

```js
function getUserSkills(userId) {
  try {
    const user = users.get(userId); // assuming get() API is a blocking/synchronous API
    const userMetaData = users.getMetaData(user); // assuming getMetaData is a blocking/synchronous API
    const skills = userMetaData.skills;
    return skills;
  } catch (error) {
    console.error(error);
  }
}
```

In this synchronous version, you call multiple functions and each function use the result of calling the previous function.
The asynchronous version using the Promises achieves the same effect, except that they are using callbacks and not blocking.

## Throwing errors from onFulfilled or onRejected handlers

If you ever call throw error yourself in the onFulfilled or onRejected handlers, or those handlers call some other function that throws errors, the call to `then` or `catch` would return a new promise that is rejected with the error received by the handler.

For example:

```js
function getUserSkills(userId) {
  return users
    .get(userId)
    .then((user) => {
      throw new Error("Cannot find meta data for user", JSON.stringify(user));
    })
    .then((userMetaData) => {
      console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
      return userMetaData.skills;
    })
    .catch((error) => console.error(error));
}
```

The first call to `then` function above would return a promise that is rejected with error. The second call to `then` does not provide any handler for rejection, so it's skipped and the event handler in `catch` would catch this error.

Note that this error is not thrown out of the getUserSkills function at all. It's handled by generating a console log. Then the error handler didn't return anything, so the caller of getUserSkills would get a Promise that's resolved to undefined.

## onFulfilled or onRejected handlers are always executed asynchronously

When an the Promise implementation needs to invoke a onFulfilled or onRejected handler, it will always execute the handler in the next tick, i.e. it will do something like

```js
setTimeout(handler, 0);
```

That means those handlers supplied to `then` or `catch` call (in the example above) would **NOT be directly pushed to the call stack** on top of the `then` or `catch` function.

Why? Because The ECMA 2015 spec declares that promises must not fire their resolution/rejection function on the same turn of the event loop that they are created on. This is very important because it eliminates the possibility of execution order varying and resulting in indeterminate outcomes.

In the example above, when `getUserSkills` function is executed (and the then/catch functions are executed), the `then/catch` function basically just registered the event handlers in the event table, and return immediately.

Later on, when an event handler needs to be executed (because the corresponding Promise is resolved/rejected), it will be put into the Event Queue and wait for its turn to run. (waiting for the event loop to push it back into the call stack).

### Why do you need to understand this?

With this knowledge, you can understand why you cannot use a try...catch block inside the `getUserSkills` function to handle the error thrown from the onFulfilled or onRejected handlers.

e.g. the codes below does not work, because when the error is thrown from the event handler, the `getUserSkills` function has already finishes its execution and is removed from the call stack.

```js
function getUserSkills(userId) {
  try {
    return users
      .get(userId)
      .then((user) => {
        throw new Error("Cannot find meta data for user", JSON.stringify(user));
      })
      .then((userMetaData) => {
        console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
        return userMetaData.skills;
      });
  } catch (error) {
    // this does not catch the error that the handler threw...
    console.log("Cannot find meta data for user..", error);
  }
}
```

## Another example with both callback, promise and then, catch

```js
function watchTutorialCallback(callback, errorCallback) {
  const userLeft = false;
  const userWatchingCatMeme = false;

  if (userLeft) {
    errorCallback({
      name: "User Left",
      message: ":(",
    });
  } else if (userWatchingCatMeme) {
    errorCallback({
      name: "User Watching Cat Meme",
      message: "WebDevSimplified < Cat",
    });
  } else {
    callback("Thumbs up and Subscribe");
  }
}

function watchTutorialPromise() {
  const userLeft = false;
  const userWatchingCatMeme = false;
  return new Promise((resolve, reject) => {
    if (userLeft) {
      reject({
        name: "User Left",
        message: ":(",
      });
    } else if (userWatchingCatMeme) {
      reject({
        name: "User Watching Cat Meme",
        message: "WebDevSimplified < Cat",
      });
    } else {
      resolve("Thumbs up and Subscribe");
    }
  });
}

watchTutorialCallback(
  (message) => {
    console.log(message);
  },
  (error) => {
    console.log(error.name + " " + error.message);
  }
);

watchTutorialPromise()
  .then((message) => {
    console.log(message);
  })
  .catch((error) => {
    console.log(error.name + " " + error.message);
  });
```

Example from https://codepen.io/WebDevSimplified

## Helper functions in Promise API

There are some handy utility functions on Promise API. You should check them out:

- Promise.resolve()
- Promise.all()

```js
const recordVideoOne = new Promise((resolve, reject) => {
  resolve("Video 1 Recorded");
});

const recordVideoTwo = new Promise((resolve, reject) => {
  resolve("Video 2 Recorded");
});

const recordVideoThree = new Promise((resolve, reject) => {
  resolve("Video 3 Recorded");
});

Promise.all([recordVideoOne, recordVideoTwo, recordVideoThree]).then(
  (messages) => {
    console.log(messages);
  }
);
```

Example from https://codepen.io/WebDevSimplified

## Practical use case study using fetch

Now, let's try to code-along with Promise API.

Many operations in JavaScript are asynchronous - for example: fetching data via HTTP, reading from a file, event handlers, etc.

In this example, we will use the **fetch API** provided by the browsers to fetch some data from a web service.

See fetch on [MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch).

The API we are going to call [display random pictures of dogs](https://dog.ceo/dog-api/).

Firstly visit https://dog.ceo/api/breeds/image/random on your browser to see what the HTTP request should return.

Then look at this piece of code below, what do you think the value of result will be? Is it a JSON string containing the dog image URL?

Run this in the Chrome Developer console. Did the console output match your expectation?

```js
const result = fetch("https://dog.ceo/api/breeds/image/random");
console.log(result);
```

In the above example, you should find that fetch returns a Promise, i.e. result is a Promise object.

If you read the documentation of the fetch() API, you can find that the returned value from the fetch API is "A Promise that resolves to a Response object."

How can we retrieve data out of this [Response object](https://developer.mozilla.org/en-US/docs/Web/API/Response)?

Again, based on the documentation of the Response, you can call the `response.json()` method to retrieve data sent in the response, assuming the data is in JSON format.

Let's try to call the .json method on the response object.

```js
fetch("https://dog.ceo/api/breeds/image/random").then((response) =>
  console.log(response.json())
);
```

Hmm...if you look at the console log, the value returned from the json() method is still a Promise. If you read the [response.json() documentation](https://developer.mozilla.org/en-US/docs/Web/API/Body/json) again, it says it returns "A promise that resolves with the result of parsing the body text as JSON."

OK, now we know that we need to chain the promise call together, like the codes below:

```js
fetch("https://dog.ceo/api/breeds/image/random")
  .then((response) => response.json())
  .then((json) => console.log(JSON.stringify(json)));
```

## Common mistakes

You will definitely makes some mistakes before you fully master the Promise API. Here are a few examples.

### Mistake: Unhandled Promise Rejections

One of the mistakes is you forget to handle errors that may be thrown from the promise.

If we remove the call to catch in the previous example:

```js
function getUserSkills(userId) {
  return users
    .get(userId)
    .then((user) => {
      console.log(`Got user ${JSON.stringify(user)}`);
      return users.getMetaDataFor(user);
    })
    .then((userMetaData) => {
      console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
      return userMetaData.skills;
    });
}
```

If there are any errors thrown from the database lookup, or the `getMetaDataFor` function, the promise would become rejected. Since we don't handle the rejection here, and assuming nobody else handle this error, then it becomes an [unhandled promise rejection](http://thecodebarbarian.com/unhandled-promise-rejections-in-node.js.html).

### Mistake: Handle Rejected Promises with try..catch

You probably get use to handle errors in JavaScript using try..catch syntax, and you would like to handle errors from promises using that syntax too. Unfortunately, that won't work.

See the example mentioned above.

```js
function getUserSkills(userId) {
  try {
    return users
      .get(userId)
      .then((user) => {
        throw new Error("Cannot find meta data for user", JSON.stringify(user));
      })
      .then((userMetaData) => {
        console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
        return userMetaData.skills;
      });
  } catch (error) {
    // this does not catch the error that the handler threw...
    console.log("Cannot find meta data for user..", error);
  }
}
```

That does not work because Promise implementation ensures the callbacks to then or catch is not executed in the current Call Stack (i.e. the callbacks are always put back to the Event Queue first and executed in later ticks).

This means the `getUserSkills` function would return before any callback handler passed to then or catch is called. The try...catch block here won't catch any error, and later when the error indeed happens, that error is thrown from the error handler function, and eventually becomes an unhandled promise rejection (the catch call returns a promise that's rejected, and nobody handles that rejected promise.)

### Mistake: Create a local instance of Promise and not return it

Look at the example below, what value would be returned from this function?

```js
function getUserSkills(userId) {
  users
    .get(userId)
    .then((user) => {
      console.log(`Got user ${JSON.stringify(user)}`);
      return users.getMetaDataFor(user);
    })
    .then((userMetaData) => {
      console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
      return userMetaData.skills;
    });
}
```

Although there are some return statements in the codes above, don't be confused by them. They are the return statements from the callbacks, and **not** the return statements for getUserSkills function.

If we miss the return statement before users.get.., the caller of this function would get **undefined** result.

Another example of forgetting returning the promise is in the example below:

```js
test("the fetch fails with an error", () => {
  expect(fetchData()).rejects.toMatch("error"); // we are missing return statement here if `fetchData` returns Promise
});
```

Assuming the `fetchData` function returns a Promise. This is a test case for some asynchronous function written in Jest. We need to use the return statement so that Jest knows this test calls an asynchronous function and it needs to wait for the promise to resolve before it ends the test function.

```js
test("the fetch fails with an error", () => {
  return expect(fetchData()).rejects.toMatch("error");
});
```

### Mistake: Forget to wait for a promise function to resolve

Sometimes, you may call a function that returns promise, but you forget about this fact and use it like a synchronous function (i.e. assuming once the function returns, all the task is done). This can leads to bugs that are hard to find.

For example:

```js
db.connect();
db.execute_statement(...);
```

Assuming the connect API returns a promise, and when that promise is resolved, you can start to execute statement with the database.
The code above does not work because when the execute_statement is called, the database connection may not be established yet.

So the correct way of using the API should be

```js
db.connect().then(() =>
const execution = db.execute_statement(...);
execution.then(...).catch(...)
);
```

### Mistake: Missing the return statement in onFulFilled or onRejected handlers

Remember the returned result from onFulfilled or onRejected handler is used as argument for the next handler in the promise chain, so if some handler forget to return anything, then the next handler would get undefined in their arguments.

For example:

```js
function getUserSkills(userId) {
  return users
    .get(userId)
    .then((user) => {
      console.log(`Got user ${JSON.stringify(user)}`);
      users.getMetaDataFor(user); // missing return statement here
    })
    .then((userMetaData) => {
      console.log(`Got metadata for user ${JSON.stringify(userMetaData)}`);
      userMetaData.skills; // missing return statement here
    })
    .catch((error) => console.error(error));
}
```

Note that the result from `getMetaDataFor` function is not returned, that means the next handler which is expecting userMataData as the argument would get **undefined** parameter value.

## How to convert API that only support callbacks to return a Promise instead

We see promises are useful tools to handle asynchronous tasks, but how can we deal with existing functions that are written in a way to take callback as arguments?

Node.js comes with a utility function called **promisify**, which takes a regular callback-taking function as an argument and returns its promisified version. e.g.

```js
const { promisify } = require("util");
const { readFile } = require("fs");
const readFilePromise = promisify(readFile);

// Original callback-style readFile
readFile("path/to/file", "utf8", (err, data) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(data);
});

// Promisified version
readFilePromise("path/to/file", "utf8")
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.error(error);
  });
```

util.promisify docs: https://nodejs.org/api/util.html#util_util_promisify_original
More information on how util.promisify works: http://2ality.com/2017/05/util-promisify.html

## Lab

Here is a useful workshop that illustrates the basics of promises. Follow the instructions step by step to get some hands-on exercises on Promise.
Promise it won't hurt

https://github.com/thoughtworks-jumpstart/promise-it-wont-hurt

Original workshop:
https://github.com/stevekane/promise-it-wont-hurt

Note:

- In the second step of the workshop, it requires you to install a library es6-promise and require it in your code. That's not necessary anymore because the latest Node version already have built in support for Promise.
- In the step on "Fetch JSON", the URL mentioned in the instruction should have been "http://localhost:1337"
