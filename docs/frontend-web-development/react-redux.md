# React Redux

## covers:

1. What is Redux
2. Do you still need redux with `use-context` hooks?
3. Setting up Redux
4. Creating an action
5. Updating states in Reducer
6. Getting data from Reducer in component
7. Handling asynchronous call with redux-promise-middleware

## What is Redux

Redux is a pattern for managing application state.

In simple, a way for you to organise the data shared between the components in your application.

## Do you still need redux with `use-context` hooks?

Yes and No. Maybe. I don't know.

When a state changes in the component, all child component will re-rendered. Updating Context frequently means your application might potentially re-render a lot more times than what you expect. Redux directly connect to the component as a HOC, this means, when the data updates, only the component using the updated data and it's children get re-rendered compare to the whole app re-rendering frequently. It can be much more performant on a big application.

Not using redux might make your app load slightly faster. You also have one less external dependency to use, learn and update, which might saves developer time. Is always better to have fewer libraries if it doesn't bring you additional value.

If your component requires lots of data sharing between component, your app is going to perform better with Redux else use Content.
