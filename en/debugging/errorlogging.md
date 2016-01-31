# Error Logging

There are several kinds of error logging, but the most basic are:

 - Displaying errors on the frontend
 - Writing errors to a log file
 - Not displaying anything at all

In a production/live environment, you want to write errors to a log file.

## Warnings vs Errors

Depending on how PHP is configured, warnings will also be shown. A warning is something that does not stop PHP from running but indicates a problem might have occurred. For example:

```php
$my_array = array(
    'alice' => 5,
    'bob' => 6
);
echo $my_array['eve'];
```

Here, I am echoing the 'eve' entry in `$my_array`, but there is no such entry. PHP responds by creating an empty value and logging a warning. Warnings are indicators of bugs and mistakes.

## PHP Error Reporting

Depending on what was defined in your `php.ini`, PHP will have an error reporting level. Everything below that level will be ignored or considered a warning. Everything above it will be considered an error. This can vary from server to server.

## The `@` operator

Never use the `@` operator. It's used to hide errors and warnings in code, but it doesn't do what people expect it to do.

`@` works by setting the error reporting level on a command so that no error is logged. It doesn't prevent the error from happening, which is what people expect it to do. This can mean fatal errors are not caught or logged. Avoid using the `@` operator, and treat all instances of it with suspicion.
