# Some and Every

## Some

.some() works by taking a callback function that returns true or false. If any of the elements in the array return true, then the entire statement returns `true`.

Another way to think of some is that it checks that any value passes the conditional provided by the function.

```js
const array = [1, 2, 3, 4, 5];

const isEvenNumber = number => number % 2 === 0;

console.log(array.some(isEvenNumber)); //true
```

Another example:

```js
const ages = [23, 32, 17, 19, 34];

const lessThan21 = age => {
  return age < 21;
};

console.log(ages.some(lessThan21)); // true
```

## Every

.every() works by taking a function that returns true or false. If all of the elements in the array return true, only then will the entire statement return `true`.

```js
const ages = [23, 32, 17, 19, 34];

const twentyOneOrAbove = function(age) {
  return age >= 21;
};

ages.every(twentyOneOrAbove); // false
```
