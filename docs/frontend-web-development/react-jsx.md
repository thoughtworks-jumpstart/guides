# React JSX

JavaScript XML

## Covers

1. What is JSX
2. Adding logic to customise the behaviour

## What is JSX

In our Hello World app, we saw the following code bellow in App.js.

```javascript
function App() {
  return (
    <div className="App">
      <p>Hello World!</p>
    </div>
  );
}
```

You might probably realise that the App function is returning HTML-like code. Instead of having the class _class_, we use the attribute _className_.

This syntax is called JavaScript XML(JSX). We are able to use JSX because Babel helps to translate the JSX syntax into javascript.

When using JSX, attributes that are typically available to us in HTML changes to _camelCase_, and some have slight changes in name. A good example would be how the _class_ attribute has changed to _className_.

Later, we shall also see how we can create custom props that can be passed in as an attribute. For now, just make sure to notice the difference.

## Adding logic to customise the behaviour

In JSX, we can utilise the full power of JavaScript. Let's consider the following code.

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

You can also insert the value that is returned by a function into an attribute. Let's try adding a new class.

Firstly, let us add a new class name `.app--red`

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

We insert the return value of `getAppClassName()` into the `className` attribute dynamically.

### Arrays

Very often, you might need to show a list of items to the user.

For example, you might need to show the user:

- a list of available drinks in a vending machine
- a list of favourite items

JSX allows you to return an array of elements/components.

```javascript
function listDisneyMovies() {
  const favDisneyMovies = [
    "Tangled",
    "Monsters, Inc.",
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

1. create a function and use it to make the Hello World! All lowercase.
2. create another function that makes odd number words uppercase and even number of word lowercase

- from `Lorem Ipsum is simply dummy text of the printing and typesetting industry.`
- to `LOREM ipsum IS simply DUMMY text OF the PRINTING and TYPESETTING industry.`

![Lorem Ipsum](/_media/loremIpsumAltCase.png)

### To Infinity And Beyond

3. create another function that alternates the character in the sentence — ignoring spaces.

![Lorem Ipsum alt char case](/_media/loremIpsumAltCharCase.png)

4. modify the function from 3, add different colours to the alternate characters.

![Lorem Ipsum alt color char case](/_media/loremIpsumAltColorCharCase.png)
