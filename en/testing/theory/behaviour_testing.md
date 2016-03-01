# Behaviour Testing

Also known as Behaviour Driven Development, this kind of testing tests the entire stack. For example a behavioral test may start by visiting a webpage, clicking a button, and checking that an expected string was found.

BDD is good for testing business requirements. It generally falls into 2 types, story based testing, and code-based testing. An example of story based testing would be Behat, which uses a human readable format so that clients can read the tests in plain English ( with support for other languages included ).

A major benefit of these types of tests is that the tests themselves do not need to load the WordPress PHP environment. A test site can be put up on a server, and the tests can be pointed at the test site. Tools such as Behat then run as if they were a user controlling a browser ( which is exactly how most Behat Mink tests work ). This makes it one of the easiest ways to introduce testing, and the easiest to learn first

# Tools for Behaviour Testing

 - [http://behat.org/Behat](http://behat.org/)
 - [SpecBDD](http://www.phpspec.net/docs/introduction.html)
