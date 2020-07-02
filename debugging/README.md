# Debugging

When developing for WordPress, it's important that your code works, but when it fails it can be like a needle in a haystack. It doesn't need to be that way.

This chapter covers:

* How to find out what errors have occurred
* How to debug the issue
* Plugins and tools to make your life easier
* Features in WordPress that make debugging easier
* How to prevent problems from occurring to begin with and easy automated tools to catch mistakes for you

But before you continue, a word on White Screens of Death

## White Screens of Death

Since we're doing this the right way, we are using the latest version of WordPress. But still a lot of documentation mentions this White Screen of Death. So what is it? Before WordPress 5.2 WordPress would just "die" if something went wrong. The only thing that would be presented was a HTTP 500 error, accompanied by a white screen. No further information was given, unless you had debugging enabled on your website. And since that is not recommended for a production/live site (and not the default) a lot of (new) WordPress developers would have no clue on what was happening. Basically a fatal error in PHP caused the WordPress process to quit and nothing was sent to the browser.

## WordPress recovery mode

Since WordPress 5.2 a new feature has been implemented known as the "recovery mode". The release notes for this specific feature stated:
> This administrator-focused update will let you safely fix or manage fatal errors without requiring developer time. It features better handling of the so-called “white screen of death,” and a way to enter recovery mode, which pauses error-causing plugins or themes.

Essentially this mode does two things: it eliminates the white screen and substitutes it with a user friendly error message. This way you, and visitors, know that something went wrong. The second thing is that it will try (if possible) to send a e-mail to the website administrator address, with a magic link that allows you to login to the dashboard of your website to possibly fix the issues. 

For developers this doesn't change much directly: when an error occurs in PHP, it gets logged somewhere, and you can find out what went wrong and where.

A good starting point for developers is to [enable `WP_DEBUG`](wp-configphp.md).

