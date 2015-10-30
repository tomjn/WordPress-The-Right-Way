# JavaScript

WordPress comes with dependency management and enqueueing for JavaScript files. Don't use raw `<script>` tags to embed JavaScript.

## Registering and Enqueueing

JavaScript files should be registered. Registering makes the dependency manager aware of the script. To embed a script onto a page, it must be enqueued.

Let's register and enqueue a script.

```php
// Use the wp_enqueue_scripts function for registering and enqueueing scripts on the front end.
add_action( 'wp_enqueue_scripts', 'register_and_enqueue_a_script' );
function register_and_enqueue_a_script() {
	// Register a script with a handle of `my-script`
	//  + that lives inside the theme folder,
	//  + which has a dependency on jQuery,
	//  + where the UNIX timestamp of the last file change gets used as version number
	//    to prevent hardcore caching in browsers - helps with updates and during dev
	//  + which gets loaded in the footer
	wp_register_script(
		'my-script',
		get_template_directory_uri().'/js/functions.js',
		array( 'jquery' ),
		filemtime( get_template_directory().'/js/functions.js',
		true
	);
	// Enqueue the script.
	wp_enqueue_script( 'my-script' );
}
```

Scripts should only be enqueued when necessary; wrap conditionals around `wp_enqueue_script()` calls appropriately.

When enqueueing javascript in the admin interface, use the `admin_enqueue_scripts` hook.

When adding scripts to the login screen, use the `login_enqueue_scripts` hook.

## Localizing

Localizing a script allows you to pass variables from PHP into JS. This is typically used for internationalization of strings (hence localization), but there are plenty of other uses for this technique.

From a technical side, localizing a script means that there will be a new `<script>` tag added right before your registered script, that contains a _global_ JavaScript object with the name you specified during localizing (the 2nd argument). This also means that if you add another script later on, that has this script as dependency, then you will be able to use the global object there as well. WordPress resolves chained dependencies just fine.

Let's localize a script.

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
	wp_register_script(
		'my-script',
		get_template_directory_uri().'/js/functions.js',
		array( 'jquery' ),
		filemtime( get_template_directory().'/js/functions.js' ),
		true
	);
	wp_localize_script(
		'my-script',
		'scriptData',
		// This is the data, which gets sent in localized data to the script.
		array(
			'alertText' => 'Are you sure you want to do this?',
		)
	);
	wp_enqueue_script( 'my-script' );
}
```

In the javascript file, the data is available in the object name specified while localizing.

```javascript
( function( $, plugin ) {
	alert( plugin.alertText );
} )( jQuery, scriptData || {} );
```

## Deregister / Dequeueing

Scripts can be deregistered and dequeued via `wp_deregister_script()` and `wp_dequeue_script()`.

## AJAX

WordPress offers an easy server-side endpoint for AJAX calls, located in `wp-admin/admin-ajax.php`.

Let's set up a server-side AJAX handler.

```php
// Triggered for users that are logged in.
add_action( 'wp_ajax_create_new_post', 'wp_ajax_create_new_post_handler' );
// Triggered for users that are not logged in.
add_action( 'wp_ajax_nopriv_create_new_post', 'wp_ajax_create_new_post_handler' );

function wp_ajax_create_new_post_handler() {
	// This is unfiltered, not validated and non-sanitized data.
	// Prepare everything and trust no input
	$data = $_POST['data'];

	// Do things here.
	// For example: Insert or update a post
	$post_id = wp_insert_post( array(
		'post_title' => $data['title'],
	) );

	// If everything worked out, pass in any data required for your JS callback.
	// In this example, wp_insert_post() returned the ID of the newly created post
	// This adds an `exit`/`die` by itself, so no need to call it.
	if ( ! is_wp_error( $post_id ) ) {
		wp_send_json_success( array(
			'post_id' => $post_id,
		) );
	}

	// If something went wrong, the last part will be bypassed and this part can execute:
	wp_send_json_error( array(
		'post_id' => $post_id,
	) );
}


