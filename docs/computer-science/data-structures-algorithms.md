# Data structures and algorithms

## Data structures

> The difference between a bad programmer and a good one is whether he considers his code or his data structures more important. Bad programmers worry about the code. Good programmers worry about data structures and their relationships.

-- Linus Torvalds, creator of Linux and Git

## Algorithms

> An algorithm must be seen to be believed.

-- Donald Knuth, author of _The Art of Computer Programming_

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/WaNLJf8xzC4?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/kPRA0W1kECg?controls=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Asymptotic analysis

### Quiz

#### One loop

Problem: Does array `A` contain the specific `value`?

Question: What is the running time of the following algorithm?

```js
for (let i = 0; i < A.length; i++) {
  if (A[i] === value) {
    return true;
  }
}
return false;
```

#### Two loops

Problem: Do either arrays `A` or `B` contain the specific `value`?

Question: What is the running time of the following algorithm?

```js
for (let i = 0; i < A.length; i++) {
  if (A[i] === value) {
    return true;
  }
}

for (let i = 0; i < B.length; i++) {
  if (B[i] === value) {
    return true;
  }
}

return false;
```

#### Two nested loops

Problem: Do arrays `A` or `B` contain a common value?

Question: What is the running time of the following algorithm?

```js
for (let i = 0; i < A.length; i++) {
  for (let j = 0; j < B.length; j++) {
    if (A[i] === B[j]) {
      return true;
    }
  }
}

return false;
```
