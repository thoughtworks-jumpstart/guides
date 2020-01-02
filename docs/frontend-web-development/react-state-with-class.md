# React States with Class Compoent

## Covers

1. Why do we need state in a React component
2. What is the difference between state and props?
3. What is local state in a React component
4. Initialize component state of a component
5. How to update state of a component
6. Pitfalls

## Why do we need state in a React component

An React application typically needs to maintain some state information. For example, if you are building the frontend for GrabTaxi for users to book cabs in React, the React application needs to keep a lot of information in its state:

- Who is the customer?
- Location of the customer
- Which driver takes the task?
- Location of the driver

## What is the difference between state and props?

props (short for "properties") and `state` are both plain JavaScript objects. While both hold information that influences the output of render, they are different in one important way: props get passed to the component (similar to function parameters) whereas `state` is managed within the component (similar to variables declared within a function).

To find out whether a piece of data should reside in `state` or in `props`, simply ask three questions about the piece of data:

1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it remain unchanged over time? If so, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

## What is local state in a React component?

- `state` is a plain javascript object ({}) where we store data that is internal/local to the component
- Only components declared as a class can have its own state. Functional React components don't have state.
- State should be considered private to the component. When we violate this principle of _encapsulation_ (e.g. another component (e.g. Navbar) knows about another component's state (e.g. Article), we will end up with tightly coupled code that is hard to (i) understand, (ii) change, (iii) extend. _Don't do it_.
- State is reserved only for interactivity, that is, data that changes over time.

## Initialize component state of a component

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      // `state` is the internal data store of a component. it's just a plain ol' javascript object
      value: 0,
    };
  }
  render() {
    <div>Counter value: {this.state.value}</div>;
  }
}

export default Counter;
```

## How to update state of a component

- The only way to change state is by calling this.setState().
- Warning: never modify this.state object directly. We will talk about this in details below.
- Where can you call setState()? Typically in some event handlers. You can also call it in some React lifecycle methods such as componentDidUpdate.

### setState API

The full signature of the setState() API is
setState(updater[, callback])
Where

- updater is either an object with the updated state, or a function that returns updated state
- callback is called once state is updated and the component is re-rendered

To update this.state.value, we call this.setState(). Update your code and try clicking on your <Counter/> component on your browser. You should see the name is changed when you click it.

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 0,
    };
  }

  render() {
    return (
      <div>
        <div>Counter value: {this.state.value}</div>
        <button onClick={this.handleClick}>increase count</button>
      </div>
    );
  }

  handleClick = () => {
    this.setState({ value: this.state.value + 1 });
  };
}
```

Bonus exercise:

- Inspect the HTML elements in your browser devtools and observe that only one line of the DOM is changed when you click on the component. This is what makes React fast

### Example: updater can be a function that returns updated state

```javascript
(state, props) => stateChange;
```

We can update the example above with this new approach:

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 0,
    };
  }

  render() {
    return (
      <div>
        <div>Counter value: {this.state.value}</div>
        <button onClick={this.handleClick}>increase count</button>
      </div>
    );
  }

  handleClick = () => {
    this.setState((currentState, props) => ({ value: currentState.value + 1 }));
  };
}
```

With this approach, you can derive new state from the current state. Note that currentState is a reference to the component state at the time the change is being applied. It should not be directly mutated. Instead, changes should be represented by building a new object based on the input from curentState and props of the component.

### A new state object is created after setState is executed

Each time when the `setState` function is executed, React clones the current state object into a new object, then it updates the cloned object with the value provided in the parameter.
Highlights:

- A new state object is created (i.e. this.state points to a different object after `setState` is executed)
- The previous state object remains unchanged. It's the newly cloned state object that is updated with new values.

This basically means a state object is immutable once it's created (unless you modify the state object directly).

### The output of the updater is merged with the new state object

The object returned from the updater argument is merged with the newly created state object.
Conceptually, this is what React does in the previous example

```javascript
this.state = Object.assign(this.state, { value: this.state.value + 1 });
```

This means, in the newly created `state` object, only the `name` property is overwritten with the new value provided in the `setState` API. If the `state` object contains any other properties, those are remain unchanged.

### `setState` API is asynchronous

The API call schedules an update to a component’s state object. When state changes, the component responds by re-rendering.

Note that the update to the state may not be done immediately. For performance optimization reasons, React internally buffers a few updates to state and flush all the changes in one go.

If you are trying to read the value of state object right after calling setState, you may not get the latest value.

In short, think of setState() as a request rather than an immediate command to update the component. Refer to the documentation of setState for more details.

### How do I know when the state is updated?

Since the setState API call is asynchronous, we have a challenge: If we have some logic to run when the state is updated, where should I put that piece of logic?

There are two solutions.

- First one, the lifecycle method componentDidUpdate would be called when the state object is updated. setState() will always lead to a re-render unless shouldComponentUpdate() returns false.
- Second one, the setState() API actually can take in a callback, which is called when the state is updated.

Out of these two solutions, the first solution is preferred.

## Pitfalls

**Don't update the state object directly!!!!**

The only way to change state is by calling `this.setState()`. You can't assign value to the state variable directly except when you declare the initial value of that variable.

For example, the following code won't work (except in constructor):

```javascript
this.state.value = "new value";
```

Why?

First reason: because React is not aware that the state is updated if you update the state object directly. Hence React won't re-render the component after you update the state object by yourself.

In contrast, when you call setState, React is aware of the state update and re-render the component correctly.

### Don't mutate existing state object

You might ask: "can I update the state object myself and then call the setState API?", like the following case:

```javascript
let items = this.state.items;
items.push(aNewItem);

this.setState({ items: items });
```

With the codes above, the changes in items cannot be detected by React because the items object is the same object before and after the state update.

One example is given in this article. In the example, a parent component passes some of its state as props to a child component (which happens to be a PureComponent). If you modify the state.items of the parent component directly instead of creating a new items object, the child component could not detect the change in its items prop and does not re-render.

Hence is generally a good practice to avoid mutating existing state object. If you need to update any field/value in the state, you should create a new copy of the value and call setState.

### Don't call setState in some React lifecycle methods like render

There are some React lifecyle methods where you should not call setState. You can find more discussion in this article

## Exercise

1. Create a Counter component with 2 buttons

- Create a new react app
- Display a initial value of 0
- Create 2 button
  - First button increases the count
  - Second button decreases the count.

### Optional

2.  Create a FizzBuzz Counter

- Modify the counter above.
  - Display the word "Fizz" instead of the number if it can be divided by 3
  - Display the word "Buzz" instead of the number if it can be divide by 5
  - Display the word "FizzBuzz" instead of "Fizz" or "Buzz" if it can be divded by both 3 and 5
  - Display number 0 if counter value is at 0.
  - Display the number value otherwise.
