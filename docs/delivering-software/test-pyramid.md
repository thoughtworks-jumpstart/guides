# Test pyramid

## Different types of testing

Unit Testing - validate that each unit of the software performs as expected

Integration Testing - ensure that the units work good together

Unit testing without integration testing:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/mC3KO47tuG0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

User Interface Testing (End-to-end (E2E) Testing) - test the application interfaces from the user's perspective

Regression Testing - verify that a previously developed and tested software hasn't regressed. (Snapshot testing is a kind of regression testing)

Manual testing - verify by hand that the software works/looks as you expect. This is time-consuming, non-repeatable and unscalable type of testing, and should be the least in proportion to the other types of tests

## More tests = better? no

On almost all of the projects, we have limited time and budget, and we don't have a lot of time to spend on writing and maintaining tests. So sometimes we need to make a choice on what kind of tests to write and maintain by considering the cost and benefits of those tests.

Also, tests take time to run. We want to be able to run tests quickly and often so that we can check that our code is passing the tests, especially in continuous integration.

The test pyramid is a model that helps illustrate this.

We should write the least number of tests that cover the most amount of code / functionality.

![software testing pyramid](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LBJBWvA1rRm3yAXiRPh%2F-LBJBezpu28QsXc1lR9n%2Ftesting_pyramid.jpg?generation=1525052257824511&alt=media)

## Unit Test vs Integration Test / Contract Test

### Unit tests

Usually we prefer to have a lot of unit tests because they are easier to write, fast to run and easier to troubleshoot when they fail. However, one challenge of writing unit tests is to isolate the function/class under test from their environment.

Typically this requires us to mock the dependencies, but too many mock objects lead to its own problems.

Setting up mock objects for each test case could be tedious and makes the test cases harder to read.

Setting up mock objects sometimes requires knowledge on internal implementation of the functions under test (e.g. the implementation depends on some global object that is not passed as function arguments). If a test case is tightly coupled with the implementation, then we are forced to change the test case when implementation changes.

One way to address this concern is to follow dependency injection approach, by declaring all dependencies in the function arguments or constructor arguments.

### Integration tests

Usually we prefer to have fewer integration tests, especially those that depends on external systems (e.g. dependency on a real database or another web service). Because of those external dependencies, it could be difficult to setup the test environment, and the test cases may fail because of external reasons.

Having said that, one type of integration test for a web service is very helpful: the Contract Test. These test cases basically define how the APIs should behave under each scenario. Whenever we make new changes, as long as all contract tests pass, we can be pretty confident that the API consumers are not affected by the change.

When you build a system of multiple web services (e.g. using the micro-service architecture), it's critical that each service has its own API contract test so that every service can be released/upgraded independently and be confident that their changes do not break existing API consumers. In that scenario, the contract tests can also be written as Consumer Driven Contracts.
