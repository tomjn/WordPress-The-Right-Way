# Templates

## Loading templates via `get_template_part`

Mention `locate_template` function


## How Templates are Chosen and the Template Hierarchy

Notes on how a template is chosen using the main query. How templates are chosen and loaded, how child themes are involved. Show the template hierarchy diagram

## Functions.php and Plugins

`functions.php` is a file in your theme that gets loaded prior to any templates. If your theme has non-template functionality, such as changing the length of excerpts, adding stylesheets and scripts, etc, this is where that code would go.

Because of the way functions.php is loaded, it can be considered a plugin, as there is no difference between `functions.php` and plugin development. However, there is a difference in how it's loaded.

If your theme registers post types and taxonomies, shortcodes, or widgets, this data is no longer available to the user when they change theme. This is a large problem for data portability, and can cause a persons site to become non-functional or broken.

Post types, taxonomies, shortcodes, and widgets, should be implemented in a separate plugin so that the users data remains portable, and their site is not broken when themes change. To do otherwise is irresponsible.

## Loading Stylesheets

enquing stylesheets properly

## Templates and Plugins

`template_include` filter

## Forms

Forms that submit to a separate standalone PHP file in your theme are bad. An example of how to handle a basic form submission on a page template

## Virtual Pages

For when you need a page/URL that doesn't have an associated post or archive, e.g. a shopping cart or an API endpoint.

## Further Reading

 - link to template diagram
 - interactive template diagram
