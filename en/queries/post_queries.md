# Post Queries

Post queries retrieve posts from the database so that they can be processed or displayed on the frontend. This section covers some vital concepts, and methods of generating these queries.

## The Main Loop

Every page displayed by WordPress has a main query. This query grabs posts from the database, and is used to determine what template should be loaded.

Once that template is loaded, the main loop begins, allowing the theme to display the posts found by the main query. Here is an example main loop:

```php
if ( have_posts() ) {
    while ( have_posts() ) {
        the_post();
        // display post
    }
} else {
    // no posts were found
}
```

## The Main Query and Query Variables

The main query is created using the URL, and is represented by a `WP_Query` object.

This object is told what to fetch using Query Variables. These values are passed into the query object at the start, and must be part of a list of valid query variables.

For example, the query variable 'p' is used to fetch a specific post type, e.g.

```php
$posts = get_posts( 'p=12' );
```

Fetches the post with ID 12. The full list of options are available on the `WP_Query` codex entry.

## Making a Query

To retrieve posts from the Database, you need to make a post query. All methods of getting posts are layers on top of the `WP_Query` object.

There are 3 ways to do this:

 - `WP_Query`
 - `get_posts`
 - `query_posts`

This diagram explains what happens in each method:

[![WordPress Core Load](../assets/query_functions.png)](../assets/query_functions.png)

### `WP_Query`

```php
$query = new WP_Query( $arguments );
```

All post queries are wrappers around `WP_Query` objects. A `WP_Query` object represents a query, e.g. the main query, and has helpful methods such as:

```php
$query->have_posts();
$query->the_post();
```
etc. The functions `have_posts();` and `the_post();` found in most themes are wrappers around the main query object:

```php
function have_posts() {
	global $wp_query;

	return $wp_query->have_posts();
}
```

### `get_posts`

```php
$posts = get_posts( $arguments );
```

`get_posts` is similar to `WP_Query`, and takes the same arguments, but it returns an array containing the requested posts in full. You shouldn't use `get_posts` if you're intending to create a post loop.

While `get_posts` is conceptually simpler than `WP_Query` for novice programmers to understand, it does have a downside. `get_posts` doesn't make extensive use of the object cache in the way that `WP_Query` does, and may not be as performant.

### Don't use `query_posts`

`query_posts` is an overly simplistic and problematic way to modify the main query of a page by replacing it with new instance of the query.

It is inefficient (re-runs SQL queries) and will outright fail in some circumstances (especially often when dealing with posts pagination). Any modern WordPress code should use more reliable methods, such as making use of the `pre_get_posts` hook, for this purpose. Do not use `query_posts()`.

### Meta Queries and Performance

When performing a query involving meta keys, there can be performance issues. This is because there is no index on the post meta tables. As a result post meta queries have the potential to be very expensive. Queries involving both post meta keys and post meta values can be even more expensive.

### `NOT IN` Queries

Queries looking for posts that are not in a category or don't have a post meta key can be very expensive, and should be avoided. The nature of the query means that they are expensive, as the database has to figure out which posts do have the term/meta key, then subtract those results from the full list of posts. These queries don't scale and are resource intensive.

## Cleaning up after Queries

### `wp_reset_postdata`

When using `WP_Query` or `get_posts`, you may set the current post object, using `the_post` or `setup_postdata`. If you do, you need to clean up after yourself when you finish your while loop. Do this by calling `wp_reset_postdata`.

A common mistake is to call `wp_reset_postdata` after the if statement. This is incorrect, as the post hasn't changed if the if statement is false, leading to potentially unexpected behavior. Always call the function before the closing brace, not after, e.g.

```php
if ( $q->have_posts() ) {
	while( $q->have_posts() ) {
		$q->the_post();
	}
	wp_reset_postdata();
}
```

### `wp_reset_query`

When you call `query_posts`, you will need to restore the main query after you've done your work. Failure to do so can lead to a large number of issues and unexpected behavior. You can do this with `wp_reset_query`. Always do this after calling `query_posts`, and only do it when necessary.

## The `pre_get_posts` Filter

If you need to change the main query and display something else on the page, you should use the `pre_get_posts` filter.

Many people will want to use this for things such as removing posts from an archive, changing the post types for search, excluding categories, and others

Here is the Codex example for searching only for posts:

```php
function search_filter($query) {
    if ( !is_admin() && $query->is_main_query() ) {
        if ($query->is_search) {
            $query->set('post_type', 'post');
        }
    }
}

add_action( 'pre_get_posts', 'search_filter' );
```

These filters can go in a themes `functions.php`, or in a plugin.

## Further Reading

 - [You don't know query](http://www.slideshare.net/andrewnacin/you-dont-know-query-wordcamp-netherlands-2012), a talk by Andrew Nacin
 - [When you should use WP_Query vs query_posts](http://wordpress.stackexchange.com/a/1755/736), Andrei Savchenko/Rarst
