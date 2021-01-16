# Unit Testing

Unit testing tests individual components. Each test runs in isolation, and tests only a single item, such as a function or method. This function or method is considered the _unit_. 

An example of a unit test is shown below. This test is describing a system where we may have a database model that represents information about various novels. We are testing to verify that the `getHorrorRating` method of the model works correctly. This method should return an integer between 0 and 5 describing how high that novel rated in the "Horror" genre.

As our example, we pull a Stephen King novel, as King is largely known as a Horror author.

```php
<?php

use Example\Assertions;
use PHPUnit\Framework\TestCase;

class AssertionsTest extends TestCase {
    public function test_get_number_returns_5(): void {
        $assertions = new Assertions();
        $this->assertEquals(5, $assertions->getNumber());
    }
}
```

The test here is limited to the single line with the assertion. Everything else is setup.

## Tools for Unit Testing

* [PHPUnit](https://phpunit.de/)

## Helpful Projects and Further Reading

* [John P Blochs WP Unit Test Starter project](https://github.com/johnpbloch/wp-unit-test-project)
* [WP Mock](https://github.com/10up/wp_mock)
* [Writing Unit Tests for WordPress](http://greg.harmsboone.org/blog/2014/01/01/writing-unit-tests-for-wordpress)
