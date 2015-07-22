# Templates

## Loading templates via `get_template_part`

When including templates in your theme, it's tempting to use code such as this:

```php
include( 'customloop.php' );
```

However, doing this breaks support for child themes. Instead using `get_template_part` will do the job better, while giving extra flexibility. For example:

```php
get_template_part( 'custom', 'loop' );
```

This way WordPress will attempt to load `custom-loop.php`. If the file does not exist, it will load `custom.php`, and if a child theme exists, it will load the child theme version of the file.

This allows fallback templates and specialised templates based on post meta and other data such as post type. For example:

```php
get_template_part( 'loop', get_post_type() );
```

This will load `loop.php`, but if a custom version of the template exists for that post type, it will load that instead. e.g. `loop-page.php`

Internally, `get_template_part` uses the `locate_template` function. This function finds the appropriate file, and returns its name. This is useful for finding a template, without loading it.

`locate_template is also useful for plugin theming. For example:

```php
// if the theme has a custom template for my plugin
if ( locate_template( 'mycustomplugin.php') != '' ) {
	// load the custom template for my plugin from the theme
	get_template_part( 'mycustomplugin.php' );
} else {
	// fallback to the plugins default theme
	include( 'defaulttemplates/mycustomplugin.php' );
}
```

If you wish to provide such a system in your plugin though, it's advised you use the `template_include` filter. Scroll down for a more in depth look at the `template_include` filter.

## How Templates are Chosen, and The Template Hierarchy

 - Notes on how a template is chosen using the main query. How templates are chosen and loaded, how child themes are involved. Show the template hierarchy diagram

## Functions.php and Plugins

`functions.php` is a file in your theme that gets loaded prior to any templates. If your theme has non-template functionality, such as changing the length of excerpts, adding stylesheets and scripts, etc, this is where that code would go.

Because of the way functions.php is loaded, it can be considered a plugin, as there is no difference between `functions.php` and plugin development. However, there is a difference in how it's loaded.

If your theme registers post types and taxonomies, shortcodes, or widgets, this data is no longer available to the user when they change theme. This is a large problem for data portability, and can cause a persons site to become non-functional or broken.

Post types, taxonomies, shortcodes, and widgets, should be implemented in a separate plugin so that the users data remains portable, and their site is not broken when themes change. To do otherwise is irresponsible.

## Loading Stylesheets

 - enquing stylesheets properly

## Templates and Plugins

 - `template_include` filter

## Forms

 - Forms that submit to a separate standalone PHP file in your theme are bad. An example of how to handle a basic form submission on a page template

## Virtual Pages

 - For when you need a page/URL that doesn't have an associated post or archive, e.g. a shopping cart or an API endpoint.

## Further Reading

 - link to template diagram
 - interactive template diagram
