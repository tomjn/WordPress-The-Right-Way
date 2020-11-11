# Testing WordPress Plugins

There are any number of ways to unit test WordPress plugins, but the _right_ way would be the _WordPress_ way. For that, we're talking a simple 3 step process:

1. [Installing WP-CLI](#installing-wp-cli)
2. [Scaffolding your Plugin](#scaffolding-your-plugin)
3. [Writing your tests](#writing-your-tests)

## Installing WP-CLI

[WP-CLI](https://wp-cli.org/) provides a command line interface for WordPress. It's a simple PHP Phar file which can be downloaded and installed on most *nix systems (with limited support for Windows). You can install it from terminal using the following commands. On Windows, you can perform all of these steps on WSL2-based *nix systems without issue.

```bash
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```

## Install a local WordPress repository

WP-CLI requires an installed WordPress repository to perform the next step. When working on developing WordPress plugins and themes, it's always best to have as many WordPress versions as possible. For that purpose, it's best to use the WordPress Git repository.

```bash
git clone git@github.com:wordpress/wordpress
```

By using the above command to clone the WordPress Github repository, you will be able to run the bleeding edge version of WordPress locally, and test your plugins against _any_ version of WordPress core.

## Scaffolding your Plugin

Once WP-CLI and WordPress are installed, you will be able to use WP-CLI to scaffold your plugins. This scaffolding provides you with a base level for your plugins, including everything that you will need to get started with properly testing your plugins with phpunit. There are two main commands. The first is focused on scaffolding the entire plugin, while the second is focused on adding test scaffolding to existing plugins.

### Scaffolding a Brand New Plugin

```bash
wp scaffold plugin <plugin name>
```

### Scaffolding Plugin Tests to Existing Plugins

```bash
wp scaffold plugin-tests <plugin name>
```

Both of these commands must be run from inside a WordPress repository. You can see the full list of options for the scaffold command on the [WP-CLI Docs](https://developer.wordpress.org/cli/commands/scaffold/).

## Writing your tests



## Further Reading

* [http://taylorlovett.com/2014/07/04/wp\_unittestcase-the-hidden-api/](http://taylorlovett.com/2014/07/04/wp_unittestcase-the-hidden-api/)