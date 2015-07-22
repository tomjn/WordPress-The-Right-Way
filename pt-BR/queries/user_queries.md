# User Queries

Similar to comment queries, user queries can be used to find individual users, users with specific roles, and other parameters.

Here is a basic User query:

```php
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

Note that the user query class may not be available yet if your code runs very early.
