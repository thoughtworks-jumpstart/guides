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

See https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/1_basics.js for more info.
