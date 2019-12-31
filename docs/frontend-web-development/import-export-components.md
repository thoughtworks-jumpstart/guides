# Import and Export of Components

Moving Components into seperate file makes codes more maintable.
Individual file should be as small as possible while containing enough logic to serve a complete purpose.

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

## import export of components

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

We ususally use this method to import functions that are related but there is no one main function that will typically be use.

## import export of default component

```javascript
import React from "react";

export default ({ name }) => {
  return <div>{name}</div>;
};
```

```
import React from "react";
import SayHi, { SayHello } from "./components/SayHi";

function App() {
  return (
    <div className="App">
      <SayHi name="Alice" />
    </div>
  );
}
```

This is perhaps the most common pattern to import and export components.
One file to hold only one component.

## import export of default object

There is also an option to export all items as an Object to group related items together.
Is more commonly to use utility functions than Component.

```javascript
import React from "react";

const SayHi = ({ name }) => {
  return <div>{name}</div>;
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
