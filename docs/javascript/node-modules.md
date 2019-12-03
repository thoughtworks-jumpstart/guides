# Node modules and packages

### Importing a module with `require`

The `require` function is provided by Node.js to import modules into a file.
It returns what the module is exporting.

Import a module named `math.js` into `calculate.js` file and put whatever the module is exporting into a variable named `math`.

```
// calculate.js
var math = require( './math.js' );
```

What is exported by `math.js` if it is an empty file? It will export an empty object `{}`.

```
console.log(math); // this will log {} to the console
```

### Export functions in a module with `module.exports`

Using the `exports` object you can export a function named `add` in the `math.js` module.
The `exports` object will be the object exported from the module.

```
// math.js
exports.add = function(number1, number2) {
  return number1 + number2;
};
```

The `math` variable in `calculate.js` will now be an object that contains the `add` key which has the function as a value.

```
{ add: [Function] }
```

You can now execute the function in calculate.js.

```
// calculate.js
var sum = math.add(1, 2); //sum is 3
```

How do you export a function but not export it inside an object? You can use the `module` object to do this.
It contains information about the module and also a key called `exports`.

The `exports` object you youre using previously points to exports key on the module object.

Exporting a function as `exports.add` is the same as exporting it as `module.exports.add`.

```
// math.js
console.log(exports === module.exports); // true
console.log(exports.add === module.exports.add); // true
```

Thus you can export the the same function in `math.js` like this:

```
// math.js
module.exports = function(number1, number2) {
  return number1 + number2;
};
```

The `math.js` module can now be used in `calculate.js` like this:

```
// calculate.js
var math = require("./math.js");
console.log(math); // [Function]
var sum = math(1, 2);
console.log(sum); // 3
```

### Packages in Node.js

If you have more modules like `math.js`, you might want to put them all into a folder called `lib` and import them. However, if our `calculate.js` is nested in a folder named calculate (`calculate/calculate.js`), then for you to import `math.js` using a different relative path (`../lib/math.js`).

```
├── lib/
|  └── math.js
└── calculate/
   ├── calculate.js
```

```
// calculate.js
var math = require("../lib/math.js");
```

The relative paths eventually becomes very difficult to determine as files get more nested.
