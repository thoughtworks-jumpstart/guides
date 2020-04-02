# Closure and currying

## Closure

Official definition on MDN:

> A closure is the combination of a function and the lexical environment
> within which that function was declared (e.g. another function, or a .js file).

Remember lexical environment?

The lexical environment consists of all the local variables in the function’s scope when the function is created. A closure enables one to refer to all local variables of a function where they were declared.

A closure is created by defining a function inside another function.

```js
function closuredFunc() {
  function closure() {
    // some code
  }
}
```

This function within a function is technically the closure. Each time the parent function is called, a new context of execution is created holding a fresh copy of all local variables.

Why use closures?

Before classes were introduced in JavaScript, common use case of closure is to create "private" variables.

Closures also allow us to use functions to create functions. This behaviour is known as **currying**.

## Currying

This name that sounds like food is related to [Haskell Curry](https://en.wikipedia.org/wiki/Haskell_Curry).

Curried functions are constructed by chaining closures by defining and immediately returning their inner functions.

```js
function createSayHi(name) {
  return function() {
    console.log(name + " say hi");
  };
}

const johnSayHi = createSayHi("john");
johnSayHi(); //the variable "name" is now considered as private and cannot be changed

const sallySayHi = createSayHi("sally");
//the variable "name" is alternatively set to "sally"

sallySayHi();
johnSayHi(); // calling the function multiple times does not change the value of "name"
sallySayHi();
```

This can be written with arrow functions too

```js
const createSayHi = name => () => {
  console.log(name + " say hi");
};
```

## Exercises

Create a function that returns a function that replaces every even-indexed letter with the letter given to the parent function.

```js
const replaceEvenCharactersWithLetter = letter => {
  return string => {
    return; // write the rest of your code here
  };
};

const replaceEvenCharactersWithW = replaceEvenCharactersWithLetter("w");

console.log(replaceEvenCharactersWithW("hello world~"));
// prints wewlw wowlw~
```

Currying is considered to be part of functional programming.
