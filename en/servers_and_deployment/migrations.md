# Migrations

There are a number of things to take note of when moving sites from server to server, and when changing their URLs.

Since server moves and domain changes are a large topic, we're going to cover only the most important things.

## Imports and Exports

When performing imports and exports, there are a lot of pitfalls as your site increases in size. To avoid problems, do the following:

 - **Use WP CLI** to run imports and exports. The admin UI is limited by the PHP time limits, if your import or export doesn't finish within the available time, it can fail. Running in a terminal using WP CLI gives you unlimited time to do it 
 - **Ask the exporter to generate in 5MB chunks**. This reduces the memory requirements of each individual import, and gives a lot more flexibility
 - **Disable image resizing**. This speeds up importing of images, letting you manually resize in bulk once the content is imported.

## Server Moves

On a new server, the environment may not match the old environment, and so you should look out for:

- Older PHP versions. Using newer PHP features on a server, then moving to an older version could cause your code to Fatal error. Check before hand what version of PHP is used and make sure it's the same or greater.
    - Run `php -v` or use the `phpversion()` command if you don't have access to the server.
    - PHPStorm uses: use the *PHP Language Level* setting to check for errors automatically.
- File system changes. Not every server puts your site at /srv/www, some use /var/www, and you should make sure that any hardcoded paths are changed to match. You can normally substitute `$_SERVER['DOCUMENT_ROOT']` instead.

## URL changes

If you're changing your sites URL, you may or may not be moving server. If you do change URL however, it's not enough to change the DNS and expect things to work. WordPress stores data in the database that contains your sites URL.

A new user may decide to use a small SQL command to search for all instances of the old URL, and replace them with the new URL. This will not work.

The reason for this is that some data is stored in serialised PHP data structures. These serialised strings contain the length of the URL, and if your URLs length changes, the data structures are no longer valid. This causes issues when you attempt to load your site.

To get around this, a number of tools are available that can look inside the data structures and modify them correctly. We recommend using WP-CLI's [search-replace](http://wp-cli.org/commands/search-replace/) command, but other solutions exist.
