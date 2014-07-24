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
( function($) {
	alert( scriptData.alertText );
} )(jQuery);
```

## Deregister / Dequeueing

Scripts can be deregistered and dequeued via `wp_deregister_script()` and `wp_dequeue_script()`.

## AJAX

WordPress offers an easy server-side endpoint for AJAX calls, located in `wp-admin/admin-ajax.php`.

Let's set up a server-side AJAX handler.

```php
function wp_ajax_create_new_post_handler() {
	// Do things here.
	$post_id = wp_insert_post( array(
		'post_title' => $_REQUEST['data']['title']
	));

	// If everything worked out, pass in any data required for your JS callback:
	if ( ! is_wp_error( $post_id ) ) {
		wp_send_json_success( array( 'post_id' => $post_id ) );

	// If something went wrong:
	} else {
		wp_send_json_error( array( 'post_id' => $post_id ) );
	}
}
// Triggered for users that are logged in.
add_action( 'wp_ajax_create_new_post', 'wp_ajax_create_new_post_handler' );
// Triggered for users that are not logged in.
add_action( 'wp_ajax_nopriv_create_new_post', 'wp_ajax_create_new_post_handler' );

function register_localize_and_enqueue_a_script() {
    wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
    // Send in localized data to the script.
    $data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
    wp_localize_script( 'my-script', 'scriptData', $data_for_script );
    wp_enqueue_script( 'my-script' );
}
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
```

And the accompanying Javascript:

```javascript
(function($) {
	$(document).ready( function() {
		$.post(
			// Localized variable, see example below.
			scriptData.ajax_url,
			{
				// The action name specified here triggers the corresponding wp_ajax_* and wp_ajax_nopriv_* hooks server-side.
				action: 'create_new_post',
				// Wrap up any data required server-side in an object.
				data: {
					title: 'Hello World'
				}
			},
			function( response ) {
				// wp_send_json_success() sets the success property to true.
				if ( response.success ) {
					// Any data that passed to wp_send_json_success() is available in the data property
					alert( 'A post was created with an ID of ' + response.data.post_id );

				// wp_send_json_error() sets the success property to false.
				} else {
					alert( 'There was a problem creating a new post.' );
				}
			}
		);
	});
})(jQuery);
```

`ajax_url` represents the admin AJAX endpoint, which is automatically defined in admin interface page loads, but not on the front-end.

Let's localize our script to include the admin URL:

```php
function register_localize_and_enqueue_a_script() {
	wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
	// Send in localized data to the script.
	$data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
	wp_localize_script( 'my-script', 'scriptData', $data_for_script );
	wp_enqueue_script( 'my-script' );
}
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
```