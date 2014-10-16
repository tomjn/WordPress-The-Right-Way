# Multisite

## Grabbing Data From Another Blog in a Network

Can be done but is expensive

```php
switch_to_blog( $blog_id );
// Do something
restore_current_blog();
```

`restore_current_blog` undos the last call to `switch_to_blog`, but only by one step, so always call `restore_current_blog` before calling `switch_to_blog` again

## Listing Blogs in a Network

Doable but doesn't scale for big networks and is expensive

## Domain Mapping

WordPress support on default Domain Mapping inside the Multisite Solution. You can add the domain value on the site settings and it works. This default solution works fine, also on an Multinetwork installation.

Often is it helpful - not necessary, that to set the `COOKIE_DOMAIN` constant to an empty string in your `wp-config.php`:

 `define('COOKIE_DOMAIN', '');`
 
Otherwise is it possible, that WordPress will always set it to your network’s `$current_site->domain` and you won’t be able to login into any of the other sites.

But if you will run your Multisite with Alias, more than one domain, then is the core not sufficient. For this goal do you need a plugin, that enhance the functionality of the WordPress Core. Currently two examples, that will work.

 * [Mercator - WordPress multisite domain mapping for the modern era.](https://github.com/humanmade/Mercator)
 * [WordPress MU Domain Mapping - Map any blog/site on a WordPressMU or WordPress 3.X network to an external domain.](https://wordpress.org/plugins/wordpress-mu-domain-mapping/)
