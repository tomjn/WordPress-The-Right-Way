# Handling Errors

While using the Core APIs, it's a good idea to check return values. For example, when creating a post, if something goes wrong you should be able to handle that outcome. Not handling errors and failures can lead to unstable code and unpredictable behavior.

## Return values

Many functions return error and success values. You should always check these values after making a call. For example `get_post_meta` returns a custom field value, but if that custom field/post meta does not exist, it returns an error value.

Different APIs return different error values, and can include:

 - `null` values
 - false
 - `WP_Error` objects

WordPress API calls at the time of writing do not throw exceptions. However if you hook into actions such as `save_post` and throw an exception, it may not be caught due to this expectation, so do not throw exceptions unless you're sure you know what you're doing.

## `WP_Error`

The `WP_Error` object is a catch all error message object returned by some APIs. It has internal storage for multiple error messages and error codes.

## `is_wp_error`

This is a helpful method to simplify error checking. It checks if a returned value was a `WP_Error` object, and also checks for a handful of other error values. It returns a true or false value, allowing checks such as these:

This is a helpful method to simplify error checking. It checks if a returned value was a `WP_Error` object, but does not check for other error values. It's shorthand for `if ( get_class( $variable ) == 'WP_Error' )`. For example:

```php
if ( !is_wp_error( $value ) ) {
    // do things
} else {
    // display a warning to the user and abort
}
```

While this is a useful function, remember, not every API returns the same error value, and you should check first.
