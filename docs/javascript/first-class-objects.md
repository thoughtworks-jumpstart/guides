# First-class objects

Functions are first-class objects, which means they can be:

- stored in a variable, object, or array
- passed as an argument to a function
- returned from a function

like any normal object

## Assigning a function to a variable

```js
// Declare
const anotherAwesomeFunction = function() {
  console.log("I am another awesome function!");
};

// Invoke
anotherAwesomeFunction();
```

Instead of `const`, you can also use `let` or `var`. But should you?

## Storing functions in object

Able to assign functions to variable means they can be stored anywhere!

```js
const Calculator = {
  add: function(a, b) {
    return a + b;
  },
  multiply: function(a, b) {
    return a * b;
  },
  divide: function(a, b) {
    return a / b;
  },
  subtract: function(a, b) {
    return a - b;
  },
};

assert(Calculator.add(2, 7) === 9, "should add 2 numbers");
assert(Calculator.multiply(2, 7) === 14, "should multiply 2 numbers");
assert(Calculator.divide(4, 2) === 2, "should divide 2 numbers");
assert(Calculator.subtract(2, 7) === -5, "should add 2 numbers");
```

## Storing functions in arrays

```js
const array = [];
array.push(function() {
  return "hello";
});

assert(array[0]() === "hello", "hello");
```

## Passing function as an argument

```js
const add = function(a, b) {
  return a + b;
};

const subtract = function(a, b) {
  return a - b;
};

const modifyNumbers = function(modifyingFunc, a, b) {
  return modifyingFunc(a, b);
};

console.log(modifyNumbers(add, 2, 1));
console.log(modifyNumbers(subtract, 2, 1));
```

## Return a function in another function

```js
function speak(text) {
  return function() {
    return "says " + text;
  };
}

const sayHi = speak("hi");
const sayMuahaha = speak("Muahaha");
assert(sayHi() === "says hi", "said hi");
assert(sayMuahaha() === "says Muahaha", "said Muahaha");
assert(speak("something")() === "says something", "said Muahaha");
```

## Exercises

https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/lab2.js

## Hoisting functions (optional)

### Function declarations

These are hoisted completely to the top. Now, we can understand why JavaScript seem to enable us to invoke a function before declaring it.

```js
hoisted(); // Output: "This function has been hoisted."

function hoisted() {
  console.log("This function has been hoisted.");
}
```

### Function expressions

These are not hoisted.

```js
expression(); // "TypeError: expression is not a function

var expression = function() {
  console.log("Will this work?");
};
```

### Combination

Let's try the combination of a function declaration and expression.

```js
expression(); // TypeError: expression is not a function

var expression = function hoisting() {
  console.log("Will this work?");
};
```

As we can see above, the variable declaration var expression is hoisted but it's assignment to a function is not. Therefore, the intepreter throws a TypeError since it sees expression as a variable and not a function.

Read [more at this tutorial](https://scotch.io/tutorials/understanding-hoisting-in-javascript).
