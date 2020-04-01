# Arrays

## Destructuring (rest operator with arrays)

```js
const sortedScores = [100, 99, 95, 80, 70, 55];
const [firstScore, secondScore, ...remainingScores] = sortedScores;
console.log(remainingScores); // prints [95, 80, 70, 55]
```

## Spread operator with arrays

Looks very similar to what we learnt before as rest parameters. However, instead of "collating the parameters", it spreads the contents of any iterable (e.g. arrays and objects). It can be conveniently used for joining arrays or inserting elements into arrays.

```js
// two simple examples
console.log(...[1, 2, 3]);
console.log(...["a", "b", "c"]);

// spread operators can be used to spread and concatenate arrays
const gentlePets = ["puppy", "rabbit", "chicken"];
const aggressivePets = ["dragon", "cobra", "tiger"];

const allPets = [...gentlePets, "kitten", ...aggressivePets];

for (p of gentlePets) {
  console.log(p);
}
```

You can even use spread with functions like the `Math.min` method:

```js
const numbers = [3, 10, 2];
const min = Math.min(...numbers);
console.log(min);
```

## find

find function returns the value of the first element in the array that satisfies the provided testing function.

```js
const numbers = [10, 20, 30, 40, 50];
const found = numbers.find(element => element > 20);
console.log(found);
```

```js
const names = ["Alice", "Bob"];
const isFound = !!names.find(name => name === "Charlie");
console.log(isFound); // returns false
```

## forEach

The .forEach() method executes a provided function once for each array element. Check MDN docs!

Syntax:

```js
someArray.forEach(function(element[, index, array]) {

})
```

Old `for loop`:

```js
const friends = ["bob", "alice", "tim", "june"];

// using the old for loop
for (let i = 0; i < friends.length; i++) {
  console.log("Hello, " + friends[i] + "!");
}
```

using forEach

```js
friends.forEach(friend => {
  console.log("Hi " + friend + "!");
});
```

Another example of forEach

```js
[1, 2, 3].forEach(function(element, index, container) {
  console.log("----");
  console.log("element value : " + element);
  console.log("index number  : " + index);
  console.log("container     : " + container);
});

// ----
// element value : 1
// index number  : 0
// container     : 1,2,3
// ----
// element value : 2
// index number  : 1
// container     : 1,2,3
// ----
// element value : 3
// index number  : 2
// container     : 1,2,3
```

- The callback function can either be an anonymous or named function (like all callbacks).
- This function can receive 3 positional parameters: element, index, container. The position matters, so the first variable name that you pass into this callback will always refer to element, no matter what you call it.
- You can omit parameters from the parameter list if you are not using them
- .forEach() is just like .map(), except .forEach() always returns `undefined`, map() returns a new array.

## Exercises

https://github.com/thoughtworks-jumpstart/javascript-basics/blob/solution/2-arrays.js
