# Multisite

## Grabbing Data From Another Blog in a Network

Getting data from another blog on the same multisite install can be done. Some people use SQL commands to do this, but this can be slow, and error prone.

Although it's an inherently expensive operation, you can make use of `switch_to_blog` and `restore_current_blog` to make it easier, while using the standard WordPress APIs.

```php
switch_to_blog( $blog_id );
// Do something
restore_current_blog();
```

`restore_current_blog` undos the last call to `switch_to_blog`, but only by one step, the calls are not nestable, so always call `restore_current_blog` before calling `switch_to_blog` again.

## Listing Blogs in a Network

Doable but doesn't scale for big networks and is expensive

## Domain Mapping

Domain mapping allows a blog on a multisite install to serve from any domain name. This way a blog does not have to be a subdirectory of the main install, or a subdomain.

Often is it helpful - but not necessary, to set the `COOKIE_DOMAIN` constant to an empty string in your `wp-config.php`:

 `define('COOKIE_DOMAIN', '');`

Otherwise WordPress will always set it to your network’s `$current_site->domain`, which could cause issues in some situations.

WordPress Core hopes to provide Domain Mapping in the future, but until then you can make use of one of the following plugins:

 * [Mercator - WordPress multisite domain mapping for the modern era.](https://github.com/humanmade/Mercator)
 * [WordPress MU Domain Mapping - Map any blog/site on a WordPressMU or WordPress 3.X network to an external domain.](https://wordpress.org/plugins/wordpress-mu-domain-mapping/)
