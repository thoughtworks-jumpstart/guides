# Execution Context

Execution context (EC) is defined as the environment in which JavaScript code is executed.

This topic is advanced, but a clear understanding of Execution Context and Execution Context Stack helps you in understanding how JavaScript code is executed.

## Types of Execution Context

There are three types of execution context in JavaScript.

### Global Execution Context

This is the default or base execution context. The code that is not inside any function is in the global execution context.

- it creates a **global object** which is the window object (in the case of browsers). It will be `module.exports` for Node.js but we will learn more about this in another topic.
- sets the value of `this` to equal to the global object. There can only be one global execution context in a program.

### Functional Execution Context

Each function has its own execution context, but it’s created when the function is invoked or called.

There can be any number of function execution contexts.

### Eval Function Execution Context

Code executed inside an `eval` function also gets its own execution context, but as we usually do not use `eval` in JavaScript and will not discuss it here.

## Execution Stack

Execution stack, also known as “calling stack” in other programming languages, is a stack which is used to store all the execution contexts created during the code execution.

```js
let a = "Hello World!";

function first() {
  console.log("Inside first function");
  second();
  console.log("Again inside first function");
}

function second() {
  console.log("Inside second function");
}

first();
console.log("Inside Global Execution Context");
```

![Execution Context Stack for the above code](https://miro.medium.com/max/2000/1*ACtBy8CIepVTOSYcVwZ34Q.png)

1. The JavaScript engine creates a global execution context and pushes it to the current execution stack.

2. When a call to first() is encountered, the JavaScript engine creates a new execution context for that function and pushes it to the top of the current execution stack

3. When the second() function is called from within the first() function, the JavaScript engine creates a new execution context for that function and pushes it to the top of the current execution stack.

4. When the second() function finishes, its execution context is popped off from the current stack, and the control reaches to the execution context below it, that is the first() function execution context.

5. When the first() finishes, its execution stack is again popped from the stack and control reaches to the global execution context.

6. Once all the code is executed, the JavaScript engine removes the global execution context from the current stack.

## Further reading

- https://tylermcginnis.com/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/

- https://www.freecodecamp.org/news/execution-context-and-the-call-stack-visually-illustrated-by-a-slice-of-tasty-cake-14f9a64dc460/
