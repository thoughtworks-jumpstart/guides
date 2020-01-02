# Lifecycle of react component

## Cover:

1. Introduction
2. Commonly use lifecycle method

## Introduction

Each class based React component has 11 “lifecycle methods” which you can override to run your own code at particular times in the process.

React's lifecycle hooks

- **Mounting**: These methods are called when an instance of a component is being created and inserted into the DOM

  - constructor()
  - getDerivedStateFromProps()
  - render()
  - componentDidMount()

- **Updating**: An update can be caused by changes to props or state. These methods are called when a component is being re-rendered

  - getDerivedStateFromProps()
  - shouldComponentUpdate()
  - render()
  - getSnapshotBeforeUpdate()
  - componentDidUpdate()

- **Unmounting**: This method is called when a component is being removed from the DOM

  - componentWillUnmount()

- **Error handling**: This method is called when there is an error during rendering, in a lifecycle method, or in the constructor of any child component

  - componentDidCatch()

- Methods prefixed with will are called right before something happens
- Methods prefixed with did are called right after something happens.

![react lifecycle hooks](_media/react-lifecycle.jpeg)

## Commonly use lifecycle method

1. Fetch data from server
2. Prevent component to rerender

### Fetch data from server

Lifecycle when mounting component

1. consturctor: initialise default value of state.
2. render: create the react DOM
3. **componentDidMount**: fetch value from server.

The lifecycle method we going to focus here is componentDidMount. We have already seen constructor and render in the previous chapters.

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    console.log("1. In Constructor");
    this.state = {
      isLoading: true,
      data: [],
    };
  }

  render() {
    console.log("2. Rendering...");
    return <div>My Component</div>;
  }

  componentDidMount() {
    console.log("3. Component is ready");
    fetchData().then(newData => {
      this.setState({
        isLoading: false,
        data: newData,
      });
    });
  }
}

function App() {
  return (
    <div className="App">
      <MyComponent />
    </div>
  );
}
```

componentDidMount happens right after render. This makes componentDidMount a good place to put logic that requires DOM node to be present or for fetching data that is requried by the components.

### Prevent component to rerender

Lifecycle when updating component

1. **shouldComponentUpdate**: checks if rendering should be called
2. render: create the updated react DOM
3. componentDidUpdate: called when component finish rendering.

Another frequently use component is `shouldComponentUpdate`. In this method, you will have access to the updated props and states values that will be use during the upcoming render.

- returning true will cause render to be called
- returning a false will prevents rendering,

Component update is typically triggered by a change in state from `setState` or change in value of `props` from a parent component.

Say if we have a Counter and we only want to rerender the component when the value is odd number.
We can check if the nextState value is 1 odd number. If is an odd number, we return true triggering a rerender. By default we will return a false.

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      value: 0,
    };
  }

  shouldComponentUpdate(nextProps, nextState) {
    const isOddNumber = nextState.value % 2 === 1;
    if (isOddNumber) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <div>
        <div>{this.state.value}</div>
        <button onClick={() => this.setState({ value: this.state.value + 1 })}>
          +1
        </button>
      </div>
    );
  }
}
```
