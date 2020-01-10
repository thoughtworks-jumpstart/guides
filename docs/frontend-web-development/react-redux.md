# React Redux

## covers:

1. What is Redux
2. Do you still need redux
3. How Redux works with React
4. Setting up the project
5. Creating a store
6. Get data from the store
7. Update data in the store

## What is Redux

Redux is a pattern for managing application state.

In simple, a way for you to organise the data shared between the components in your application.

## Do you still need redux

Yes and No. Maybe. I don't know.

When a state changes in the component, all child component will re-rendered. Updating Context frequently means your application might potentially re-render a lot more times than what you expect. Redux directly connect to the component as a HOC, this means, when the data updates, only the component using the updated data and it's children get re-rendered compare to the whole app re-rendering frequently. It can be much more performant on a big application.

Not using redux might make your app load slightly faster. You also have one less external dependency to use, learn and update, which might saves developer time. Is always better to have fewer libraries if it doesn't bring you additional value.

If your component requires lots of data sharing between component, your app is going to perform better with Redux else use `use-content` instead.

## How Redux works with React

### Terminology

- `store`: holds all the data
- `connect`: component marks intent to want data from the store
- `mapStateToProps`: a function to take specific data from the store and put into the component's prop
- `action`: an object with a `type` and optional `payload`
  - `type`: the intent of the action
  - `payload`: data that comes along with the action. It doesn't have to be called `payload`, but `payload` carries a proper meaning most of the time.
- `dispatch`: takes in an action and trigger `reducer` to update data
- `reducer`: a function that automatically gets triggered when an action is a dispatch. Update data based on the action receives and returns an updated data object to the store.

### Retrieving data:

Redux helps you to create a common object(`store`) that hold all shared data. Data in `store` is a combination of all the different data for the individual component. Data in the `store` is made available to different components base on a subscription model(using `connect`). We can create a function(`mapStateToProps`) to map the data into the `props` of the component letting the component have access to the data.

store => mapStateToProps => component.props

### Updating data:

We can update data by triggering a `dispatch`. The `dispatch` takes in an `action` as an argument. `action` is an object with a `type` that indicates the intent and an optional `payload` for additional data. Dispatching an action triggers the `reducer` which identify the data to update based on the `type` with the data to update with which is the `payload`. The dispatch also updates the data in the store.

component dispatch action => reducer => store

## Setting up the project

Chinese New Year is near at the time of the writing, so we are going to build a fortune cookie app.

First, create a new React Project `fortunes` and install some dependencies.

`redux` provide us with the functionality to create a store and the ability to update it. `react-redux` create the binding/connection between react component and redux store.

```sh
npm install redux react-redux
```

## Creating a store

### Create a reducer.

src/store/fortunes.js

```javascript
const initialState = {
  predictions: [
    {
      id: 0,
      name: "John Smith",
      fortune: "Over self-confidence is equal to being blind.",
    },
    {
      id: 1,
      name: "Alice"
      fortune: "Happiness isn't something you remember, it's something you experience."
    }
  ],
};

export default function(state = initialState, action) {
  switch (action.type) {
    default:
      return state;
  }
}
```

A reducer takes in the current state of the object, and an `action`. We are going to look at the `action` in a later section.

The first time the reducer runs, we do not have a state, we can initialise the current state with some hardcoded data or an empty array. We choose to initialise with some predictions so we can explore how to retrieve the data.

The reducer returns an object. This object is the new data in the store. For now, we return the state without modification.

### Export the store object

src/store/index.js

```javascript
import { createStore, combineReducers } from "redux";
import fortunes from "./fortunes";

const reducers = combineReducers({
  fortunes,
});

export default createStore(reducers);
```

We can have many reducers, separating the reducers based on the business requirements allow us to group related data. Data related are also more likely to be retrieved and used together, so having different reducers help us to organise the code in a more maintainable and readable way compare to having everything in one file.

When we have more than one reducer, we need to combine them. Redux provides us with a handy method `combineReducer` for this purpose. After combining the reducer, we pass the combined reducer into the `createStore` method for Redux to create the store for us.

