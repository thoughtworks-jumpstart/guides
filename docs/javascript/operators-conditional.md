# Operators and conditional statements

## Comparison operators

- === and !== (vs. == and =)
- < and >
- <= and >=

### Type coercion

`===` will prevent unexpected type conversions. Using `==` might lead JavaScript to convert types to make the comparison work.

## Logical operators and combining conditions

- And: `&&`
- Or: `||`
- Not: `!`
- `!true` // returns false
- `!false` // returns true
- `!(1===1)` // returns false

You can also use !! to convert a variable into a boolean type. For example, !!'foo' will return true.

Let's look at an example:

```js
const names = ["Alice", "Bob"];
const isFound = names.find(name => name === "Charlie");
console.log(isFound); // returns undefined
```

In the code above, if a name is found then isFound will contain the name, otherwise it will be undefined.

```js
const names = ["Alice", "Bob"];
const isFound = !!names.find(name => name === "Charlie");
console.log(isFound); // returns false
```

A better way is to use type coercion to return a boolean value like in the code above.

### Unary Operators

`typeof` operator takes only one value.

```js
console.log(typeof 6.0); // number
console.log(typeof "x"); // string
```

The minus operator can be used both as a binary operator and as a unary operator.

```js
console.log(-(10 - 5));
```
