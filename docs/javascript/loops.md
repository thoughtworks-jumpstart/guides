# Loops

There are many ways to loop through an array or object.

## for loop

Syntax:

```js
myArray = ["alice", "tim", "bob", "june"];
for (startingValueOfI; endingCondition; incrementStep) {
  // do something with myArray[i]
}
```

Example:

```js
someArray = ["alice", "tim", "bob", "june"];
for (var i = 0; i < someArray.length; i++) {
  console.log("Hi " + someArray[i] + "!");
}
```

## while loop

`n < 3` returns true or false.
When it returns true, it does something.
`n` is incremented inside the while loop.

```js
var n = 0;
while (n < 3) {
  console.log("i is " + i);
  n++;
}
```

## for...of statement

Use `for...of` to iterate over the values in an iterable, like an array for example:

```js
for (variable of iterable) {
  statement;
}
```

```js
const numbers = [10, 20, 30];
for (const number of numbers) {
  console.log(value);
}
```

Strings are also an iterable type, so you can use for...of on strings:

```js
const str = "abcde";

for (const char of str) {
  console.log(char.toUpperCase().repeat(3));
}
```

### for...in statement

Use for...in to iterate over the object keys:

```js
for (key in object) {
  // do something with key
}
```

```js
const car = {
  wheels: 4,
  doors: 2,
  seats: 5,
};

for (const thing in car) {
  console.log(`my car has ${car[thing]} ${thing}`);
}
```

You can also use for...in to iterate over the index values of an iterable like an array or a string:

```js
const str = "Turn the page";

for (const index in str) {
  console.log(`Index of ${str[index]}: ${index}`);
}
```

Make sure you remember the difference between `for...of` and `for...in`!
