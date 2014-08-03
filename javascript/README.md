# 要更新

# Javascript

利用する:

## `wp_enqueue_script`
## `wp_localize_script`
## register/deregister
## `wp_script_is`

スクリプトがエンキューされているか、出力されているか、登録されているかは`wp_script_is`を呼び出すことで確認できます。

例えば、fitvidが追加されているかをチェックし、追加されていなければそれをエンキューするには以下のようにします:

```php
$handle = 'fluidVids.js';
$list = 'enqueued';
if (wp_script_is( $handle, $list )) {
	return;
} else {
	wp_register_script( 'fluidVids.js', plugin_dir_url(__FILE__).'js/fluidvids.min.js');
	wp_enqueue_script( 'fluidVids.js' );
}
```

## `admin_enqueue_scripts`

管理画面にJavaScriptを追加する場合、`admin_head`アクションを使いたくなるかもしれませんが、ダメです。その代わり、管理画面にJavaScriptを含めるにはこのフックを使います。例えば:

```php
function my_enqueue($hook) {
    wp_enqueue_script( 'my_custom_script', plugin_dir_url( __FILE__ ) . 'script.js' );
}
add_action( 'admin_enqueue_scripts', 'my_enqueue' );
```

## `wp_enqueue_media`
## `login_enqueue_scripts`

ログイン画面にスタイルやスクリプトを追加するときは`login_enqueue_scripts`を使います。例えば:

```php
function themeslug_enqueue_style() {
	wp_enqueue_style( 'core', 'style.css', false );
}

function themeslug_enqueue_script() {
	wp_enqueue_script( 'my-js', 'filename.js', false );
}

add_action( 'login_enqueue_scripts', 'themeslug_enqueue_style', 10 );
add_action( 'login_enqueue_scripts', 'themeslug_enqueue_script', 1 );
```

## AJAX

利用可能なビルトインのスクリプトのすべてのリスト(jQueryなどなど)

Maybe a bit about backbone.
