# Objects

## Destructuring

Old way:

```js
const person = {
  firstName: "Wes",
  lastName: "Bos",
};
const first = person.firstName;
const last = person.lastName;
```

Instead of creating two variables, what if we can extract them in one line?

```js
const { firstName, lastName } = person;
```

This is not a block. This is not an object. It’s the destructuring syntax.

Give me a variable called `firstName`, a variable called `lastName`, and take it from the `person` object.

```js
const wes = {
  firstName: "Wes",
  lastName: "Bos",
  links: {
    social: {
      twitter: "https://twitter.com/wesbos",
      facebook: "https://facebook.com/wesbos.developer",
    },
  },
};
```

Old way:

```js
const twitter = wes.links.social.twitter;
const facebook = wes.links.social.facebook;
```

Destructure!

```js
const { twitter, facebook } = wes.links.social;
console.log(twitter, facebook);
```

Use it with other functions like map!

```js
var books = [
  {
    author: "Bill Gates",
    title: "The Road Ahead",
    isAvailable: true,
  },
  {
    author: "JRR Tolkkien",
    title: "Lord of the Rings",
    isAvailable: true,
  },
  {
    author: "JK Rowling",
    title: "Harry Potter",
    isAvailable: false,
  },
];

function listTitles(booksArray) {
  return booksArray.map(({ title }) => title);
}
```

## Spread

We have seen spread operator with arrays.

You can also use spread operator with objects.
In the example below, the spread operator is used to get all the fields of one object, before they are merged into a new object.

```js
const defaultConfiguration = {
  displayTab: true,
  displayMenu: true,
  displaySideBar: true,
};

const myEditorConfiguration = {
  ...defaultConfiguration,
  displaySideBar: false,
};

console.log(myEditorConfiguration);
```

## Object methods

For plain objects, the following methods are available:

Object.keys(obj) – returns an array of keys.
Object.values(obj) – returns an array of values.
Object.entries(obj) – returns an array of [key, value] pairs.

### Object.entries(obj)

Gives a 2d array of key-value pairs in an object.

```js
const object1 = {
  a: "somestring",
  b: 42,
};

console.log(Object.entries(object1));
// [ [ 'a', 'somestring' ], [ 'b', 42 ] ]
```

```js
for (let [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// expected output:
// "a: somestring"
// "b: 42"
// order is not guaranteed
```

See MDN docs! https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries

## Exercises

### Redo objects lab with destructuring, map, filter.

https://github.com/thoughtworks-jumpstart/javascript-basics/blob/master/3-objects.js

### Count the properties

```js
let user = {
  name: "John",
  age: 30,
};

console.log(countProperties(user)); // 2
```

Define a function `countProperties`.

### Sum the properties

```js
const salaries = {
  John: 100,
  Pete: 300,
  Mary: 250,
};

console.log(sumSalaries(salaries)); // 650
```

Define a function `sumSalaries`.
