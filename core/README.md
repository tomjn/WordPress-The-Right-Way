# Core

WordPress core is the code that powers WordPress itself. It is what you get when downloading WordPress from wordpress.org, minus the themes and plugins.

## Load Process

todo: add a note on the core load process, and references Rarsts blog post

[![WordPress Core Load](../assets/wordpress_core_load.png)](../assets/wordpress_core_load.png)

## Deregistering jQuery

Many plugin & theme developers attempt to unregister the jQuery that comes with core, and add their own copy, normally the jQuery on the Google CDN. Do not do this, it can cause compatability issues.

Instead use the copy of jQuery that comes with WordPress and aim for the version used in the latest WordPress when testing. This ensures maximum compatability across plugins.

## Modifying Core

It's tempting to modify parts of Core to remove or add things, but this must never be done. When WordPress updates, all your changes will be lost.

Instead, use Hooks/Actions and Filters to modify Core behaviour.

## How core development works on .org

 - trunk
 - develop
 - grunt builds develop into trunk
 - Trac
 - code freeze
 - release
