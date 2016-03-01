# Code Style Guide

## Clean Code

It's important to keep code readable and maintainable. This prevents small but critical errors from becoming hidden in your code, while making whole classes of bugs incredibly obvious ( missing closing braces are easy to spot when you indent consistently ).

While it's best to use the same standard as everybody else, if you're more comfortable using a PSR standard, then use that. If you do though, do it consistently.

### Indenting

Indenting in WordPress is done using tabs, representing 4 spaces visually. Indenting is important for readable code, and each statement should be on its own line. Without indenting, it becomes very difficult to understand what's happening, and mistakes are easier to make. This also makes support requests on the forums and stack exchange difficult to answer.

A good editor will auto-indent for you, most can re-indent a file if you've older code that needs fixing.

A good way to ensure that all members on a team are using the same
styles is to use [Editor Config](http://editorconfig.org). It contains
plugins for different editors, so everyone can use their favorite
editor.

For instance, the following `.editorconfig` file enforces the above
rule, indentation as tabs of width 4 spaces.

```ini
[*.php]
indent_style = tabs
indent_size = 4
```

### PHP Tag Spam

The `<?php` and `?>` tags should be used sparingly. For example:

```php
<?php while( have_posts() ) { ?>
    <?php the_post(); ?>
    <?php the_title(); ?>
    <strong><?php the_date(); ?></strong>
    <?php the_content(); ?>
<?php } ?>
```
Would be easier to read as:

```php
<?php
while( have_posts() ) {
    the_post();
    the_title();
    ?>
    <strong><?php the_date(); ?></strong>
    <?php
    the_content();
} ?>
```

A good guideline is to calculate what needs to be displayed, then display it all in one go rather than mixing the two.

### Linting

A lot of editors support or have built in syntax checkers. These are called Linters. When using a good editor, syntax errors are highlighted or pointed out.

For example, in PHPStorm, a syntax error is given a red underline.

## Coding Standards

WordPress follows a set of coding standards. These differ from the PSR standards. For example, WordPress uses tabs rather than spaces, and places the opening bracket on the same line.

The WordPress Contributor Handbook covers the coding standards in more details. Click below to read more:

- [HTML Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/html/)

- [PHP Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/php/)

- [JavaScript Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/javascript/)

- [CSS Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/css/)


#### PHP Code Sniffer & PHP CS Fixer

PHP Code Sniffer is a tool that finds violations of the coding standard. Many editors integrate support, including support for a second tool that fixes those violations automatically.

To use this, you will need the [WordPress Coding Standards definition](https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards).
