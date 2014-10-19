# Migrations

There are a number of things to take note of when moving sites from server to server, and when changing their URLs.

Since server moves and domain changes are a large topic, we're going to cover only the most important things.

## Server Moves

On a new server, the environment may not match the old environment, and so you should look out for:

 - Older PHP versions. Using newer PHP features on a server, then moving to an older version could cause your code to Fatal error. Check before hand what version of PHP is used and make sure it's the same or greater.
 - File system changes. Not every server puts your site at /srv/www, some use /var/www, and you should make sure that any hardcoded paths are changed to match

## URL changes

If you're changing your sites URL, you may or may not be moving server. If you do change URL however, it's not enough to change the DNS and expect things to work. WordPress stores data in the database that contains your sites URL.

A new user may decide to use a small SQL command to search for all instances of the old URL, and replace them with the new URL. This will not work.

The reason for this is that some data is stored in serialised PHP data structures. These serialised strings contain the length of the URL, and if your URLs length changes, the data structures are no longer valid. This causes issues when you attempt to load your site.

To get around this, a number of tools are available that can look inside the data structures and modify them correctly. We recommend using WP CLI, but other solutions exist.
