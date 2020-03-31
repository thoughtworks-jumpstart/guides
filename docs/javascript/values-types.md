# Values and Types

## Primitive Types

### Numbers (64 bits per number)

- 1
- 0.23847
- 10.4
- NaN
- Infinity

```
(1/0) // Infinity
(0/0) // NaN

typeof(Infinity) // "number"

Number.POSITIVE_INFINITY === Infinity // true
Number.NEGATIVE_INFINITY === -Infinity // true
```

### Strings (16 bits per string element)

- 'hi!'
- "this is another string"
- `` `this is a special string called a "template string". It can span across multiple lines` ``
- `` `half of 100 is ${100 / 2}. This can embed values.` ``
- "con" + "cat" + "e" + "nate"
- "This is the first line\nthis is the second line"
- "we are\\\non the same line because of escaped character \\"

### Booleans

- true
- false

### Others

- Symbol (introduced in ES6)
- null
- undefined

Primitive types can be combined together to create objects.

### Quiz

What is the output of the following?

```js
console.log("Abc" < "Zyx");
console.log("abc" < "Abc");
console.log(NaN == NaN);
```

## Automatic type conversion

```js
console.log(8 * null); // 0
console.log("5" - 1); // 4
console.log("5" + 1); // 51
```

JavaScript will sliently convert the types when an operator is applied to the "wrong" type.

This is **type coercion**.

## Working with numbers

- Arithmetic operators: +, -, /, \_, \*\*, %
- Math methods (e.g. Math.pow(2,2))
- Increment/decrement operators (++ and --)
- Operators with assignment: +=, -=, /=, \_=

## Working with strings

- Single and double quotes
- Template strings
- String properties (e.g. `"some string".length`)
- String methods (e.g. `"some string".toLowerCase()`, `"some string".toUpperCase()`)
  and much more (see [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String))!

## Declaring and initialization variables

![JavaScript variable lifecycle](https://scotch-res.cloudinary.com/image/upload/dpr_2,w_800,q_auto:good,f_auto/media/8976/bNTL1QI3RFebh7C1JPYC_variable%20hoisting.png)

Three types of declarations:

- var e.g. `var isSaturday = false`
- let e.g. `let age = 45`
- const e.g. `const name = 'Jack'`

A variable (or constant) contains a value, (e.g. "hello" or 42).
You use variables to store, retrieve, and manipulate values that appear in your code.
Variables can refer to other variables.

`let` and `const` were introduced in modern JavaScript to solve the problems that `var` had.
In modern JS, use `let` or `const` whenever possible and appropriate.

```js
const a = 1;
const b = a; // b is also 1

const x = 15;
const y = x + 20; // y is ???
```

## Naming rules and conventions

Try to give your variables meaningful names to make it easy for other people to understand what your code does.

- Names are case-sensitive
- Names cannot start with numbers
- Generally speaking, use only alphabets
- The name must not be a reserved keyword (e.g. var, for, if, while)
- See full list of reserved keywords here
- Use camelCase for names instead of snake_case or kebab-case

## Differences between var and let and const

### Scoping

While `var` is **function scoped**, `let` and `const` is **block scoped**. We shall elaborate more in the [scopes](./scopes) topic.

### Variable Hoisting

JavaScript treats all variable declarations using `var` as if they are declared at the top of a functional scope (if declared inside a function) or global scope (if declared outside of a function) regardless of where the actual declaration occurs. Only declarations are hoisted (or "lifted"), not initializations.

This is done by the JavaScript interpreter.

To understand what we mean by this, let's look at an example:

```js
console.log(name); // prints undefined
console.log(y); // throws ReferenceError: y is not defined
var name = "James";
```

This behaviour could be quite unintuitive.
This is equivalent to writing the code like this:

```js
var name;
console.log(name); // prints undefined
console.log(y); // throws ReferenceError: y is not defined
name = "James";
```

A best practice is to place all var statements near to the top of the function. This increases the clarity of the code.

Are `let` and `const` hoisted?
Yes. There are many conflicting answers of this online but let's check the reliable [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_Types#Variable_hoisting).

> In ECMAScript 2015, let and const are hoisted but not initialized. Referencing the variable in the block before the variable declaration results in a ReferenceError, because the variable is in a "temporal dead zone" from the start of the block until the declaration is processed.
> [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_Types#Variable_hoisting)

Example of `let` being "declared" but not initialized throwing **ReferenceError**.

```js
console.log(name); // prints undefined
console.log(foo); // throws ReferenceError
var name = "James";
let foo = 1;
```

### Temporal Dead Zone (optional)

Temporal Dead Zone is a period between entering scope and variable being declared, in which variable can not be accessed, for `let` and `const`.

The above behaviour of `var` is different from `let` and `const`.

```js
console.log(name); // prints undefined
console.log(foo); // throws ReferenceError
var name = "James";
let foo = 1;
```

We get a similar error as before when `foo` is not defined at all as a variable. Has `foo` been declared before the console log?

Test it out here in [a REPL](https://repl.it/@leeyh20/hoisting).

This seems as if `let` and `const` are not hoisted. But this is not true.

We can get a sense that `let` is hoisted due to the [following code](https://stackoverflow.com/questions/47589655/javascript-how-let-is-hoisted-or-not-inside-if-block):

```js
var a = 1;

if (true) {
  /* start of the Temporal Dead Zone */
  console.log(a); /* code in the Temporal Dead Zone */
  /* last line of the Temporal Dead Zone */
  let a = 2; /* end of the Temporal Dead Zone */
}

console.log(a);
```

See https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let for more information.
