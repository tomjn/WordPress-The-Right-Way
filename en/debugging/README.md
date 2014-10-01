# Debugging

When developing for WordPress, it's important that your code works, but when it fails it can be like a needle in a haystack. It doesn't need to be that way.

This chapter covers:

 - How to find out what errors have occurred
 - How to debug the issue
 - Plugins and tools to make your life easier
 - Features in WordPress that make debugging easier
 - How to prevent problems from occurring to begin with and easy automated tools to catch mistakes for you

But before you continue, a word on White Screens of Death

## White Screens of Death

A common issue with new WordPress developers is the white screen of death. This happens when a fatal error occurs in PHP. Many new developers respond to this by making changes and hoping the problem goes away, but there are better ways of dealing with this.

When an error occurs in PHP, it gets logged somewhere, and you can find out what went wrong and where.

A good starting point for developers is to [enable `WP_DEBUG`](wp-configphp.md).

