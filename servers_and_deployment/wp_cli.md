# WP CLI

WP CLI is a command line tool maintained by numerous experienced WordPress developers and core contributors. It's similar to Drupals Drush.

## Deploying a New Install

To download WordPress into a folder on the command line, use this command:

```text
wp core download
```

This will download the latest version of WordPress into the current directory. Next you'll need to create your `wp-config.php`:

```text
wp core config --dbname=testing --dbuser=wp --dbpass=securepswd
```

Finally, run the install command to set up the database:

```text
wp core install --url="example.com" --title="Example Site" --admin_user="exampleadmin" --admin_password="changeme" --admin_email="example@example.com"
```

You should now have a fresh new WordPress install ready to log in to.

### Multisite

If you want to create a multisite install, use the `wp core multisite-convert` command:

```text
wp core multisite-convert --title="My New Network" --base="example.com"
```

## Importing Content

You may have content you want to pre-add or migrate to your new install. For this WP CLI provides the content import and export commands. These commands accept or create standard wxr files, the same format used by the WordPress export and import plugin in the admin interface.

Use this command to import:

```text
wp import content.wxr
```

Use this command to export:

```text
wp export
```

