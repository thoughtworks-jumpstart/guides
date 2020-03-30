# JavaScript basics

## History

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/Sh6lK57Cuk4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Brandan Eich created JavaScript in 10 days in May 1995, under duress and conflicting management imperatives — “make it look like Java,” “make it easy for beginners,” “make it control almost everything in the Netscape browser.”

Now, JavaScript has become the language of the web. To date, JavaScript is the only programming language with built-in support in all major web browsers for client-side application scripting. Moreover, in recent years, JavaScript has become a popular language for implementing server-side applications with the advent of the Node.js platform. In the last three years, JavaScript has changed drastically.

JavaScript has its idiosyncracies and gotchas, which we will discover over the course of working with it. But it can also be a very powerful tool in our toolkit, if we understand it and use it well.
Here is a good overview of the JavaScript landscape as of 2018:
[Modern JavaScript Explained For Dinosaurs](https://medium.com/the-node-js-collection/modern-javascript-explained-for-dinosaurs-f695e9747b70)

## ES6

In this course, we will be teaching modern JavaScript. You might come across the term `ES6` when we talk about newer JavaScript features.
ES6 means ECMAScript 6, also known as ECMAScript 2015 or ES6, is the version of the ECMAScript standard defined in year 2015. ES6 is a significant update to the JavaScript language.

Are you confused by the terms like "ES6", "ECMAScript 2015"? Are they the same thing as "JavaScript"? Read [this article for more details](https://www.freecodecamp.org/news/whats-the-difference-between-javascript-and-ecmascript-cba48c73a2b5/).

## Printing output

```js
console.log("hello world");
console.log(42);
```

## Comments

Single line:

```js
// this will not be executed
```

Multiline:

```js
/* this is a comment
that spans across multiple lines */
```

Shortcut: ⌘/ (Mac) or Ctrl + / (Windows)

## Semicolons?

- They are optional!
- Semicolons are inferred, but only before a }, at the end of a line, or at the
  end of a program.
- Never omit a semicolon before a statement beginning with (, [, +, -, or /.
- As a convention in our class, we will use the 'prettier' style guide, which
  includes semi-colons by default.

## What are the basics to learn?

- Values and Types
- Operators and Conditional Statements
- Functions
- Objects
- Arrays
- Array methods
- Loops
- Errors

## JavaScript toolchain

Commonly used, modern tools when we code in JavaScript:

- node and npm
- linting with eslint and prettier
- webpack
- babel

## Exercises

If you are new to JavaScript, you can try the following simple library (just the first exercise):
https://github.com/thoughtworks-jumpstart/js-basics-1
