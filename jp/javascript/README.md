# JavaScript

WordPressにはJavaScriptのための依存マネージャーとエンキューの機能が組み込まれています。そのままのJavaScriptを埋め込む`<script>`タグは使ってはいけません。

## 登録とエンキュー

JavaScriptファイルは登録するようにします。登録により依存マネージャーにスクリプトがあることを知らせます。ページにスクリプトを埋め込むにはな必ずエンキューさせます。

ではスクリプトの登録とエンキューをしてみましょう。

```php
// Use the wp_enqueue_scripts function for registering and enqueueing scripts on the front end.
add_action( 'wp_enqueue_scripts', 'register_and_enqueue_a_script' );
function register_and_enqueue_a_script() {
    // Register a script with a handle of `my-script`
    //  + that lives inside the theme folder,
    //  + which has a dependency on jQuery,
    //  + where the UNIX timestamp of the last file change gets used as version number
    //    to prevent hardcore caching in browsers - helps with updates and during dev
    //  + which gets loaded in the footer
    wp_register_script(
        'my-script',
        get_template_directory_uri().'/js/functions.js',
        array( 'jquery' ),
        filemtime( get_template_directory().'/js/functions.js',
        true
    );
    // Enqueue the script.
    wp_enqueue_script( 'my-script' );
}
```

スクリプトは必要なときだけエンキューするようにしましょう。`wp_enqueue_script()`の呼び出しを適切に条件分岐でラップしましょう。

## ローカライズ

スクリプトをローカライズすることにより、PHPからJSに変数を渡すことができるようになります。これは文字列の国際化(つまりローカライゼーション)によく利用されますが、他にもたくさんの使い道があります。

技術的な面では、スクリプトをローカライズするということは登録したスクリプトの直前に新しい `<script>` タグが追加されることを意味していて、ローカライズしているときに指定した名称(2番めの引き数)とともに _グローバル_ なJavaScriptのオブジェクトを含んでいることを意味します。これはまた、別のスクリプトをあとから追加したら依存関係にしたがってこのスクリプトを持つということであり、同じようにグローバルなオブジェクトを利用できるということでもあります。WordPressはこうしたチェーンされた依存関係もちゃんと解決します。

ではスクリプトをローカライズしてみましょう。

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
	wp_register_script(
		'my-script',
		get_template_directory_uri().'/js/functions.js',
		array( 'jquery' ),
		filemtime( get_template_directory().'/js/functions.js' ),
		true
	);
	wp_localize_script(
		'my-script',
		'scriptData',
		// This is the data, which gets sent in localized data to the script.
		array(
			'alertText' => 'Are you sure you want to do this?',
		)
	);
	wp_enqueue_script( 'my-script' );
}
```

このJavaScriptファイルの中ではデータは

```javascript
( function( $, plugin ) {
	alert( plugin.alertText );
} )( jQuery, scriptData || {} );
```

## 登録解除 / キューの解除

スクリプトは`wp_deregister_script()`と`wp_dequeue_script()`によって登録の解除とキューの解除ができます。

## AJAX

WordPressでは、`wp-admin/admin-ajax.php`にある、AJAX呼び出しのための簡単なサーバーサイドのエンドポイント提供しています。

ではサーバーサイドのAJAXハンドラーをセットアップしてみましょう。

```php
// Triggered for users that are logged in.
add_action( 'wp_ajax_create_new_post', 'wp_ajax_create_new_post_handler' );
// Triggered for users that are not logged in.
add_action( 'wp_ajax_nopriv_create_new_post', 'wp_ajax_create_new_post_handler' );

function wp_ajax_create_new_post_handler() {
	// This is unfiltered, not validated and non-sanitized data.
	// Prepare everything and trust no input
	$data = $_POST['data'];

	// Do things here.
	// For example: Insert or update a post
	$post_id = wp_insert_post( array(
		'post_title' => $data['title'],
	) );

	// If everything worked out, pass in any data required for your JS callback.
	// In this example, wp_insert_post() returned the ID of the newly created post
	// This adds an `exit`/`die` by itself, so no need to call it.
	if ( ! is_wp_error( $post_id ) ) {
		wp_send_json_success( array(
			'post_id' => $post_id,
		) );
	}

	// If something went wrong, the last part will be bypassed and this part can execute:
	wp_send_json_error( array(
		'post_id' => $post_id,
	) );
}


add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
    wp_register_script(
    	'my-script',
    	get_template_directory_uri().'/js/functions.js',
    	array( 'jquery' ),
    	filemtime( get_template_directory().'/js/functions.js' ),
    	true
    );
    // Send in localized data to the script.
    wp_localize_script(
    	'my-script',
    	'scriptData',
    	array(
    		'ajax_url' => admin_url( 'admin-ajax.php' ),
    	)
    );
    wp_enqueue_script( 'my-script' );
}
```

そしてJavaScriptは以下のようになります:

```javascript
( function( $, plugin ) {
	$( document ).ready( function() {
		$.post(
			// Localized variable, see example below.
			plugin.ajax_url,
			{
				// The action name specified here triggers
				// the corresponding wp_ajax_* and wp_ajax_nopriv_* hooks server-side.
				action : 'create_new_post',
				// Wrap up any data required server-side in an object.
				data   : {
					title : 'Hello World'
				}
			},
			function( response ) {
				// wp_send_json_success() sets the success property to true.
				if ( response.success ) {
					// Any data that passed to wp_send_json_success() is available in the data property
					alert( 'A post was created with an ID of ' + response.data.post_id );

				// wp_send_json_error() sets the success property to false.
				} else {
					alert( 'There was a problem creating a new post.' );
				}
			}
		);
	} );
} )( jQuery, scriptData || {} );
```

`ajax_url`は管理画面のAJAXエンドポイントを表していて、管理画面のインターフェースページが読み込まれると自動的に定義されます。

次に、管理画面のURLを含んだスクリプトをローカライズしてみましょう:

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
	wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
	// Send in localized data to the script.
	$data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
	wp_localize_script( 'my-script', 'scriptData', $data_for_script );
	wp_enqueue_script( 'my-script' );
}
```

