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

`query_posts` is an overly simplistic and problematic way to modify the main query of a page by replacing it with new instance of the query.

It is inefficient (re-runs SQL queries) and will outright fail in some circumstances (especially often when dealing with posts pagination). Any modern WordPress code should use more reliable methods, such as making use of the `pre_get_posts` hook, for this purpose. Do not use `query_posts()`.


### Cleaning up after Queries

#### `wp_reset_postdata`

When using `WP_Query` or `get_posts`, you may set the current post object, using `the_post` or `setup_postdata`. If you do, you need to clean up after yourself when you finish your while loop. Do this by calling `wp_reset_postdata`.

#### `wp_reset_query`

When you call `query_posts`, you will need to restore the main query after you've done your work. You can do this with `wp_reset_query`.

### The `pre_get_posts` Filter

If you need to change the main query and display something else on the page, you should use the `pre_get_posts` filter.

Many people will want to use this for things such as removing posts from an archive, changing the post types for search, excluding categories, and others

Here is the Codex example for searching only for posts:

```
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

## Taxonomy Queries

When dealing with taxonomies ( including post categories and tags ), it's safer to rely on the generic APIs rather than the legacy helper APIs. These include:

 - `get_taxonomies`
 - `get_terms`
 - `get_term_by`
 - `get_taxonomy`
 - `wp_get_object_terms`
 - `wp_set_object_terms`

It's easier to learn one set of APIs, and think of categories and tags as just another taxonomy, rather than mixing and matching older functions such as `get_category` etc

## Comment Queries

```
<?php
$args = array(
   // args here
);

// The Query
$comments_query = new WP_Comment_Query;
$comments = $comments_query->query( $args );

// Comment Loop
if ( $comments ) {
	foreach ( $comments as $comment ) {
		echo '<p>' . $comment->comment_content . '</p>';
	}
} else {
	echo 'No comments found.';
}
```
## User Queries

```
<?php
$args = array(
    //
);

// The Query
$user_query = new WP_User_Query( $args );

// User Loop
if ( ! empty( $user_query->results ) ) {
	foreach ( $user_query->results as $user ) {
		echo '<p>' . $user->display_name . '</p>';
	}
} else {
	echo 'No users found.';
}
```

## SQL

### WPDB

It can be tempting for the uninformed to resort to a raw SQL query to grab posts. Only do this as a last resort.

But if you have to make an SQL query, use `WPDB` objects.

### dbDelta and Table Creation

The `dbDelta` function examines the current table structure, compares it to the desired table structure, and either adds or modifies the table as necessary, so it can be very handy for updates.

The dbDelta function is rather picky, however. For instance:

 - You must put each field on its own line in your SQL statement.
 - You must have two spaces between the words `PRIMARY KEY` and the definition of your primary key.
 - You must use the key word `KEY` rather than its synonym `INDEX` and you must include at least one KEY.
 - You must not use any apostrophes or backticks around field names.
 - `CREATE TABLE` must be captalised

With those caveats, here are the next lines in our function, which will actually create or update the table. You'll need to substitute your own table structure in the $sql variable.

## Further Reading

 - [You don't know query](http://www.slideshare.net/andrewnacin/you-dont-know-query-wordcamp-netherlands-2012), a talk by Andrew Nacin
 - [When you should use WP_Query vs query_posts](http://wordpress.stackexchange.com/a/1755/736), Andrei Savchenko/Rarst
 - [Creating Tables With Plugins - Codex](http://codex.wordpress.org/Creating_Tables_with_Plugins#Creating_or_Updating_the_Table)
 - [pre_get_posts - Codex](http://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts)
