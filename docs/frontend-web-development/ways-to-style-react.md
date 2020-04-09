# Some common ways to style React

How to style CSS components is a very opinionated topic, and we typically don't want to cover them in the course. Working on multiple projects that use various ways of styling, I realise that no styling methodology can make styling easy to read and modify if developers don't want to learn CSS properly or doesn't want to follow team's convention.

We have listed a few ways that are common in styling react.

1. Importing CSS File with using the `classname`.
2. Importing CSS File with use with CSS Module
3. Inline Styling
4. Using Styled Components
5. Using SASS

## Importing CSS File with use classname

### Advantage

- Simple and easy to understand
- when creating react app, an example is already there
- no additional learning require if you already know HTML and CSS

### Disadvantage

- Child component maybe be affected by the imported CSS file
- It is hard to style deeply nested components
- might require additional convention in the team to keep things standardise

App.css

```css
.App-header h1 {
  color: rebeccapurple;
}
```

App.js

```js
import React from "react";
import "./App.css";

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Hello</h1>
      </header>
    </div>
  );
}

export default App;
```

## Importing CSS File with use with CSS Module

### Advantage

- CRA supports CSS Module out of the box meaning no additional installation required
- Relatively simple and same as CSS
- Child components will not be affected

### Disadvantage

- `classname` get change, which makes selecting element harder if you need to find the element for testing or debugging.

App.module.css

```
.appHeader {
  color: rebeccapurple;
}
```

App.js

```
import React from 'react';
import styles from "./App.module.css";

function App() {
  return (
    <div>
      <header>
        <h1 className={styles.appHeader}>Hello</h1>
      </header>
    </div>
  );
}

export default App;
```

## Inline Styling

### Advantage

- Inline Styling makes referencing of styles by the component easier
- There is also a minimum learning curve
- Styles are unlikely to get overwritten

### Disadvantage

- Mixing view and logic in the same place
- files can become quite large in length which can make finding code harder and more difficult to read
- you lost the option to style items globally which might need more styles.

```javascript
import React from "react";
import "./App.scss";

function App() {
  return (
    <div>
      <header>
        <h1 style={styles.header}>Hello</h1>
      </header>
    </div>
  );
}

const styles = {
  header: {
    color: "rebeccapurple",
  },
};

export default App;
```

## Using Styled Components

### Advantage

- CSS and JS are all in one file making referencing of styles by the component easier
- There is a learning curve

### Disadvantage

- The syntax can look weird to people who are new to it
- Mixing view and logic in the same place
- files can become quite large in length which can make finding code harder and more difficult to read
- require an additional package

Install the package`npm install styled-components`.

```javascript
import React from "react";
import styled from "styled-components";

const Header = styled.h1`
  color: rebeccapurple;
`;

function App() {
  return (
    <div>
      <header>
        <Header>Hello</Header>
      </header>
    </div>
  );
}

export default App;
```

## Using SASS

### Advantage

- Relative similar to CSS meaning there is a low learning curve
- SASS is a trendy package with many developers find it more intuitive than CSS
- Allow variables and computations
- Allow nested styles

### Disadvantage

- Developers who only work on SASS tend not to understand how CSS selectors work
- There is a learning curve to use SCSS features
- There is not much benefit if you already separate its small components and style them individually
- you need to install the package

1. install sass `npm install node-sass`
2. Create an SCSS file `scss`

App.scss

```css
$myColor: rebeccapurple;

.App {
  h1 {
    color: $myColor;
  }

  .pink-text {
    color: pink;
  }
}
```

3. Import Sass in our javascript file

```javascript
import React from "react";
import "./App.scss";

function App() {
  return (
    <div>
      <h1>Hello</h1>
      <p clsasName="pink-text">pink text</p>
    </div>
  );
}

export default App;
```
