# React Context

React Context is an advanced topic and you should already be familiar with basic React concepts before proceeding with the rest of this topic.

In a typical React application, data is passed from a parent component to a child component via props. This is usually fine, but can become cumbersome for certain types of props (e.g. locale preference, UI theme) that are required by many components within an application.

## State management libraries

One way to solve this is to use a state management library like Redux or Mobx, but as the creator of Redux himself has written, [you might not need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367).

## You may not need Context

### Component composition

In the same way, you may not need Context. As the React documentation states, if you only want to avoid passing some props through many levels, [component composition](https://reactjs.org/docs/composition-vs-inheritance.html) is often a simpler solution than context.

### Render props

Another option is to create a component with a [render props](https://reactjs.org/docs/render-props.html) which takes a function that returns a React element and calls it instead of implementing its own render logic.

## Context

Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree. Learn more about [Context](https://reactjs.org/docs/context.html).
