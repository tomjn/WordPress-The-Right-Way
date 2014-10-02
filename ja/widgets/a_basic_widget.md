# 基本的なウィジェット

## ウィジェットとは

## 一番簡単なウィジェット

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

## ウィジェットフィールドの追加

バックエンドにフォームフィールドを追加し、フロントエンドのそのフィールドにアクセスする

## `the_widget`

サイドバーなしでウィジェットを表示させるには

```php
the_widget( $widget, $instance, $args );
```

これを行う必要があるとのなら、やり方を再考すべきでしょう。
