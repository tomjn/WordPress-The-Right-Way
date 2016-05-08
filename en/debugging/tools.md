# Tools

Debugging tools fall into two categories:

 - Tools to diagnose issues when they arise and reveal problems
 - Tools that prevent mistakes and errors from ever happening to begin with

The age old adage still applies: **prevention is better than cure**

## Debugging Tools / Plugins

Installing the plugin [Developer](https://wordpress.org/plugins/developer/) from the WordPress repository will give you quick access to a broad range of debugging tools. The following debugging plugins are quite useful:

 - [Log Deprecated Notices](https://wordpress.org/plugins/log-deprecated-notices/) Logs usage of deprecated functions.
 - [Debug Bar](http://wordpress.org/plugins/debug-bar) Provides an interface for debugging PHP Notices/Warnings/Errors, reviewing SQL Queries, analysing caching behaviour and much more. It's also extendable with plugins.
 - [Debug Console](https://wordpress.org/plugins/debug-bar-console/) The Debug Console for example is really useful.
 - [Query Monitor](https://wordpress.org/plugins/query-monitor/) View debugging and performance information on database queries, hooks, conditionals, HTTP requests, redirects and more.

### Xdebug and Remote Debugging
The [Xdebug](http://xdebug.org/index.php) PHP Extension allows for enhanced debugging, function and method tracing, and profiling of PHP applications. This is [installed with VVV and can be turned on/off](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Code-Debugging#meet-xdebug).

With [PHPStorm](https://www.jetbrains.com/phpstorm/), you can install a browser extension to access Xdebug (or [Zend Debugger](http://files.zend.com/help/Zend-Studio/zend-studio.htm#debugging_php_in_zend_studio.htm)) from within the IDE.

Rather than manually adding `var_dump` statements and reloading the page, you can add a breakpoint anywhere in your PHP code, execution will stop and you can see a stack trace, inspect (and modify) the values of all variables and objects or manually evaluate (test) a PHP expression.

With [zero-configuration debugging](http://confluence.jetbrains.com/display/PhpStorm/Zero-configuration+Web+Application+Debugging+with+Xdebug+and+PhpStorm) (controlled via cookies and bookmarklets) you don't need to add `?XDEBUG_SESSION_START` to your URLs and you can also debug HTTP post requests.

### PHP Debuggers
- [DBG](http://www.php-debugger.com/) - PHP Debugger and Profiler

### Browser Web Inspectors
- [Chrome DevTools](http://discover-devtools.codeschool.com/) for Google Chrome
- [Firebug](http://getfirebug.com/) for Mozilla Firefox
- [F12 developer tools](http://msdn.microsoft.com/library/ie/bg182326) for Internet Explorer
- [Opera Dragonfly](http://www.opera.com/dragonfly/) for Opera

## Prevention

There are a number of tools dedicated to analysing code and catching semantic mistakes, or pointing out problems in code.

[PHP Mess Detector](http://phpmd.org/) for example, will highlight long variable names, npath and cyclomatic complexity, classes that are too large, unused variables, and other problems. [SCheck](https://github.com/facebook/pfff/wiki/Scheck) is a tool provided by Facebook, and performs similar checks, such as finding dead statements and unused classes.

If you can't type hint, you can make use of a tool such as [phantm](https://github.com/colder/phantm/) to infer types and find clashes. Many others exist though, and integrate with your editor/IDE, so look around
