# Queries

This chapter talks about two kinds of queries. Post queries, taxonomy queries, comment queries, user queries, and general SQL queries.

## Post Queries

### The Main Loop

Every page displayed by WordPress has a main query. This query grabs posts from the database, and is used to determine what template should be loaded.

Once that template is loaded, the main loop begins, allowing the theme to display the posts found by the main query. Here is an example main loop:

```
if ( have_posts() ) {
    while ( have_posts() ) {
        the_post();
        // display post
    }
} else {
    // no posts were found
}
```

### The Main Query and Query Variables

The main query is created using the URL, and is represented by a `WP_Query` object.

This object is told what to fetch using Query Variables. These values are passed into the query object at the start, and must be part of a list of valid query variables.

For example, the query variable 'p' is used to fetch a specific post type, e.g.

    $posts = get_posts( 'p=12' );

Fetches the post with ID 12. The full list of options are available on the `WP_Query` codex entry.

### Making a Query

To retrieve posts from the Database, you need to make a post query. All methods of getting posts are layers on top of the `WP_Query` object.

There are 3 ways to do this:

 - `WP_Query`
 - `get_posts`
 - `query_posts`

This diagram explains what happens in each method:

[![WordPress Core Load](../assets/query_functions.png)](../assets/query_functions.png)

#### `WP_Query`

```
$query = new WP_Query( $arguments );
```
All post queries are wrappers around `WP_Query` objects. A `WP_Query` object represents a query, e.g. the main query, and has helpful methods such as:

```
$query->have_posts();
$query->the_post();
```
etc. The functions `have_posts();` and `the_post();` found in most themes are wrappers around the main query object:

```
function have_posts() {
	global $wp_query;

	return $wp_query->have_posts();
}
```

#### `get_posts`

```
$posts = get_posts( $arguments );
```

`get_posts` is similar to `WP_Query`, and takes the same arguments, but it returns an array containing the requested posts in full. You shouldn't use `get_posts` if you're intending to create a post loop.

#### Don't use `query_posts`

`query_posts` is used to modify the main query, however it does this by discarding it and replacing it with a new query. This is incredibly wasteful and has a major performance cost.

Using `query_posts` can encourage bad habits, and cannot be nested inside other queries. Use the `pre_get_posts` filter instead.


### Cleaning up after Queries

#### `wp_reset_postdata`

When using `WP_Query` or `get_posts`, you may set the current post object, using `the_post` or `setup_postdata`. If you do, you need to clean up after yourself when you finish your while loop. Do this by calling `wp_reset_postdata`.

#### `wp_reset_query`

When you call `query_posts`, you will need to restore the main query after you've done your work. You can do this with `wp_reset_query`.

### The `pre_get_posts` Filter

## Taxonomy Queries

## Comment Queries

## User Queries

## SQL

### WPDB

It can be tempting for the uninformed to resort to a raw SQL query to grab posts. Only do this as a last resort.

But if you have to make an SQL query, use `WPDB` objects.

### dbDelta and Table Creation

dbDelta is fiddly and awkward to use.
