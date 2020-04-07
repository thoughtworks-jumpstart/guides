# Redux Thunk

## About

Redux thunk is a middleware that allows you to create asynchronous action. We are going to build a Todolist that fetches data using the [jsonplaceholder](https://jsonplaceholder.typicode.com/) to give us some data.

Redux Thunk is probably the most popular middleware to support async in actions creator due to its simplicity and being one of the first excellent libraries to do so.

This guide assumes you know actions and reducers in redux.

Other popular alternatives to Redux Thunk

- redux-saga - Use generator - Have the flexibility to massage the data before sending data or receiving data back.
  - Can be quite complicated to understand
- redux-promise-middleware - like redux-thunk but less popular - relatively easy to use and understand

## Installation

```sh
npm install redux react-redux redux-thunk
```

- redux: provides the capability to create a store
- react-reduct: connects the store to component
- redux-thunk: middleware to async in actions creator

## 1. Create a reducer

In `src/store/todo.reducer.js`

```javascript
const defaultState = [];

const todoReducer = (state = defaultState, action) => {
  switch (action.type) {
    case "ADD_TODO":
      state.push(action.payload);
      return [...state];
    default:
      return state;
  }
};

export default todoReducer;
```

## 2. Using the reducer Create a store

The benefit of Redux lies within using a single store. The store will have access to all the data.

In src/store/index.js

```javascript
import todoReducer from "./todo.reducer";
import { createStore, combineReducers, applyMiddleware } from "redux";
import thunk from "redux-thunk";

const rootReducer = combineReducers({
  todolist: todoReducer,
});

const store = createStore(rootReducer, applyMiddleware(thunk));

export default store;
```

### 3. Create Todo Component

The Todo Component is the same as not using Redux Thunk. We use `connect` to map the states we get back from the store and pass it as props.

```javascript
import React from "react";
import { connect } from "react-redux";
import { addTodoFromAPI } from "./actions";

const TodoItem = ({ id, title, completed }) => {
  return <p>{`${id}. ${title} is ${completed ? "" : "not "}completed`}</p>;
};

const TodoList = (props) => {
  return (
    <div>
      Todo:{" "}
      {props.todolist.map((item) => (
        <TodoItem
          key={item.id}
          id={item.id}
          title={item.title}
          completed={item.completed}
        />
      ))}
      <div>
        <button
          onClick={() => {
            // do nothing now
          }}
        >
          add todo
        </button>
      </div>
    </div>
  );
};

const mapStateToProps = (state) => {
  return {
    todolist: state.todolist,
  };
};

export default connect(mapStateToProps)(TodoList);
```

### 4. Connect the Store to the App

Usually, we do it at the level that will wrap all our components.

```javascript
import React from "react";
import "./App.css";
import { Provider } from "react-redux";
import store from "./store";
import Todo from "./Todo";

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <Todo />
      </div>
    </Provider>
  );
}

export default App;
```

## 5. Create Action Creators

Here we add two action creators. The first `addTodo` is a typical action creator that returns an object with a type and a payload.

The second action creator is something new for _Redux Thunk_ m we create an action creator `addTodoFromAPI` that returns a function taking in a single argument `dispatch`. When can then do whatever async code we want and in the `.then` we call `dispatch` with another action creator that does the actual updating of the data in the reducer.

In actions/index.js

```javascript
export const addTodo = (payload) => {
  return {
    type: "ADD_TODO",
    payload,
  };
};

let todoCounter = 1;
export const addTodoFromAPI = () => {
  return (dispatch) => {
    fetch(`https://jsonplaceholder.typicode.com/todos/${todoCounter++}`)
      .then((response) => response.json())
      .then((payload) => dispatch(addTodo(payload)));
  };
};
```

## Lastly, We can now map the action creators back to our Todo Component and use them in the component

```javascript
import React from "react";
import { connect } from "react-redux";
import { addTodoFromAPI } from "./actions";

const TodoItem = ({ id, title, completed }) => {
  return <p>{`${id}. ${title} is ${completed ? "" : "not "}completed`}</p>;
};

const TodoList = (props) => {
  return (
    <div>
      Todo:{" "}
      {props.todolist.map((item) => (
        <TodoItem
          key={item.id}
          id={item.id}
          title={item.title}
          completed={item.completed}
        />
      ))}
      <div>
        <button onClick={() => props.addTodoFromAPI()}>add todo</button>
      </div>
    </div>
  );
};

const mapStateToProps = (state) => {
  return {
    todolist: state.todolist,
  };
};

const mapDispatchToProps = {
  addTodoFromAPI,
};

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);
```
