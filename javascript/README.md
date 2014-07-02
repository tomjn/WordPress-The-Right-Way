# Javascript

Using:

## `wp_enqueue_script`
## `wp_localize_script`
## register/deregister
## `wp_script_is`
## `admin_enqueue_scripts`
## `wp_enqueue_media`
## `login_enqueue_scripts`

When adding styles and scripts to the login screen, use the `login_enqueue_scripts` hook to add them. For example:

```
function themeslug_enqueue_style() {
	wp_enqueue_style( 'core', 'style.css', false ); 
}

function themeslug_enqueue_script() {
	wp_enqueue_script( 'my-js', 'filename.js', false );
}

add_action( 'login_enqueue_scripts', 'themeslug_enqueue_style', 10 );
add_action( 'login_enqueue_scripts', 'themeslug_enqueue_script', 1 );
```

## AJAX

List of all the built in scripts that can be used (jQuery, etc, etc)

Maybe a bit about backbone.
