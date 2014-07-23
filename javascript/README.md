# Javascript

WordPress comes with dependency management and enqueueing for Javascript files. Don't use raw `<script>` tags to embed Javascript.

## Registering and Enqueueing

Javascript files should be registered. Registering makes the dependency manager aware of the script. To embed a script onto a page, it must be enqueued.

Let's register and enqueue a script.

```php
function register_and_enqueue_a_script() {
	// Register a script with a handle of `my-script` that lives inside the theme folder, which has a dependency on jQuery.
	wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
	// Enqueue the script.
	wp_enqueue_script( 'my-script' );
}
// Use the wp_enqueue_scripts function for registering and enqueueing scripts on the front end.
add_action( 'wp_enqueue_scripts', 'register_and_enqueue_a_script' );
```

Scripts should only be enqueued when necessary; wrap conditionals around `wp_enqueue_script()` calls appropriately.

When enqueueing javascript in the admin interface, use the `admin_enqueue_scripts` hook.

When adding scripts to the login screen, use the `login_enqueue_scripts` hook.

## Localizing

Localizing a script allows you to pass variables from PHP into JS. This is typically used for internationalization of strings (hence localization), but there are plenty of other uses for this technique.

Let's localize a script.

```php
function register_localize_and_enqueue_a_script() {
	wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
	// Send in localized data to the script.
	$data_for_script = array( 'alertText' => __( 'Are you sure you want to do this?' ) );
	wp_localize_script( 'my-script', 'scriptData', $data_for_script );
	wp_enqueue_script( 'my-script' );
}
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
```

In the javascript file, the data is available in the object name specified while localizing.

```javascript
( function( $ ) {
	alert( scriptData.alertText );
} )( jQuery );
```

## Deregister / Dequeueing

Scripts can be deregistered and dequeued via `wp_deregister_script()` and `wp_dequeue_script()`.

## AJAX

[missing documentation]