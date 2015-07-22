# Comment Queries

You can retrieve comments using the `WP_Comment_Query` class. When WordPress tries to load a single post, it constructs one of these objects in order to retrieve the number of comments it has, ready for when it's displayed later on.

This is a basic comment query:

```php
$args = array(
   // args here
);

// The Query
$comments_query = new WP_Comment_Query();
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

Comment queries can find comments of different types across multiple or single posts. Using a comment query can be faster than a raw SQL command thanks to the built cache system.
