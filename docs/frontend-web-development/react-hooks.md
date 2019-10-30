# React Hooks

Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

Learn more about [React Hooks](https://reactjs.org/docs/hooks-intro.html)

## State Hook

Typically, when you want to add state to a function component you need to convert it into a class component. React Hooks solves this issue by allowing you to use state in function components.

First, import `useState` from React.

```js
import React, {useState} from "react";
```

### Managing state in class components

First, lets recall how we manipulate state in a class component

Declaring state in a class component

```js
this.state = {
  count: 0
};
```

Setting state in a class component

```js
this.setState({count: 1});
```

### Managing state in function components

Declaring state in a functional component with `useState`. `useState` is a function that takes one argument as the initial value of the state.

```js
const [count, setCount] = useState(0);
```

Setting state in a class component

```js
setCount(1);
```

Here's an example

```js
import React, {useState} from "react";

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

## Effect Hook

In class components, we are able to perform side effects by taking advantage of lifecycle methods like `componentDidMount` and `componentDidUpdate`.

How would we do this in function components? Answer, `useEffect`.

Here's an example

```js
import React, {useState, useEffect} from "react";

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

## Rules of Hooks

It is important to note that **React relies on the order in which Hooks are called**. See [explanation](https://reactjs.org/docs/hooks-rules.html#explanation).

0. Always use Hooks at the top level of your React function. Don’t call Hooks inside loops, conditions, or nested functions.
0. Only call Hooks from React functions. Don’t call Hooks from regular JavaScript functions

To ensure these rules are enforced, configure [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks).

When using React Hooks you have to be careful about how to use them. Read more about [these rules](https://reactjs.org/docs/hooks-rules.html).
