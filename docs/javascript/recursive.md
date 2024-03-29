# Recursive

Recursive function is a function that calls itself.

Three parts to a recursive solution:

1. Base case (you know these results)
2. Incrementally change something and move towards the base case
3. Calling itself recursively at the return statement with a smaller problem

Simple Example:

```js
countdown = (num) => {
  if (num < 0) {
    return;
  }
  console.log(`counting down ${num}`);
  return countdown(num - 1);
};

countdown(10);
```

The base case is `num < 0` where it will stop calling itself.
Moving towards the base case by changing the argument passed to `num - 1`.

## Fibonacci sequence

Another example:

Let's see the Fibonacci sequence

```js
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...
```

The pattern is such that the next number is the sum of the two preious numbers.

```js
F(n) = F(n - 1) + F(n - 2);
```

Let's write an iterative version

```js
function fibonacci(num) {
  let first = 0;
  let second = 1;

  while (num >= 0) {
    const temp = second;
    second = second + first;
    first = temp;
    num--;
  }
  return first;
}
```

fib(10) gives you 89.

What's the corresponding recursive version?

This is converting the while loop to a base case by inverting the while loop condition.

```js
function fibonacci(num) {
  if (num < 0) {
    return 1;
  }

  return fibonacci(num - 1) + fibonacci(num - 2);
}
```

However, you see that it becomes slightly different.
fib(10) gives you 233 instead of 89.

This is because we only used one base case which is the value of `second` in the previous example.

```js
function fibonacci(num) {
  if (num < 0) {
    return 0;
  }

  return fibonacci(num - 1) + fibonacci(num - 2);
}
```

Returning zero does not work as adding zero to zero will always give you zero. The function will only be able to return zero.

Considering we start with two variables and have two variables `first` and `second` that change, we can consider having two base cases.

```js
function fibonacci(pos) {
  if (pos === 0) {
    return 0;
  }
  if (pos === 1) {
    return 1;
  }
  return fibonacci(pos - 1) + fibonacci(pos - 2);
}
```

The recursive solution translates from the mathematical recurrence relation very nicely.

However, this is not very efficient.

![recursive fibonacci tree](https://miro.medium.com/max/1400/1*svQ784qk1hvBE3iz7VGGgQ.jpeg)

Picture from:
https://miro.medium.com/max/1400/1*svQ784qk1hvBE3iz7VGGgQ.jpeg

There are many repeated problems being solved.

To improve this efficiency, we can use a technique in dynamic programming called memoization. However, this is another concept and we will not elaborate here.

## Exercises

1. A palindrome is a string of numbers / characters that when flipped around, it will still form the same string. In other words, reading it forwards and backwards is the same.

Some examples are Anna, Civic, Kayak, Level, Madam, Mom, Noon, Racecar, Radar, Redder, Refer.

Try to solve this recursively.

You can consider using string methods such as `substr()` or `substring()`. (Which are two different string methods in JavaScript!)

```js
const assert = require("assert");
function isPalindrome(string) {
  // fill in your code here recursive solution
  return false;
}
assert(isPalindrome("Anna") === true);
```

2. Count the number of keys in a nested object recursively!
   Hint: Use `typeof`? How to differentiate an array from a literal object?

It is possible to use a loop or use `reduce`.

Example of a nested object to count the keys of:

```js
const bulbasaur = {
  id: 1,
  name: {
    english: "Bulbasaur",
    japanese: "フシギダネ",
    chinese: "妙蛙种子",
  },
  type: ["Grass", "Poison"],
  parent: {
    name: {
      english: "Ivysaur",
      japanese: "フシギソウ",
      chinese: "妙蛙草",
    },
  },
};
```

This object has 11 keys.
