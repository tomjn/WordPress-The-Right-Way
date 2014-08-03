# A Basic Widget

## What is a Widget

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

Adding form fields to the backend, then accessing them on the frontend

## `the_widget`

How to display a widget without a sidebar

```php
the_widget( $widget, $instance, $args );
```

A note that if you need to do this then you should reconsider your approach
