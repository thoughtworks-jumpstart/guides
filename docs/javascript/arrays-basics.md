# Arrays basics

An array is a data structure that allows us to store a collection of values in a single variable

# Syntax

## Defining an array

```js
const numbers = [1, 5, 10, 42];
```

## Accessing array element(s)

```js
numbers[0]; // returns 1
numbers[1]; // returns 5
numbers[2]; // returns 10
numbers[3]; // returns 42
numbers[4]; // returns undefined
```

## Adding array element(s)

```js
numbers.push(43);
```

Note: this mutates the state of the original array. We will learn another way of doing this in the second arrays chapter that does not mutate the original array

## Removing array elements

```js
numbers.slice(i, j);
```

i is the starting index
j (optional) is the ending index (non-inclusive of the element at index j). If excluded, the ending index is the length of the array

This method does not mutate the original array. Rather, it returns a new array

```js
const animals = ["ant", "bison", "camel", "duck", "elephant"];

animals.slice(2); // returns ["camel", "duck", "elephant"]

animals.slice(2, 4); // returns ["camel", "duck"]

animals.slice(1, 5); // returns ["bison", "camel", "duck", "elephant"]
```

What other in-built method for arrays can remove elements from the original array?

## Joining arrays to form strings

```js
someArray.join(someDelimiter);
```

```js
["hello", "who", "are", "you"].join("_"); // hello_who_are_you
["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"].join(""); // hello world
```

## concat

Merge two or more arrays together. See MDN docs!

```js
const cats = ["kitten", "tom"];
const dogs = ["bulldog", "chihuahua"];
const catsAndDogs = cats.concat(dogs);
console.log(catsAndDogs);
```

## Creating arrays from strings

```js
someString.split(someDelimiter);
```

```js
"hello world".split(" "); // ['hello', 'world']
"hello world".split(""); // ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
```

You can also create arrays from strings using Array.from()

```js
Array.from("hello world"); // returns ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
```

## sort

Sorting an array in JavaScript looks simple as

```js
const letters = ["e", "d", "a", "c", "b"];
letters.sort();
console.log(letters);
```

However, you will find that this does not work on numbers.

```js
const numbers = [5, 4, 3, 2, 1];
numbers.sort();
console.log(numbers); // not sorted!
```

We need to provide a comparison function as an argument to `sort` for it to work.

```js
const numbers = [5, 4, 3, 2, 1];
numbers.sort((a, b) => a - b);
```

We will learn more about arrow functions later.

## Exercises

- [Array Explorer](https://sdras.github.io/array-explorer/)
