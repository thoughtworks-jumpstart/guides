# Functions

Functions allow us to keep our code **DRY (Don't Repeat Yourself)** by allowing us to group repeated logic into reusable and readable pieces of code.

We have already seen examples of built-in functions such as `console.log(message)`. We can create our own functions too.

## Function declaration

Syntax:

```js
function name(parameters) {
  ...function body...
}
```

Example:

```js
function showMessage() {
  console.log("Hello there, human");
}
```

### Functions can return values

```js
function createMessage() {
  return "Hello there, human";
}
```

Default return value is `undefined` if not specified.

```js
function sayHello() {
  console.log("hello");
}

console.log(sayHello()); //undefined
```

### Functions can have parameters

```js
function createMessage(name) {
  return `Hello there, ${name}`;
}
```

## Invocation

Without functions:

```js
console.log("Hello there, Amanda");
console.log("Hello there, Bob");
console.log("Hello there, Caroline");
```

With functions:

```js
console.log(createMessage("Amanda")); // Hello there, Amanda
console.log(createMessage("Bob")); // Hello there, Bob
console.log(createMessage("Caroline")); // Hello there, Caroline
```

We can call the function `createMessage(name)` we have created.

Create once, invoke many times!

## Default parameters

```js
function increment(origNumber, add = 0) {
  const newNumber = origNumber + add;
  return newNumber;
}

console.log(`nothing added to one: ${increment(1)}`); // 1
console.log(`adding one to one: ${increment(1, 1)}`); // 2
```

Extra parameters will be ignored.

## Rest parameters `...`

Rest parameters `...` allow you to pass in more arguments than the number of parameters by gathering all the arguments.

```js
function sum(...rest) {
  console.log(rest); // [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
  let total = 0;
  rest.forEach(item => (total += item));
  return total;
}

console.log(sum(0, 1, 2, 3, 4, 5, 6, 7, 8, 9) === 45); // true
```

Rest parameters `...` must be at the end when there are other parameters.

```js
function showName(firstName, lastName, ...nicknames) {
  console.log(firstName + " " + lastName); // Kopi Lim
  console.log(nicknames[0]); // Boss
  console.log(nicknames[1]); // Kopi C
  console.log(nicknames.length); // 2
}

showName("Kopi", "Lim", "Boss", "Kopi C");
```

## IIFE, Immediate Invoke Function Expression

```js
(function() {
  console.log("this runs immediately");
})();

(function() {
  console.log("this runs immediately too!!");
})();
```

After the function is defined, it runs once and can never be called again.

### Why might we (not) need IIFE?

Normally, functions and variables added to the global scope are available to all scripts that are loaded on a page.

We can use IIFEs to create "private" variables and functions that only can be accessed in our defined function.

```js
(function initGame() {
  // "Private" variables that no one has access to outside this IIFE
  var lives;
  var weapons;

  init();

  // "Private" function that no one has access to outside this IIFE
  function init() {
    lives = 5;
    weapons = 10;
  }
})();
```

However, in modern JavaScript, we have `let` and `const` now which gives us block scope. We can use block scope to now replace IIFE.

```js
{
  const initGame = function() {
    const lives;
    const weapons;

    const init = function() {
      lives = 5;
      weapons = 10;
    }
    init();
  }
}
```

However, knowing about IIFE gives us an understanding of the importance of scope and closure and will be especially useful in maintaining legacy JavaScript code.

See https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/1_basics.js for more info.

## Exercises

- Define a function, invertCase(someString), that returns the input string with its case inverted

```js
assert(invertCase("Hello") === "hELLO");
assert(invertCase("hELLO wORLD") === "Hello World");
```

- https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/lab1.js
