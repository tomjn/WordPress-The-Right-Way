# Standalone PHP Files

You may be tempted to add PHP files to your theme or plugin that handle AJAX requests, process forms, or generate stylesheets on the fly. This is bad, and a very large security hole on your site.

For a lot of higher end agencies in the WP world this is a major red flag in any interview, and a sign of poor code quality in code reviews. A bad codesmell that points to this is bootstrapping WordPress via `wp-blog-header.php`.

Standalone PHP files carry a number of problems, here are a small few:

 - They're fragile, and rely on being in a specific folder. Moving functionality from a plugin to an mu-plugin, or a theme, would cripple the code.
 - They're always active, even if the theme or plugin aren't. For example, on a multisite such a file can be ran even if it's in the theme for an unrelated site. The only way to prevent it from being ran is to remove it entirely from the site
 - AJAX handlers built this way need to perform all the security checking themselves, making it easy to slip up and make mistakes
 - These files cannot be filtered or rewritten by the normal filters and hooks available in WP, so they may not work with caching plugins
 - These files expose the full URL path of a plugin or theme

A good example of this going wrong is `timthumb.php`. A popular image resizing script that was a standalone file. It was exploited to upload malware, causing widespread damage.

## The Solution

So what should you do instead?

### Form Handling

You don't need to pass a URL to the `action` parameter, just leave its value blank or remove it completely. Then you can use the same code to handle the form by adding a hidden input. For example:

```html
<input type="hidden" name="my_form" value="true" />
```

```php
$show_form = false;
if ( !empty( $_POST['my_form'] ) ) {
	// handle the form
	...
	// if succesful
	get_template_part( 'form', 'success' );
} else {
	get_template_part( 'form', 'page1' );
}
```

### AJAX

Use the REST API. It lets you specify all the valid parameters, and it will check if the user is logged in for you if requested. It even gives error messages in human readable language, with pretty URLs.

Here's an example REST API endpoint:

```php
add_action( 'rest_api_init', function () {
        register_rest_route( 'test/v1', '/test/', array(
                'methods' => 'GET',
                'callback' => 'tomjn_rest_test'
        ) );
} );
function rest_test() {
        return "test data";
}
```

Now I can make AJAX requests to `example.com/wp-json/test/v1/test`, and it will respond with JSON `"test data"`.

### Dynamic Stylesheet Generation

There are several options here:

 - Pre-build your stylesheets using webpack locally
 - Use `wp_add_inline_style`
 - Register a rewrite rule to trigger stylesheet output


