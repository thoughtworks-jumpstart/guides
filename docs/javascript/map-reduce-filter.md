# Map, reduce, filter

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/e-5obm1G_FY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

11:24 is important in the video.

## map

Without map:

```js
const result = [];
const numbers = [1, 2, 3];
for (const number of numbers) {
  result.push(number * 2);
}
console.log(result);
```

With map:

```js
const result = [1, 2, 3].map(element => {
  return element * 2;
});

console.log(result);
```

Another example:

```js
const numbers = [1, 2, 3];
const multiplyBy2 = number => {
  return number * 2;
};

const doubledNumbers = numbers.map(multiplyBy2);
console.log(doubledNumbers);
```

- .map() is syntactically similar to .forEach(). The key difference is the map returns an array, but forEach doesn't

- Like .forEach(), map takes in a callback function that can receive 3 positional parameters: element, index, container.

## reduce

.reduce()
.reduce() takes in a callback function that can take in 4 positional parameters: accumulator, current value, current index, source array.
The callback is function to execute on each element in the array (except for the first, if no initialValue is supplied).

It can also take in another parameter which is the accumulator's Initial Value. If no initialValue is supplied, the first element in the array will be used as the initial accumulator value and skipped as currentValue.

### Calculating sum of an array of integers

```js
function sum(numbers) {
  return numbers.reduce((acc, cur) => {
    return acc + cur;
  }, 0);
}
```

sum([1,2]) is 3

Another example:

```js
[1, 2, 3, 4, 5].reduce((accumulator, element, index) => {
  console.log(accumulator, element, index);
  return accumulator + element;
});

// prints:
// 1 2 1
// 3 3 2
// 6 4 3
// 10 5 4
// returns 15
```

### Reducing array of strings into one string

Not only can you accumulate numbers, you can also accumulate strings!

```js
const epic = ["a", "long", "time", "ago", "in a", "galaxy", "far far", "away"];
const result = epic.reduce(function(accumulator, element) {
  return accumulator + " " + element;
}); // returns 'a long time ago in a galaxy far far away'
```

If this is difficult to understand, log the values!

```js
const result = epic.reduce((accumulator, element) => {
  console.log(
    'logging -- element: "' +
      element +
      '", ' +
      'accumulator: "' +
      accumulator +
      '"'
  );
  return accumulator + " " + element;
});

// logging -- element: "long",    accumulator: "a"
// logging -- element: "time",    accumulator: "a long"
// logging -- element: "ago",     accumulator: "a long time"
// logging -- element: "in a",    accumulator: "a long time ago"
// logging -- element: "galaxy",  accumulator: "a long time ago in a"
// logging -- element: "far far", accumulator: "a long time ago in a galaxy"
// logging -- element: "away",    accumulator: "a long time ago in a galaxy far far"
// >> 'a long time ago in a galaxy far far away'
```

### Changing the structure of an object

Example: We want to change up the structure of our users so that we can use the users' full name as the key and have their email as the value.

Normally, this would take a lot of looping and initializing some variables. However, with reduce we can set an empty object as our starting point (i.e. previous) and do it all in a single go!

```js
const users = [
  { fullName: "George Washington", email: "george@us.gov" },
  { fullName: "John Adams", email: "john@us.gov" },
  { fullName: "Thomas Jefferson", email: "thomas@us.gov" },
  { fullName: "James Madison", email: "james@us.gov" },
];

const result = users.reduce((usersObj, user) => {
  usersObj[user.fullName] = user.email;
  return usersObj;
}, {});

// { 'George Washington': 'george@us.gov',
//   'John Adams': 'john@us.gov',
//   'Thomas Jefferson': 'thomas@us.gov',
//   'James Madison': 'james@us.gov' }
```

## filter

Like .forEach() and .map(), .filter() takes in a callback function that can take in 3 positional parameters: element, index, container.

However, unlike .forEach() and .map(), the callback function must return true or false. This callback is also known as predicate.

```js
const numbers = [1, 2, 3, 4, 5, 6];

const isEvenNumber = number => {
  return number % 2 == 0; // this returns true if the number has no remainder when divided by 2
};

const evenNumbers = numbers.filter(isEvenNumber);

console.log(evenNumbers);
// [2, 4, 6]

function isOddNumber(number) {
  return !isEvenNumber(number);
}

const oddNumbers = numbers.filter(isOddNumber);

console.log(oddNumbers);
// [1, 3, 5]
```

## Exercises

map: https://github.com/thoughtworks-jumpstart/javascript-basics/blob/solution/4-map.js

filter: https://github.com/thoughtworks-jumpstart/javascript-basics/blob/solution/5-filter.js

### Count words with reduce

Given an array of strings, use `reduce` to create an object that contains the number of times each string occured in the array. Return the object directly (no need to console.log).

Do not use any for/while loops or forEach.
Do not create any unnecessary functions e.g. helpers.

```js
const inputWords = ["Apple", "Banana", "Apple", "Durian", "Durian", "Durian"];

console.log(countWords(inputWords));
// =>
// {
//   Apple: 2,
//   Banana: 1,
//   Durian: 3
// }
```
