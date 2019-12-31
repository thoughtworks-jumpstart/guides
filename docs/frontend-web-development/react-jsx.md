# React JSX

Javascript Extention

## Covers

1. What is JSX
2. Adding logic to customise behaviour

## What is JSX

In our Hello World app we saw the following code bellow in App.js

```javascript
function App() {
  return (
    <div className="App">
      <p>Hello World!</p>
    </div>
  );
}
```

You might probably realise that the App function is returning an HTML like code. Instead of having the class as "class", it becomes "className".

This syntax is called a Javascript Extension(JSX). JSX is possible because of babel helping to translate the jsx syntax into javascript. We will not look further into babel instead we going to focus on JSX.

Using JSX, attribute typically available becomes camelCase and some have a slight change in name. Example class to className.

Later we will see how we can create custom props that can be passed in as an attribute, for now just notice the difference.

## Adding logic to customise behaviour

In JSX, we can use the full power of javascript. Considering the following code.

```javascript
import React from "react";
import "./App.css";

function toUpperCase(word) {
  return word.toUpperCase();
}

function App() {
  return (
    <div className="App">
      <p>{toUpperCase("Hello World!")}</p>
    </div>
  );
}

export default App;
```

You can also change the value that will be passed into an attribute. Let trying adding a new class.

First lets add a new classname `.app--red`

```css
.app {
  text-align: center;
}

.app--red {
  color: red;
}
```

```javascript
import React from "react";
import "./App.css";

function toUpperCase(word) {
  return word.toUpperCase();
}

function addRedClassMaybe(classes) {
  return classes.concat(Math.random() > 0.5 ? " app--red" : "");
}

function getAppClassName() {
  let classnames = "app";
  classnames = addRedClassMaybe(classnames);

  return classnames;
}

function App() {
  return (
    <div className={getAppClassName()}>
      <p>{toUpperCase("Hello World!")}</p>
    </div>
  );
}

export default App;
```

### Arrays

Very often, you will have to show a list of items to the end user.
For example, a list of available drinks in a vending machine, list of favorite items.
JSX allow you to return array of elements / components.

```javascript
function listDisneyMovies() {
  const favDisneyMovies = [
    " Tangled",
    " Monsters, Inc.",
    "Aladdin",
    "Toy Story",
    "Mulan",
  ];

  return favDisneyMovies.map(movie => <div>{movie}</div>);
}

function App() {
  return <div className="App">{listDisneyMovies()}</div>;
}
```

### Exercise

1. create a function and use it to make the Hello World! all lowercase.
2. create another function that makes odd number words uppercase and even number of word lowercase

- from "Lorem Ipsum is simply dummy text of the printing and typesetting industry."
- to "LOREM ipsum IS simply DUMMY text OF the PRINTING and TYPESETTING industry."

![Lorem Ipsum](/_media/loremIpsumAltCase.png)

### To Infinity And Beyond

3. create another function that alternates the character in the sentence. Ignoring spaces.

![Lorem Ipsum alt char case](/_media/loremIpsumAltCharCase.png)

4. modify the function from 3, add different colors to the alternate charachters.

![Lorem Ipsum alt color char case](/_media/loremIpsumAltColorCharCase.png)