We export the store for our application to consume.

### Connecting the app to store

We then can connect our app to our store
src/App.js

```javascript
import React from "react";
import { Provider } from "react-redux";
import store from "./store";
import FortuneList from "./components/FortuneList";
import "./App.css";

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <FortuneList />
      </div>
    </Provider>
  );
}

export default App;
```

Here we import in the created `store` from `./store`. For our react app to use the data, we need a `Provider` wrap around the app component. Now, all component inside have access to the store.

## Get data from the store

src/components/FortuneList.js

```javascript
import React from "react";
import { connect } from "react-redux";
import UserFortune from "./UserFortune";

const FortuneList = ({ fortunes }) => {
  return (
    <div>
      <h1>Fortune List</h1>
      {fortunes.predictions.map(data => (
        <UserFortune name={data.name} prediction={data.fortune} />
      ))}
    </div>
  );
};

const mapStateToProps = state => {
  const { fortunes } = state;
  return { fortunes };
};

export default connect(mapStateToProps)(FortuneList);
```

First, we import `connect` form `react-redux`.
Write a function, `mapStateToProps`, the function returns data that get added to the props. In the state, we have all the data inside the store.
Recall in earlier section, `const reducers = combineReducers({ fortunes });`
When data in the store gets updated. The method triggers, and if there is a change in the `props` that we return in `mapStateToProps`, the component gets re-render.

Now, the fortune data is available in the component.

## Update data in the store

### Writing an action creator

First, we create an action creator. The action creator returns an object with `type` and `payload`, the `type` is the intent of the action and the optional `payload` represents the data.

src/actions/index.js

```javascript
export const ADD_FORTUNE_TYPE = "ADD_FORTUNE";
export const addFortune = (id, name, fortune) => {
  return {
    type: ADD_FORTUNE_TYPE,
    payload: {
      id,
      name,
      fortune,
    },
  };
};
```

When the reducer receives the `ADD_FORTUNE` action, we then add the new prediction to the state.

src/store/fortunes.js

```javascript
import { ADD_FORTUNE_TYPE } from "../actions";

const initialState = {
  predictions: [],
};

export default function(state = initialState, action) {
  switch (action.type) {
    case ADD_FORTUNE_TYPE:
      return {
        ...state,
        predictions: [...state.predictions, action.payload],
      };
    default:
      return state;
  }
}
```

Dispatching the add fortune action when the button gets pressed.

src/components/FortuneList.js

```javascript
import React, { useState } from "react";
import { connect } from "react-redux";
import UserFortune from "./UserFortune";
import { addFortune } from "../actions";
import uuid from "uuid/v1";
import fortuneTeller from "../utils/fortuneTeller";

const FortuneList = ({ fortunes, addFortune }) => {
  const [newName, setNewName] = useState("");

  return (
    <div>
      <h1>Fortune List</h1>
      {fortunes.predictions.map(data => (
        <UserFortune key={data.id} name={data.name} prediction={data.fortune} />
      ))}

      <div>
        <input
          type="input"
          onChange={event => setNewName(event.target.value)}
        />
        <button
          onClick={() => {
            addFortune(uuid(), newName, fortuneTeller());
            setNewName("");
          }}
        >
          Get Fortune
        </button>
      </div>
    </div>
  );
};

const mapStateToProps = state => {
  const { fortunes } = state;
  return { fortunes };
};

const mapDispatchToProps = dispatch => ({
  addFortune: (id, name, fortune) => dispatch(addFortune(id, name, fortune)),
});

export default connect(mapStateToProps, mapDispatchToProps)(FortuneList);
```

The important thing here is the `mapDispatchToProps` function where we pass a method that triggers the dispatch to our props.

```javascript
const mapDispatchToProps = {
  addFortune,
};

export default connect(mapStateToProps, mapDispatchToProps)(FortuneList);
```

The above code is equivalent, passing `mapDispatchToProps` as an object connects the dispatch for us implicitly.

![fortune telling app](_media/redux_fortuneTelling.png)
