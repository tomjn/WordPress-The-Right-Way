# Security

## Salts

Your passwords and cookies are stored with salts applied. Salts are strings of data that are kept secret, and hashed together with important data so that it's harder to guess. This way a hacker can't just run through every password and generate a rainbow table of all possible results and brute force every website. Instead they need to generate a new table for every site they target after acquiring the secret salts used. There is an API to provide salts and secret keys at wordpress.org, which you can then copy paste into your `wp-config.php`.

## Escaping

When outputting data, you should escape it. For example, if you output a css class, you should use `esc_attr`, otherwise, an attacker could sneak in the value `classname"><script>alert('hello');</script><span` and run arbitrary code on your site.

An important part of escaping however, is to escape as late as possible. If you escape a variable once, then use it 5 times, that variable may be modified at any point between escaping and output, so always escape at the moment of output.

 - Sanitise early
 - Escape Late
 - Escape Often

## Nonces

In the days of MySpace, a user could add an image to their profile, and set the `src` tag as `/logout.php`. Any user who visited their profile would be immediatley logged out. This is an example of a CSRF attack or Cross Site Reference attack.

In order to get around this, we use nonces. Nonces are small tokens that can be passed around to validate an action. For example, a form may contain a nonce, which is then checked for when processed. This makes sure that all form submissions came from the form, and not a malicious or unintended script.

@todo: Add notes on how to use nonces effectively

*Note:* In the United Kingdom, a nonce is a name for a child sex offender, be careful of using the word out of context

## The Location of `wp-config`

 - You can move it one level up so it's not in a web accessible location

## Table prefixes

 - Don't use the default wp_
 - Notes on automated attacks

## User ID 1

 - Don't call it 'admin'
 - Don't give it administrator priviledges

## Roles and Capabilities

 - What they are

## Removing vs Hiding Settings Pages

 - Hiding things with CSS doesn't make it secure
 - People have dev tools too
 - Automated tools ignore CSS
 - how to remove admin menus and change the capabilities needed to do things

## Custom Password Reset Code

 - Some people write their own password reset facilities. This is bad
 - If you really must, make it a forgotten password link, don't make it actually show your password

## timthumb.php

 - Don't use it
 - There's an image API for that
 - timthumb was disowned by its creators and is officially no longer supported
 - Banned on a number of managed WordPress hosts

## SSL

All big clients deserve an SSL certificate. If you're running an e-commerce site, this is especially true, and your entire site should be using SSL for all logged in users.

 - A note on public wifi, unsecured wifi, and snooping
 - Maybe mention firesheep?

### Admin Only SSL

 - If your site isn't an ecommerce site, but you have users who visit the backend, their logged in sessions should be sent over an https connection.
 - Explain how

## Myths

There are a lot of feel good security fixes that float around, that do nothing to help your security, waste your time, and sometimes increase the risk. Here are a few:

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
