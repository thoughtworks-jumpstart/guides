# Test-driven development

## What are the benefits of Test Driven Development (TDD)

In TDD, as its name implies, we first start by writing tests and only when it fails, then we are allowed to switch to the component code to develop. Once the test passes, we must switch back to writing more tests before we can write more component code.

In this way, TDD ensures that only essential code is written, and it would also be impossible to have a feature that does not have a corresponding test.

In the TDD process we would :

- Run all tests in watch mode, and ensure that all are passing (Green) before we begin
- Create test case for the component we wish to build
- Render the component in the test, and watch the test fail (Red)
- Go ahead to create the component file and watch the test pass (Green)
- Switch back to the test and create an expect/assertion and watch the test fail (Red)
- Develop the simplest code you can do to make the test pass (Green)
- Refactor the code if it is not clean (Refactor)
- Add a new test with another expect/assertion and watch the test fail (Red)
- Develop the simplest code you can do to make the test pass again (Green)
- Keep repeating the cycle till the component is fully tested and developed (Refactor)

## FAQ

1. In the Labs we run only the test which we are developing, while in TDD we run all tests. Why is that so?

   Ans: In the first workflow we run all the tests when we are developing, but once the development phase is over, we no longer touch the App code. Hence we can choose to run only the test we are developing, so that there is less noise on the console

   In TDD we are developing all the time. We need to run all tests as a safety net to help us detect when we have broken something.

2. I have heard about TDD and the term Red - Green - Refactor. How are these related?

   Ans: **Red - Green - Refactor** is describing the process of writing a test and letting it fail (Red), writing the code to pass the test (Green), and looking at the code from the previous cycle and the current cycle, and cleaning up if that is required (Refactor)

3. Can you explain what is the Refactor step and why do we need to do this ?

   Ans: Since TDD consists of many cycles of code change, rather than a upfront design of what you wish to code. As the code from the cycles accumulate, reduandant code could build up. eg: Code repetition, bad naming of variables or functions etc. If this is not cleaned up, it could become a tech debt, and make the code hard to maintain in the future.

   Hence the Refactor steps means to clean the code so that it has the same behaviour as in the Green stage but is shorter and more readable, and has no repetition.
