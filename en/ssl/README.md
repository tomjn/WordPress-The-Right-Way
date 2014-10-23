# Security

## Salts

 - A mention on rainbow tables and why salts are necessary and their purpose
 - The keys and salts in `wp-config.php`
 - The API wordpress.org provides for adding them

## Nonces

 - Not the British variety
 - Try to explain Cross site reference attacks
 - Add notes on how to use nonces effectively

## The Location of `wp-config`

 - You can move it one level up so it's not in a web accessible location

## Table prefixes

 - Don't use the default wp_
 - Notes on automated attacks

## User ID 1

 - Don't call it 'admin'
 - Don't give it administrator privilledges

## Roles and Capabilities

 - What they are

## Removing vs Hiding Settings Pages

 - Hiding things with CSS doesn't make it secure
 - People have dev tools too
 - Automated tools ignore CSS
 - how to remove admin menus and change the capabilities needed to do things

## Custom Password Reset Code

 - Some people write their own password reset facilities
 - This is bad
 - If you really must, make it a forgotten password link, don't make it actually show your password

## timthumb.php

 - Don't use it
 - There's an image API for that
 - timthumb was disowned by its creators and is officially no longer supported
 - Banned on a number of managed WordPress hosts

## SSL

All big clients deserve an SSL certificate. If you're running an e-commerce site, this is especially true, and your entire site should be SSL for all logged in users.

 - A note on public wifi, unsecured wifi, and snooping
 - Maybe mention firesheep?

### Admin Only SSL

 - If your site isn't an ecommerce site, but you have users who visit the backend, their logged in sessions should be sent over a https connection.
 - Explain how

## Myths

There are a lot of feel good security fixes that float around, that do nothing to help your security, waste your time, and sometimes increase the risk. Here are a few

### Hiding the Admin and Login URLs

 - Some people try to change the admin and login URLs in hopes it will fool attackers and automated tools
 - WordPress adds in /admin/ and /login/ rewrite rules in the newer versions so moving the files is pointless
 - It can break some functionality in code without necessary care
 - Trying to go to the admin URLs will redirect you to the changed login URL anyway, and if you fix that then the modal box that shows in the admin screen when your session expires will be broken too

### Deactivated Plugins & Themes

 - Because of how PHP works, deactivated plugins can still be hit from a users web browser
 - Badly written plugins might do things if the right URL is loaded, even if they're not activated. This is especially true of plugins with their own AJAX endpoints that don't use the WP AJAX API.

## Recovering From Attacks

 - Take and use regular backups
 - Download a fresh copy of WordPress and extract it over the top of your existing install to make sure that WP Core is unmodified
 - Check your plugins and code against version control
