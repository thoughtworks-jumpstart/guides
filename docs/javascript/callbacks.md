# Callbacks

We have been been using callbacks with many of the built-in functions like map, reduce and filter.

Let's understand what actually are these callback functions.

## What is it?

What is a callback function? How does it help us?

Analogy:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/-mir74x3f5Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Basics:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/haz4SBcEYAw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Callback functions are just calling another function within when the task is done. It is probably called **callback** because it is like you asking the function to start a certain task and then "make a call back" to the callback function we are giving it.

## Examples

```js
function sleepInSeptember(wakeMeUpWhenSeptemberEnds) {
  console.log("sleeping in september");
  console.log("will call wakeMeUpWhenSeptemberEnds later");
  wakeMeUpWhenSeptemberEnds();
}
```

We can pass in a callback function:

```js
sleepInSeptember(function() {
  console.log("wake up");
});
```

We can also pass in an arrow function:

```js
sleepInSeptember(() => console.log("wake up"));
```

## setTimeout and setInterval

More realistic use of callbacks which are commonly used with events and api calls:

We may decide to execute a function not right now, but at a certain time later. We are scheduling a call for later.

```js
console.log("at 0 seconds");
setTimeout(() => {
  console.log("Hello 1 second later");
}, 1000);
```

```js
console.log("at 0 seconds");
setInterval(() => {
  console.log("Hello every 1 second");
}, 1000);
```

## Exercises

Let's code our own Map and Filter functions to understand callbacks:

```js
const assert = require("assert");
const myMap = (array, callback) => {
  // fill in your code
};

const myFilter = (array, callback) => {
  // fill in your code
};

assert.deepEqual(
  myMap([1, 2, 3, 4], (element, index) => element * 2) === [2, 4, 6, 8]
);

assert.deepEqual(
  myFilter([1, 2, 3, 4], (element, index) => element < 3) === [1, 2]
);
```

where the callback should be expected to have two parameters, element and index.

Do not use the original map or filter function. You can use loops.

Your functions should return a new array.
