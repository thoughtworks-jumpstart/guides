# Errors

## Reporting abnormal scenarios by throwing errors

If your program encounter a situation that it cannot handle, you can throw errors to report the situation.

In the example below, an error is thrown when the given divider is zero.

```js
function divide(dividend, divisor) {
  if (divisor === 0) {
    throw new Error("The divisor cannot be zero");
  }
  // continue to check other cases to handle
}
```

## Handling errors with catch

When you call a function that may throw errors, and if you have a proper way to handle those errors, then you can choose to catch some of them.

```js
try {
  loadConfiguratoniFile(filePath);
} catch (error) {
  if (error.message === "file not found") {
    // use default configuration
    return defaultConfiguration;
  } else {
    // re-throw the error if we cannot handle it
    throw error;
  }
}
```

Always check the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) if you are unsure.

## Recommended readings

https://frontarm.com/james-k-nelson/will-finally-run-quiz/
