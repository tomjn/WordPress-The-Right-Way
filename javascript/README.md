# Javascript

Using:

## `wp_enqueue_script`
## `wp_localize_script`
## register/deregister
## `wp_script_is`

You can check if a script has been enqueued, printed, or registered by calling `wp_script_is`.

For example, here we check if fitvid has been added, and enqueued it if it hasn't:

```
$handle = 'fluidVids.js';
$list = 'enqueued';
if (wp_script_is( $handle, $list )) {
	return;
} else {
	wp_register_script( 'fluidVids.js', plugin_dir_url(__FILE__).'js/fluidvids.min.js');
	wp_enqueue_script( 'fluidVids.js' );
}
```

## `admin_enqueue_scripts`

When adding javascript to the admin area, it's tempting to use the `admin_head` action. Don't do this, instead use this hook when including javascript in the Admin area. For examples:

```
function my_enqueue($hook) {
    wp_enqueue_script( 'my_custom_script', plugin_dir_url( __FILE__ ) . 'script.js' );
}
add_action( 'admin_enqueue_scripts', 'my_enqueue' );
```

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
