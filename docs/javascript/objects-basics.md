# Objects

Objects offer us a way to store data in a rich way - in a way that numbers/strings/booleans/arrays cannot. Objects allow us to model Real World Things (e.g. a user, a cat, a car) in our applications.

Objects can store 2 types of things: data (i.e. values - like strings, numbers, booleans, arrays) and behaviour (functions).

## Syntax

### How to define an object using object-literal in JavaScript

```js
const emptyObject = {};

const myObjectWithTwoKeys = {
  myKey: "someValue", // this is known as a key-value pair
  myOtherKey: "someOtherValue", // this is another key-value pair
};
```

### Getting values by keys

```js
// using dot notation
myObjectWithTwoKeys.myKey; // returns 'someValue'

// using square bracket notation
myObjectWithTwoKeys["myKey"]; // returns 'someValue'
```

Dot notation will NOT work if you are using a variable as a key. You will have to use the square bracket notation if you want to use variables as a key.

```js
const key = "myKey";
myObjectWithTwoKeys[key]; // returns 'someValue'
```

Defining object methods

```js
const drink = {
  name: "Teh C",
  type: "milk tea",

  printMessage() {
    console.log("wow, you can define functions in objects! (a.k.a. methods)");
  },
};

// Invoke a method
drink.printMessage();
```

Use the `this` keyword to refer to the object (e.g. this.myKey)

```js
const drink = {
  name: "Teh C",
  type: "milk tea",

  printType: function() {
    // alternative syntax for defining methods
    console.log(this.type); // milk tea
  },
};

// Invoke
drink.printType();
```

Setting values by keys (after object has been created)

```js
const cat = {};

// Add a new property
cat.name = "Fluffy";

// Add a new method
myObject.awesomeMethod = function() {
  console.log("i'm an awesome method!");
};
```

## Exercises

https://github.com/thoughtworks-jumpstart/javascript-basics/blob/master/3-objects.js
