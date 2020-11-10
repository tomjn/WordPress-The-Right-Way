# Test Driven Development

Test Driven Development is a development process which takes the seemingly counter-intuitive approach of writing tests before writing the code that the tests cover.

The core idea of test driven development is to create failing tests that you are then able to compare your code against, which gives you assurance that your code at least performs the task that it was intended, based on the tests.

Test driven development has been around as long as there has been software development, but [Kent Beck](https://en.wikipedia.org/wiki/Kent_Beck) is credited with *rediscovering* it in the early 2000s.

## Steps of the TDD Process

1. Add a test
1. Run all tests and see if the new test fails
1. Write the code
1. Run tests again to see if any tests fail
1. Refactor code

## Types of Testing

As you can gather from the list above, the steps for TDD are quite simple, at a high level. But, much like with everything in software and web development, when things are simple, we are very good at complicating them. As a way of complicating matters, test driven development as a theory is agnostic to the specific type of testing that you perform. All it states is that the tests should be written first, and should be written specifically for the code that you are planning to write.

### Unit Testing

When getting started in TDD, it's almost uniform that you will be introduced to unit testing first. Unit testing is the core of most people's approach to TDD, as it takes the least amount of effort to setup. For PHP, you simply need to install PHPUnit, and start writing tests. There's more of a setup process for WordPress specifically, which you can read more about on the [unit testing](unit_testing.md) pages.

## Behaviour Tests / Behaviour Driven Development

BDD [(Behaviour Driven Development)](bdd.md) could easily be considered just another form of TDD. The only real difference is _the language that is used_. BDD tools are designed to be read as plain english when written out. For example, the BDD PHP Testing tool [SpecBDD](http://www.phpspec.net/) allows you to write tests like so:

```php
<?php

namespace spec;

use PhpSpec\ObjectBehavior;

class StephenKingNovel extends ObjectBehavior
{
    public function it_should_be_scary(){
        $this->getHorrorRating()->shouldBe(5);
    }
}
```

Which looks a lot like what we can do with PHPUnit as a Unit Test:

```php
<?php

use Example\Novel;
use PHPUnit\Framework\TestCase;

class NovelTest extends TestCase
{
    public function test_stephen_king_novel_horror_rating_is_scary(): void
    {

        $novel = Novel::getByAuthor('Stephen King');
        
        $this->assertEquals(5, $novel->getHorrorRating());

    }
}
```

With PHPUnit we're telling the code that we want these two integers to be equal. In SpecBDD, we're telling the code that the horror rating should be 5. It's a subtle difference, but one could argue that the latter is easier to read. For larger projects, it might be a good idea to include both forms of tests.

### Integration Testing

_Need more information here_

## Parallels to the Scientific Method

If you've studied TDD for any length of time, it's hard to mistake the parallels with the scientific method. We'll describe the scientific method as making an observation, creating a hypothesis about that observation, creating an experiment to test that hypothesis, collecting and analyzing data from that experiment, and then coming to a conclusion about that hypothesis.

Each of these steps can be connected to steps of the TDD process. If you really dig into it, it becomes clear that TDD is just [software development's scientific method](https://travis-weston.medium.com/software-development-is-the-scientific-method-b5edbf6dafc0).

The scientific method is modified to fit the science. We see that in Psychiatry, Sociology, and many other sciences that don't have things to physically touch. Software development is just another one of those.
