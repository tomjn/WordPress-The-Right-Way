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
