# Import and Export of Components

Moving Components into separate file makes codes more maintainable.
The individual file should be as small as possible while containing enough logic to serve a whole purpose.

## Covers

1. import external modules
2. import export of components
3. import export of default component
4. import export of default object

## Import external modules

When using code not written by us

```sh
  npm install pokemon
  npm start
```

```javascript
import React from "react";
import pokemon from "pokemon";

function App() {
  return <div className="App">{pokemon.random()}</div>;
}

export default App;
```

## Import export of components

1. Create a folder "components" in the "src" folder
2. Create a new file "greetings.js"
3. Import React
4. export SayHi and SayHello component

src/components/greetings.js

```javascript
import React from "react";

export const SayHi = ({ name }) => {
  return <div>Hi {name}</div>;
};

export const SayHello = ({ name }) => {
  return <div>Hello {name}</div>;
};
```

src/App.js

```javascript
import React from "react";
import { SayHi, SayHello } from "./components/greetings";

function App() {
  return (
    <div className="App">
      <SayHi name="Alice" />
      <SayHello name="Bob" />
    </div>
  );
}
```

We usually use this method to import functions that are closely related.

## Import and export of a default component

```javascript
import React from "react";

export default ({ name }) => {
  return <div>{name} says Hi</div>;
};
```

```
import React from "react";
import SayHi from "./components/SayHi";

function App() {
  return (
    <div className="App">
      <SayHi name="Alice" />
    </div>
  );
}
```

## Import and export of a default object

There is also an option to export all items as an Object to group related items together.
Is more commonly to use utility functions than Component.

```javascript
import React from "react";

const SayHi = ({ name }) => {
  return <div>{name} says Hi</div>;
};

const SayHello = ({ name }) => {
  return <div>Hello {name}</div>;
};

export default {
  SayHi,
  SayHello,
};
```

```javascript
import React from "react";
import Greetings from "./components/greetings";

function App() {
  return (
    <div className="App">
      <Greetings.SayHi name="Alice" />
      <Greetings.SayHello name="Bob" />
    </div>
  );
}
```

## Lab

We have an application that is not working currently due to missing import and export statement.

1. git clone from `git clone https://github.com/thoughtworks-jumpstart/react-import-export.git`
2. Read the readme and fix the application
