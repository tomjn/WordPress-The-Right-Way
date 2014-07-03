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

A note on the domain mapping plugin
