# What are Query Variables and Where do They Come From?

Query variables are what gets passed into the main `WP_Query` as arguments. For example:

```php
$query = new WP_Query ( [
    'posts_per_page' => 5,
    'post_type'      => 'page',
]);
```

Here, `posts_per_page` and `post_type` are query variables.

## Query Variables Work in URLs Too

You can append query variables on to a URL in WordPress, e.g. 

 - https://example.com/category/wordpress/
 - https://example.com/category/wordpress/?posts_per_page=5

This is because all URLs are made out of query variables. Rewrite rules take a pretty URL, and map it on to an ugly URL made entirely out of query variables. You can see this in action by browsing a WordPress site with permalinks turned off.

Because all URLs in WordPress are actually query variables on an `index.php` call, these variables are passed directly to the main query. They determine:

 - Which posts get loaded
 - If the main query is for an archive or a single page

This means we can control what page WordPress thinks it's on, or modify what it does in the database via the `pre_get_posts` filter.

## Query Variables Determine Which Template Gets Loaded

There's a misunderstanding that's common amongst those new to WordPress theming, that the template determines what gets loaded. You might see questions such as "How do I load XYZ with `single.php`?".

The answer is that WordPress grabs posts from the database long before it figures out which theme template to load. By the time it decides to load `single.php`, it's already retrieved the post. This is where the template hierarchy comes in.

Based on the main query, the template loader asks questions to figure out if we're in an archive, and what kind of archive or post. It then loads the most specific template it can find, falling back to more generic options until it reaches `index.php`.

If you install Query Monitor, you can see the query variables for the main query, and which templates it tried to load and in which order.

## Adding New Query Variables

Now that we know how query variables work, we might want to add our own query variable to act as a tag or flag we can watch for in a rewrite rule.

Query variables are whitelisted for security reasons, but we can add extra query variables to the whitelist using the `query_vars` filter:

```php
function wpd_query_vars( $query_vars ){
    $query_vars[] = 'my-api';
    return $query_vars;
}
add_filter( 'query_vars', 'wpd_query_vars' );
```
