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

You can also use [double-not operator](https://repl.it/@MabelLee/DoubleNotOperator) `!!` to convert a variable into a boolean type. For example, !!'foo' will return `true`.

Example:

```js
const isFound = undefined;
console.log(isFound); // returns undefined
```

In the code above, if a name is found then isFound will contain the name, otherwise it will be undefined.

```js
const isFound = !!undefined;
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

### If conditional statements

```js
if (someCondition === "a") {
  // code to run if someCondition === "a"
} else if (someCondition === "b") {
  // code to run if someCondition === "b"
} else {
  // otherwise, run some other code instead
}
```

Another example:

```js
const food = "pizza"; // change this value and see the different log messages

if (food === "pizza") {
  console.log("yummy!");
} else if (food === "fried chicken") {
  console.log("even yummier!");
} else if (food === "broccoli") {
  console.log("ewww!");
} else {
  console.log("I don't know what to feel about this");
}
```

### While

### Switch case statements

```js
switch (fruit.toLowerCase()) {
  case "oranges":
    console.log("Oranges are $0.59 a kilo.");
    break;
  case "mangoes":
  case "papayas":
    console.log("Mangoes and papayas are $2.79 a kilo.");
    break;
  default:
    console.log("Sorry, we do not sell " + fruit.toLowerCase() + ".");
}
```

### Ternary conditional statements

The dictionary definition of tern is a "set of three". A ternary condition follows the following syntax: `condition ? doSomethingIfTrue() : doSomethingIfFalse()`

```js
skyColor === "grey" ? bringRaincoat() : bringPicnicMat();
laptop === "macbook" ? setupFaster() : struggleWithWindows();
```
