# Unit Testing

Unit testing tests individual components. Each test runs in isolation, and tests only a single item, such as a function or method. If a unit test involves multiple interacting objects, then you have written an integration test.

For example, if I have this function:

```php
function add( $a, $b ) {
  return $a + $b;
}
```

A unit test might check:

 - If 5+5 = 10
 - That 2+3 is not 7
 - That 0+0 does not fail

# Tools for Unit Testing

 - [PHPUnit](https://phpunit.de/)
 - [PHPSpec](http://phpspec.net/)

# Helpful Projects and Further Reading

 - [John P Blochs WP Unit Test Starter project](https://github.com/johnpbloch/wp-unit-test-project)
 - [WP Mock](https://github.com/10up/wp_mock)
 - [Writing Unit Tests for WordPress](http://greg.harmsboone.org/blog/2014/01/01/writing-unit-tests-for-wordpress)
