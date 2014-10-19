# A Basic Widget

## What is a Widget

A widget is a self contained block of content that can be inserted into a widget area on the front end. It's primary use case is for sidebars, but it can be used for pages, headers, and other template areas.

It defines a frontend template, an admin form, a save function, and a constructor that provides the name and description.

Widgets must be registered on the `widgets_init` action.

## The Simplest Widget Possible

```php
class My_Widget extends WP_Widget {

	public function __construct() {
	    parent::__construct(
			'my_widget', // Base ID
			__('My Widget', 'text_domain'), // Name
			array( 'description' => __( 'A my widget', 'text_domain' ), ) // Args
		);
	}

	public function widget( $args, $instance ) {
	    echo 'hello world';
	}

	public function form( $instance ) { }

	public function update( $new_instance, $old_instance ) {
	    return array();
	}
}

// make WordPress aware of this widget:
add_action( 'widgets_init', function(){
     register_widget( 'My_Widget' );
});
```

 - `__construct`
 - `widget`
 - `form`
 - `update`

## Adding Widget Fields

@TODO: - Adding form fields to the backend, then accessing them on the frontend

## `the_widget`

While not advised, sometimes it's necessary or desirable to display a widget, without a sidebar. This may be useful in theme development.

To do this, use a function called `the_widget`. It takes the ID of a widget, an instance array, and some arguments.

```php
the_widget( $widget, $instance, $args );
```

A note that if you need to do this then you should reconsider your approach
