# Node.js modules and packages

Refer to the github repository: [Create simple Node.js modules with no server](https://github.com/thoughtworks-jumpstart/simple-node-modules)

## Import modules in scripts

### Import a module with require

The `require` function is provided by Node.js to import modules into a file.
It returns what the module is exporting.

First, we have two files, `math.js` and `calculate.js`.

Import a module named `math.js` into `calculate.js` file and put whatever the module is exporting into a variable named `math`.

```js
// calculate.js
var math = require("./math.js");
```

It is a loose convention to give the variable the same name as what you are importing but this is not enforced.

What is exported by `math.js` if it is an empty file? It will export an empty object `{}`.

```js
console.log(math); // this will log {} to the console
```

### Export functions in a module with module.exports

Using the `exports` object you can export a function named `add` in the `math.js` module.
The `exports` object will be the object exported from the module.

```js
// math.js
exports.add = function (number1, number2) {
  return number1 + number2;
};
```

The `math` variable in `calculate.js` will now be an object that contains the `add` key which has the function as a value.

```js
{
  add: [Function];
}
```

You can now execute the function in calculate.js.

```js
// calculate.js
var sum = math.add(1, 2); //sum is 3
```

How do you export a function but not export it inside an object? You can use the `module` object to do this.
It contains information about the module and also a key called `exports`.

The `exports` object you youre using previously points to exports key on the `module` object.

Exporting a function as `exports.add` is the same as exporting it as `module.exports.add`.

```js
// math.js
console.log(exports === module.exports); // true
console.log(exports.add === module.exports.add); // true
```

Thus you can export the the same function in `math.js` like this:

```js
// math.js
module.exports = function (number1, number2) {
  return number1 + number2;
};
```

It can also be written like this:

```js
// math.js
function add(number1, number2) {
  return number1 + number2;
}

module.exports = add;
```

The `math.js` module can now be used in `calculate.js` like this:

```js
// calculate.js
var math = require("./math.js");
console.log(math); // [Function]
var sum = math(1, 2);
console.log(sum); // 3
```

What is the `module.exports` object?

Referring back to what we have learnt about the "global execution context" and arrow functions, `module.exports` in a module was explained to be the **global object** that we are referring to when we use `this`.

```js
const assert = require("assert");
console.log(module.exports === this);

this.hello = "hello";
console.log(this.hello === "hello");
console.log(module.exports.hello === "hello");
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

```js
// calculate.js
var math = require("../lib/math");
```

The relative paths eventually becomes very difficult to determine as files get more nested.

To solve this problem, Node.js can import modules for us without relative paths if they are placed in a folder called `node_modules`. This is an unfortunate name because these are actually not just normal modules anymore. Modules in this folder are called _packages_.

### How to import packages in node_modules

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

```js
// calculate.js
var math = require("lib/math");
```

Note that if you use `require("lib")`, Node.js will try to search for an **index.js** file inside the lib package directory.

Package management gets tough when we add more packages, especially those that are 3rd party and open-source. This is why we will use a package manager such as `npm` to manage and update our packages.

Normally we will not commit nor push the folder `node_modules` to our git repository.

## Node.js Core Modules

Node.js comes with core modules (built-in packages) provided as the Node Standard Library. Being built-in packages, you do not need to install them to import them.
Their sources are on [Github](https://github.com/nodejs/node/tree/master/lib).

The main (not all!) core modules are:

- [http](http://nodejs.org/api/http.html#http_http): HTTP clients and servers
- [util]http://nodejs.org/api/util.html): Utilities
- [querystring](http://nodejs.org/api/querystring.html): Parses query-string formatted data
- [url](http://nodejs.org/api/url.html): Parses URL data
- [fs](http://nodejs.org/api/fs.html): Writing and reading files

When we require(<packagename>) a package, Node.js first searches for the package name in the built-in packages. If it doesn’t find it in the standard library, it then searches for it in node_modules.

For example, to import the `fs` built-in package for reading files:

```js
var fs = require("fs");
```

## Commonly used Node.js libraries

### Nodemon

When a change is in made in the project files, you normally would need to restart the Node.js server to see the change.

Nodemon is a library that will make Node.js server automatically restart when there is a change.

Install nodemon:

```
npm install --save-dev nodemon
```

Though JavaScript utilities like nodemon are often recommended to be installed globally, different projects might use different utilities. Therefore we shall add it to devDependencies.
