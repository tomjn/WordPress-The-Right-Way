# Test Driven Development

Test Driven Development also known as TDD, is a development process which takes the seemingly counter-intuitive approach of writing tests before writing the code that the tests cover.

The core idea of test driven development is to create failing tests that you are then able to compare your code against, which gives you assurance that your code at least performs the task that it was intended, based on the tests.
## Steps of the TDD Process

1. Add a test
2. Run all tests and see if the new test fails
3. Write the code
4. Run tests again to see if any tests fail
5. Refactor code

## Types of Testing

As you can gather from the list above, the steps for TDD are quite simple, at a high level. Test driven development as a theory is agnostic to the specific type of testing that you perform. All it states is that the tests should be written first, and should be written specifically for the code that you are planning to write.

### Unit Testing

When getting started in TDD, it's almost uniform that you will be introduced to unit testing first. Unit testing is the core of most people's approach to TDD, as it takes the least amount of effort to setup. For PHP, you simply need to install PHPUnit, and start writing tests. There's more of a setup process for WordPress specifically, which you can read more about on the [unit testing](unit_testing.md) pages.

## Behavior Tests / Behavior Driven Development

Behavior driven development [(Behavior Driven Development)](bdd.md) is a testing methodology that emerged from TDD, with more of a focus on using plain language to write tests. When written out, BDD tests are generally easier to read as sentences than unit tests, or integration tests.

### Integration Testing

_Need more information here_

## Parallels to the Scientific Method

If you've studied TDD for any length of time, it's hard to mistake the parallels with the scientific method. We'll describe the scientific method as making an observation, creating a hypothesis about that observation, creating an experiment to test that hypothesis, collecting and analyzing data from that experiment, and then coming to a conclusion about that hypothesis.

Each of these steps can be connected to steps of the TDD process. If you really dig into it, it becomes clear that TDD is just [software development's scientific method](https://travis-weston.medium.com/software-development-is-the-scientific-method-b5edbf6dafc0).

The scientific method is modified to fit the science. We see that in Psychiatry, Sociology, and many other sciences that don't have things to physically touch. Software development is just another one of those.
