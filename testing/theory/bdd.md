# Behaviour Driven Development

Behaviour Driven Development is a form of test driven development that uses software api's organized as plain language to write tests that can be read like sentences.

Let's take our example below. Read over the code, and see if you can understand what is going on:

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

Have you figured it out yet?

Well, I hope so. It's written in plain english. Stephen King novels should be scary, in our system scary is determined by the `getHorrorRating` method, with a max rating of 5. We are testing here that Stephen King novels have a horror rating of 5, which should be the case.

This is such a simple example of BDD, but you can do much more with it. Imagine expanding this to your WordPress plugin or theme development. How would you incorporate BDD into your testing suite?

## Tools for BDD
* [PHPSpec](http://phpspec.net/)