add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
    wp_register_script(
    	'my-script',
    	get_template_directory_uri().'/js/functions.js',
    	array( 'jquery' ),
    	filemtime( get_template_directory().'/js/functions.js' ),
    	true
    );
    // Send in localized data to the script.
    wp_localize_script(
    	'my-script',
    	'scriptData',
    	array(
    		'ajax_url' => admin_url( 'admin-ajax.php' ),
    	)
    );
    wp_enqueue_script( 'my-script' );
}
```

And the accompanying JavaScript:

```javascript
( function( $, plugin ) {
	$( document ).ready( function() {
		$.post(
			// Localized variable, see example below.
			plugin.ajax_url,
			{
				// The action name specified here triggers
				// the corresponding wp_ajax_* and wp_ajax_nopriv_* hooks server-side.
				action : 'create_new_post',
				// Wrap up any data required server-side in an object.
				data   : {
					title : 'Hello World'
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
	} );
} )( jQuery, scriptData || {} );
```

`ajax_url` represents the admin AJAX endpoint, which is automatically defined in admin interface page loads, but not on the front-end.

Let's localize our script to include the admin URL:

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
	wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
	// Send in localized data to the script.
	$data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
	wp_localize_script( 'my-script', 'scriptData', $data_for_script );
	wp_enqueue_script( 'my-script' );
}
```

## The JavaScript side of WP AJAX

There are several ways to go on this. The most common is to use `$.ajax()`. Of course, there are shortcuts available like `$.post()` and `$.getJSON()`.

Here's the default example.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
	"use strict";

	// Alternate solution: jQuery.ajax()
	// One can use $.post(), $.getJSON() as well
	// I prefer defered loading & promises as shown above
	$.ajax( {
		 url  : plugin.ajaxurl,
		 data : {
			action      : plugin.action,
			_ajax_nonce : plugin._ajax_nonce,
			// WordPress JS-global
			// Only set in admin
			postType     : typenow,
		 },
		 beforeSend : function( d ) {
		 	console.log( 'Before send', d );
		 }
	} )
		.done( function( response, textStatus, jqXHR ) {
			console.log( 'AJAX done', textStatus, jqXHR, jqXHR.getAllResponseHeaders() );
		} )
		.fail( function( jqXHR, textStatus, errorThrown ) {
			console.log( 'AJAX failed', jqXHR.getAllResponseHeaders(), textStatus, errorThrown );
		} )
		.then( function( jqXHR, textStatus, errorThrown ) {
			console.log( 'AJAX after finished', jqXHR, textStatus, errorThrown );
		} );
} )( jQuery, scriptData || {} );
```

Note that above example uses `_ajax_nonce` to verify the NONCE value, which you will have to set by yourself when localizing the script. Just add `'_ajax_nonce' => wp_create_nonce( "some_value" ),` to your data array. You can then add a referrer check to your PHP callback that looks like `check_ajax_referer( "some_value" )`.

## AJAX on click

Actually it's pretty simple to execute an AJAX request when some clicks (or does some other user interaction) on some element. Just wrap up your `$.ajax()` (or similar) call. You can even add a delay like you might be used to.

```javascript
$( '#' + plugin.element_name ).on( 'keyup', function( event ) {
	$.ajax( { ... etc ... } )
		.done( function( ... ) { etc }
		.fail( function( ... ) { etc }

} )
	.delay( 500 );
```

## Multiple callbacks for a single AJAX request

You might come into a situation where multiple things have to happen after an AJAX request finished. Gladly jQuery returns an object, where you can attach all of your callbacks.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
	"use strict";

	// Alternate solution: jQuery.ajax()
	// One can use $.post(), $.getJSON() as well
	// I prefer defered loading & promises as shown above
	var request = $.ajax( {
		 url  : plugin.ajaxurl,
		 data : {
			action      : plugin.action,
			_ajax_nonce : plugin._ajax_nonce,
			// WordPress JS-global
			// Only set in admin
			postType     : typenow,
		 },
		 beforeSend : function( d ) {
		 	console.log( 'Before send', d );
		 }
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #1 executed' );
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #2 executed' );
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #3 executed' );
	} )
} )( jQuery, scriptData || {} );
```

## Chaining callbacks

A common scenario (regarding how often it is needed and how easy it then is to hit the mine trap), is chaining callbacks when an AJAX request finished.

About the problem first:

> AJAX callback (A) executes
> AJAX Callback (B) doesn't know that it has to wait for (A)
> You can't see the problem in your local install as (A) is finished too fast.

The interesting question is how to wait until A is finished to then start B and its processing.

The answer is "deferred" loading and ["promises"](http://en.wikipedia.org/wiki/Futures_and_promises), also known as "futures".

Here's an example:

```javascript
( function( $, plugin ) {
    "use strict";

    $.when(
        $.ajax( {
            url :  pluginURl,
            data : { /* ... */ }
        } )
           .done( function( data ) {
                // 2nd call finished
           } )
           .fail( function( reason ) {
               console.info( reason );
           } );
    )
    // Again, you could leverage .done() as well. See jQuery docs.
    .then(
        // Success
        function( response ) {
            // Has been successful
            // In case of more then one request, both have to be successful
        },
        // Fail
        function( resons ) {
            // Has thrown an error
            // in case of multiple errors, it throws the first one
        },
    );
    //.then( /* and so on */ );
} )( jQuery, scriptData || {} );
```
[_Source: WordPress.StackExchange / Kaiser_](http://wordpress.stackexchange.com/a/118796/385)
