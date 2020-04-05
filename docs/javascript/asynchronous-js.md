# Asynchronous JavaScript

## Event loop

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/8aGhZQkoFbQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Call stack

JavaScript has a call stack for functions (also known as the execution stack of execution contexts). Every time when a function is executed, a new execution context is added to a stack. When one execution context finishes execution, it is removed from the stack.

What’s a stack? A stack is an array-like data structure but it can only add items to the back and only remove the last item. In short, it's Last In First Out (LIFO), like a stack of plates

If some code is "blocking", (maybe it is getting some data from external API) we have to wait for that code to finish running before we can pop it off the call stack and move on to the next function

If we block the call stack, the browser cannot render and it is stuck, looks like the website "hung".

### Asynchronous callbacks

The solution is to have asynchronous callbacks. Example of asynchronous callback is the `display()` callback below.

```js
function main() {
  console.log("front");
  setTimeout(function display() {
    console.log("hello there");
  }, 0);
  console.log("back");
}
main();
```

What is the output on the console?

`setTimeout` is pushed into the call stack when the web API setTimeout is called. It disappears quickly from the call stack but the timer in the web API continues running in the background.

### Task queue and event loop

When the web API is done, it pushes the callback into a task queue.

The event loop then kicks in, because its job is to look at the call stack and task queue (also known as event queue). In fact, it repeatedly checks if there is anything in the task queue.

What’s a queue? A queue is an array-like data structure but it can only add items to the back and only remove the first item. In short, it's First In First Out (FIFO), like a queue at the cashier

**If the call stack is empty**, the event loop takes the first callback in the task queue and pushes it into the call stack

The callback runs. This all happens in the JavaScript V8 Engine.

### Exercise

With the understanding on the event loop, let's do another exercise. Run the code below. Can you explain what you observe with the event-loop model we introduce above?

```js
const origTime = new Date().getSeconds();

setTimeout(function () {
  // prints out "2 secs", meaning that the callback is not called immediately after 500 milliseconds.
  console.log("Ran after " + (new Date().getSeconds() - origTime) + " seconds");
}, 500);

while (true) {
  if (new Date().getSeconds() - origTime >= 2) {
    console.log("Good, looped for 2 seconds");
    break;
  }
}
```

## Async tasks

### Blocking vs Non-blocking (a.k.a Asynchronous) Tasks

As we learnt in previous sessions, the JavaScript engine only provide one thread to run the event loop. We need to understand the implication of this fact.

One of the implication is if a task takes too long to finish, all the other events in the Event Queue (or task queue) cannot be processed in time, then end users may feel the UI is not responsive.

For example, if a user clicks a button and the event handler is triggered and put into the Event Queue, however, there is a long-running task that hogs the Call Stack, then the button-click event handler will not be run in time, and the user would wonder why the button does not work.

This kind of blocking behavior can be a result of CPU-intensive number-crunching task, or a result of calling some blocking API.

One typical blocking task is call synchronous I/O tasks, which happens when your application needs to read a file from hard disk (or make an HTTP request) and it waits for the result to be ready before it moves to the next step.

```js
const data = fs.readFileSync(...)
console.log(data);
```

Writing code in this kind of synchronous style is very common (you have already written some code like this in the past!), but if your code needs to be executed by JavaScript engine, it's generally a bad behavior and we should avoid it.

Most of the time, the tasks in the event queue should be finished quickly, and if a task needs to take sometime (e.g. reading a file from hard disk), the task should be executed asynchronously.

What are the ways to write asynchronous code in JS?

- callback
- promise
- async/await

We have already seen callbacks being used with `setTimeout`.