## WP AJAX のJavaScriptサイド

これを行うにはいくつか方法があります。もっとも一般的なのは `$.ajax()` を使う方法です。もちろん  `$.post()` や `$.getJSON()` などのショートカットも利用可能です。

以下はデフォルトの例です。

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
	"use strict";

	// Alternate solution: jQuery.ajax()
	// One can use $.post(), $.getJSON() as well
	// I prefer defered loading & promises as shown above
	$.ajax( {
		 url  : plugin.ajaxurl,
		 data : {
			action      : plugin.action,
			_ajax_nonce : plugin._ajax_nonce,
			// WordPress JS-global
			// Only set in admin
			postType     : typenow,
		 },
		 beforeSend : function( d ) {
		 	console.log( 'Before send', d );
		 }
	} )
		.done( function( response, textStatus, jqXHR ) {
			console.log( 'AJAX done', textStatus, jqXHR, jqXHR.getAllResponseHeaders() );
		} )
		.fail( function( jqXHR, textStatus, errorThrown ) {
			console.log( 'AJAX failed', jqXHR.getAllResponseHeaders(), textStatus, errorThrown );
		} )
		.then( function( jqXHR, textStatus, errorThrown ) {
			console.log( 'AJAX after finished', jqXHR, textStatus, errorThrown );
		} );
} )( jQuery, scriptData || {} );
```

上の例ではNONCE値の検証のため `_ajax_nonce` を使ってることに注意してください。これはスクリプトをローカライズする際に自分でセットする必要があります。データ配列に  `'_ajax_nonce' => wp_create_nonce( "some_value" ),`を追加するだけです。すると、PHPコールバックに次のようなリファラーチェックを追加できます: `check_ajax_referer( "some_value" )`

## クリック時のAJAX

特定の要素に対するクリック時(もしくはその他のユーザーインタラクション時)にAJAXリクエストを実行するのは、実際のところとても簡単です。単に `$.ajax()` (もしくは類似のものの)呼び出しをラップするだけです。また、ディレイも追加することができます。

```javascript
$( '#' + plugin.element_name ).on( 'keyup', function( event ) {
	$.ajax( { ... etc ... } )
		.done( function( ... ) { etc }
		.fail( function( ... ) { etc }

} )
	.delay( 500 );
```

## シングルのAJAXリクエストへの複数コールバック

AJAXリクエスト完了後に複数のことをする必要があることがあります。幸いなことにjQueryではオブジェクトを返しますので、すべてのコールバックをアタッチすることができます。

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
	"use strict";

	// Alternate solution: jQuery.ajax()
	// One can use $.post(), $.getJSON() as well
	// I prefer defered loading & promises as shown above
	var request = $.ajax( {
		 url  : plugin.ajaxurl,
		 data : {
			action      : plugin.action,
			_ajax_nonce : plugin._ajax_nonce,
			// WordPress JS-global
			// Only set in admin
			postType     : typenow,
		 },
		 beforeSend : function( d ) {
		 	console.log( 'Before send', d );
		 }
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #1 executed' );
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #2 executed' );
	} );

	request.done( function( response, textStatus, jqXHR ) {
		console.log( 'AJAX callback #3 executed' );
	} )
} )( jQuery, scriptData || {} );
```

## コールバックの連鎖

よくある状況としては(どのくらい頻繁に必要とされるか、メイントラップにどのくらい簡単にひっとするかによりますが)、AJAXリクエストが完了した時のコールバックの連鎖です。

最初の問題:

> AJAXコールバック(A)が実行され
> AJAXコールバック(B)が(A)を待たなくてはならないことを知らない
> (A)の終了が早すぎてローカルでの問題が見えない

Aが終了するまでどのように待ち、Bがどのようにスタートして処理するのかは、興味深い質問です。

答えは「遅延」読み込みと「futures」としても知られる「[promises](http://en.wikipedia.org/wiki/Futures_and_promises)(日[本語の解説](http://ja.wikipedia.org/wiki/Future))」です。
以下はその例です:

```javascript
( function( $, plugin ) {
    "use strict";

    $.when(
        $.ajax( {
            url :  pluginURl,
            data : { /* ... */ }
        } )
           .done( function( data ) {
                // 2nd call finished
           } )
           .fail( function( reason ) {
               console.info( reason );
           } );
    )
    // Again, you could leverage .done() as well. See jQuery docs.
    .then(
        // Success
        function( response ) {
            // Has been successful
            // In case of more then one request, both have to be successful
        },
        // Fail
        function( resons ) {
            // Has thrown an error
            // in case of multiple errors, it throws the first one
        },
    );
    //.then( /* and so on */ );
} )( jQuery, scriptData || {} );
```
[_Source: WordPress.StackExchange / Kaiser_](http://wordpress.stackexchange.com/a/118796/385)
