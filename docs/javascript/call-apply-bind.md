# Call, apply, bind

.bind(), .apply() and .call() help us fix the problems when `this` loses its reference due to its dynamic scoping rules. These few functions allows us to **fix the value of this** before those functions are invoked.

## .call()

The .call() method allows us to execute a function with (i) a specific value of `this`, and (ii) the parameters to be passed into the function.

How to use it: `func.call(thisArg, arg1, arg2, ..., argn)`

```js
function greet(greeting, punctuation) {
  console.log(greeting, this.name, punctuation);
}

const dino = {
  name: "barney",
};

greet.call(dino, "howdy", "!!!!!!"); // prints 'howdy barney !!!!!!'
```

We passed the object (dino) as a first parameter to the call method, which means we passed it's context (`this`) too to the function `greet`. This way we have access to any method and property of this particular object (dino).

## .apply()

Now, .apply() serves the exact same purpose as .call() (to allow us to call a function with a specific object of our choice as this).

The only difference between how they work is that .call() expects all parameters to be passed in individually, whereas .apply() expects an **array of all of our parameters**.

How to use it: `func.apply(thisArg, [arg1, arg2, ..., argn])`

```js
function greet(greeting, punctuation) {
  console.log(greeting, this.name, punctuation);
}

var dino = {
  name: "barney",
};

greet.apply(dino, ["howdy", "!!!!!!"]); // prints 'howdy barney !!!!!!
greet.call(dino, "howdy", "!!!!!!"); // see the (small) difference between apply() and call()?
```

## .bind()

The .bind() method **creates a new function** that, when called, has its `this` keyword set to a specific object provided by us. In other words, the function's `this` value is permenently changed. Whereas for .call() and .apply(), it was changed once and not saved.

How to use it:

`const newFunction = someFunction.bind(someSpecificObject)`

Simple example:

```js
// problem:
const tom = {
  name: "tom",
  introduceSelf: function () {
    console.log("hi i am", this.name);
  },
  introduceSelfArrow: () => {
    console.log("hi i am", this.name);
  },
};

const introduceFunction = tom.introduceSelf;
introduceFunction(); // prints 'hi i am undefined'
```

If we ran `tom.introduceSelf()` it would have worked perfectly because the method is called from the object `tom` and so the `this` inside `introduceSelf` will have a reference to the caller object.

We cannot use arrow functions with `introduceSelf` if not `this.name` will not work as intended.

However when we assign the function to another variable and call the variable instead, the value of `this` passed to the function when it is invoked is the global scope's `this`.

We can solve the missing `this` reference by using `bind(obj)`.

```js
// solution:
const introduceFunction = tom.introduceSelf.bind(tom);
introduceFunction(); // prints 'hi i am tom'
```

This example is contrived, but when you encounter problems with this losing its reference (e.g. when you work with callback functions), think about using bind()!

Another example:

```js
var tom = {
  name: "tom",
  introduceSelf: (printIntroMessage) => {
    printIntroMessage();
  },
};

const introMessageSingapore = {
  print: function () {
    console.log(`Hi, i am ${this.name} from Singapore`);
  },
};

tom.introduceSelf(introMessageSingapore.print);

// solution
introduceFunction = introMessageSingapore.print.bind(tom);
tom.introduceSelf(introduceFunction); // prints 'hi i am tom'
```

## Arrow vs bind

Arrow functions VS bind
There’s a subtle difference between an arrow function => and a regular function called with .bind(this):

.bind(this) creates a “bound version” of the function.
The arrow => doesn’t create any binding. The function simply doesn’t have this. The lookup of this is made exactly the same way as a regular variable search: in the outer lexical environment.

We now use arrow functions instead of bind to have lexical scope. 

## Exercises

Go through the 4 tasks here: http://javascript.info/bind#bound-function-as-a-method .

## Further reading

https://dev.to/alexantoniades/call-apply-bind-the-basic-usages-5gpl

https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/5_applyCallBind.js
