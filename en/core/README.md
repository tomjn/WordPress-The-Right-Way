# Core

WordPress core is the code that powers WordPress itself. It is what you get when downloading WordPress from wordpress.org, minus the themes and plugins.

## Load Process

At the most basic, the WordPress core loading follows this pattern:

 - Load MU plugins
 - Load Activated plugins
 - load theme functions.php
 - Run init hook
 - Run main query
 - Load template

Administration and AJAX requests follow a similar but lighter process. This diagram covers the specifics:

[![WordPress Core Load](../assets/wordpress_core_load.png)](../assets/wordpress_core_load.png)

## Deregistering jQuery

Many plugin and theme developers attempt to unregister the jQuery that comes with core, and add their own copy, normally the jQuery on the Google CDN. Do not do this, it can cause compatability issues.

Instead use the copy of jQuery that comes with WordPress and aim for the version used in the latest WordPress when testing. This ensures maximum compatability across plugins.

## Modifying Core

It's tempting to modify parts of Core to remove or add things, but this must never be done. When WordPress updates, all your changes will be lost.

Instead, use Hooks/Actions and Filters to modify Core behaviour.

## Further Reading

 - [Making Sense of Core Load](http://www.rarst.net/wordpress/wordpress-core-load/)
