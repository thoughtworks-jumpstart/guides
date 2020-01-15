# Hello World with React

## Covers

1. Creating a React app with create-react-app
2. Understanding the different components that makes up React

## Creating a hello-world app with create-react-app

### Creating the project folder

We can use _create-react-app_ to easily setup a project for us

```sh
  npx create-react-app hello-world
```

Verify this by type the "ls" command, and you should see the folder `hello-world` in the listing

```sh
ls
```

Go into the folder by using `cd` command.

```sh
cd hello-world
```

Install dependencies.

```sh
npm install
```

Start the app.

```sh
npm run start
```

You should see something similar to the image below in your default browser.

![react start page](_media/react-placeholder.png)

### Cleaning Up

1. Replace the header tag with a paragraph tag and type "Hello World!".
2. Remove all styles in the `App.css` other than `.App`

App.js

```javascript
import React from "react";
import "./App.css";

function App() {
  return (
    <div className="App">
      <p>Hello World!</p>
    </div>
  );
}

export default App;
```

App.css

```css
.App {
  text-align: center;
}
```

## Exercise

1. Create a new react app and print out `Hello \<your favourite character name\>`
2. Add the image of your favourite character.
   ![react start page](_media/react-hello-world-ex1.png)

- tips
  - add the image into the public folder
  - use img tag
  - access the image with `src={process.env.PUBLIC_URL + "/<image name>.jpg"}`
