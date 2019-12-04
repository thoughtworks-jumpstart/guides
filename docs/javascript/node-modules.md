# Node modules and packages

## Importing modules in scripts

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

## Packages in node_modules

### Why use node_modules?

Relative paths are confusing.

If you have more modules like `math.js`, you might want to put them all into a folder called `lib` and import them. However, if our `calculate.js` is nested in a folder named calculate (`calculate/calculate.js`), then for you to import `math.js`, you would have to use a different relative path (`../lib/math.js`).

Folder structure:

```
├── lib/
|  └── math.js
└── calculate/
   └── calculate.js
```

Importing `math.js` module in `calculate.js`:

```
// calculate.js
var math = require("../lib/math");
```

The relative paths eventually becomes very difficult to determine as files get more nested.

To solve this problem, Node.js can import modules for us without relative paths if they are placed in a folder called `node_modules`. Modules in this folder are called _packages_.

### Using node_modules

For example, if the `lib` folder is placed inside `node_modules`, it will become a package that you can import in `calculate.js`.

Folder structure:

```
├── node_modules/
|  └── lib/
|      └── math.js
└── calculate/
   └── calculate.js
```

Now you can import `math.js` file from the `lib` package.

```
// calculate.js
var math = require("lib/math");
```

Note that if you use `require("lib")`, Node.js will try to search for a index.js file inside the lib package directory.

Package management gets tough when we add more packages, especially those that are 3rd party and open-source. This is why we will use a package manager such as `npm` to manage and update our packages.
