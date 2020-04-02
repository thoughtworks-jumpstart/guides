# Scopes

https://github.com/thoughtworks-jumpstart/learning-functions/blob/master/7_scopes.js

While `var` is **function scoped**, `let` and `const` is **block scoped**.
An example of block scoping can be seen in this example:

```js
if (true) {
  var name = "Luke"; // not block scoped
}

console.log(name); // returns Luke
```

```js
if (true) {
  let name = "Luke"; // block scoped
}

console.log(name); // returns ReferenceError
```

Another example of scoping from [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).

```js
function varTest() {
  var x = 1;
  {
    var x = 2; // same variable!
    console.log(x); // 2
  }
  console.log(x); // 2
}

function letTest() {
  let x = 1;
  {
    let x = 2; // different variable
    console.log(x); // 2
  }
  console.log(x); // 1
}
```

Another code example of scoping:

```js
var petSound = "Meow";

function makeSound(isHungry) {
  if (isHungry) {
    var petSound = "Meeeeeoww";
    return petSound;
  }
  return petSound;
}

makeSound(false); // undefined
```

Observe what happen when we replace `var` with `let`:

```js
let petSound = "Meow";

function makeSound(isHungry) {
  if (isHungry) {
    let petSound = "Meeeeeoww";
    return petSound;
  }
  return petSound;
}

makeSound(false); // returns `Meow`
```

Be careful when refactoring old JavaScript code that might only use `var`.

## More about Function scope with a function in function

```js
function getHawkerCentreFood(place) {
  const pricesAtPlaces = {
    honglim: [
      {
        name: "wanton mee",
        price: 5,
      },
      {
        name: "chicken rice",
        price: 4,
      },
    ],
    amoy: [
      {
        name: "hor fun",
        price: 6,
      },
      {
        name: "chicken rice",
        price: 3,
      },
    ],
  };
  const createPrettyPrices = () => {
    const dishes = pricesAtPlaces[place];
    let prices = "";
    dishes.forEach(dish => (prices += `${dish.name} cost $${dish.price}\n`));
    return prices;
  };

  return createPrettyPrices();
}

console.log(getHawkerCentreFood("honglim"));
```
