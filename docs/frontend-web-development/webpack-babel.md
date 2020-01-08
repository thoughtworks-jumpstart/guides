# Webpack and Babel

`create react app` uses Webpack and babel to create a straightforward setup for users.
While it might not help you on your day to day work as a react developer, It is beneficial to have a glimpse of how the magic happens.

## Covers

1. What is Webpack
2. Simple demo on Webpack
3. What is Babel
4. Using Babel with Webpack

## What is Webpack

Webpack is a module bundler. In simple, it just does 3 things.

1. look for files based on given conditions
2. do something to the files using a loader, typically to bundle multiple files into one file
3. put the result somewhere (i.e. dist/main.js)

For a long explanation, you can refer to the [official documentation](https://webpack.js.org/concepts/)

## Simple demo on Webpack

1. create a new folder: `mkdir webpack-demo`
2. create an `index.html` file

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Webpack Demo</title>
  </head>
  <body>
    <div id="container"></div>
    <script src="dist/bundle.js"></script>
  </body>
</html>
```

3. install Webpack

```sh
npm init -y
npm install webpack webpack-cli
npm install moment
```

4. create an `index.js` file in the `src` folder

```javascript
import moment from "moment";

const container = document.getElementById("container");
container.appendChild(document.createTextNode(moment()));
```

5. Create a Webpack config file `webpack.config.js`

```javascript
module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
  },
};
```

6. Add run Webpack script in package.json

```javascript
  "scripts": {
    "webpack:dev": "webpack --mode development --watch"
  },
```

7. Run the build script `npm run webpack:dev`, a file is created for you `dist/bundle.js`

8. View index.html on a browser or run in live server. The time should show correctly.

Webpack have successfully bundle `moment.js` with `index.js` to a single file `bundle.js`
