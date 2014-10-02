# クエリー

この章では2つの種類のクエリーについて説明します。ポストクエリー、タクソノミークエリー、コメントクエリー、ユーザークエリー、そして通常のSQLクエリーです。

## ポストクエリー

### メインループ

WordPressによって表示されるすべてのページにはメインクエリーがあります。このクエリーはデータベースから投稿を取得し、どのテンプレートが読み込まれるべきなのかを決定するのにも使われます。

テンプレートが読み込まれるとメインループが開始され、メインクエリーによって見つけられた投稿をテーマが表示します。

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

### メインクエリーとクエリー変数

メインクエリーはURLがを使って生成され、`WP_Query`オブジェクトによって表示されます。

このオブジェクトはクエリー変数を使って何を取得するのかを命令されます。これらの変数は最初にクエリーオブジェクトに渡され、有効なクエリー変数の一部となります。

例えば、クエリー変数 'p' は特定の投稿タイプを取得するのに次ように使われます:

```php
$posts = get_posts( 'p=12' );
```

これはID12の投稿を取得します。完全なオプションの一覧はCodexの`WP_Query`のエントリーで参照できます。

### クエリー発行

データベースから投稿を取得するには投稿クエリーを作成する必要があります。投稿取得のすべてのメソッドは`WP_Query`オブジェクトの上に層になっています。

これを行うには3つの方法があります:

 - `WP_Query`
 - `get_posts`
 - `query_posts`

このダイアグラムは各メソッドで何が行われているのかを説明しています:

[![WordPressコアの読み込み](../assets/query_functions.png)](../assets/query_functions.png)

#### `WP_Query`

```php
$query = new WP_Query( $arguments );
```

すべての投稿クエリーは`WP_Query`オブジェクトのラッパーです。`WP_Query`オブジェクトはクエリー(例えばメインクエリー)を表していて、次のような便利なメソッドを持っています:

```php
$query->have_posts();
$query->the_post();
```

たいていのテーマにみられる関数の`have_posts();`と`the_post();`はメインクエリーオブジェクトのラッパーです:

```php
function have_posts() {
	global $wp_query;

	return $wp_query->have_posts();
}
```

#### `get_posts`

```php
$posts = get_posts( $arguments );
```

`get_posts`は`WP_Query`と似ていて同じ引き数を取りますが、リクエストされた投稿のすべてを含んだ配列を返します。投稿ループの作成を意図していないのであれば`get_posts`は使うべきはありません。

#### `query_posts`は使わない

`query_posts`は極端に単純化されていて、ページのメインクエリーを変更する方法としては、クエリーの新しいインスタンスでそれを置き換えるので問題のある方法です。

これは非効率(SQLクエリーを再度走らせます)で、特定の状況(特にページングを扱うときに)で完全に動作しなくなります。この用途では`pre_get_posts`フックの利用など、モダンなWordPressのコードにはより信頼性のあるメソッドを使うべきです。`query_posts()`を使ってはいけません。


### クエリー発行後の後始末

#### `wp_reset_postdata`

`WP_Query`もしくは`get_posts`を使うとき、`the_post`もしくは`setup_postdata`を使ってカレントの投稿オブジェクトをセットできます。これを行ったら、ループが終わった時点で後始末をする必要があります。`wp_reset_postdata`を呼び出すことでこれを行います。

#### `wp_reset_query`

`query_posts`を呼び出した時、その動作が終わったらメインクエリーを戻す必要があります。これは`wp_reset_query`で行えます。

### `pre_get_posts` フィルター

メインクエリーを変更して、そのページに何か別のものを表示させる必要がある場合には`pre_get_posts`を使うといいでしょう。

アーカイブから投稿を取り除いたり、検索時の投稿タイプを変更したり、カテゴリーを除外したりなどなどのためにこれはよく利用されます。

以下は投稿のみを検索対象にするためのCodexに記載されているサンプルです:

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

このフィルターはテーマの`functions.php`もしくはプラグイン内で使えます。

## タクソノミークエリー

タクソノミー(投稿のカテゴリーやタグも含む)を扱うときは古いヘルパーAPIよりも包括的な以下の様なAPIに頼るほうが安全です:

 - `get_taxonomies`
 - `get_terms`
 - `get_term_by`
 - `get_taxonomy`
 - `wp_get_object_terms`
 - `wp_set_object_terms`

APIのひとセットを学ぶほうがより簡単ですし、カテゴリーとタグは単なるタクソノミーの一種として考え、`get_category`などの古い関数の組み合わせとマッチングとは考えないほうがいいでしょう。

## コメントクエリー

```php
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

## ユーザークエリー

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

## SQL

### WPDB

よく知らない方は、投稿記事を取得するために生のSQLクエリーに頼ろうとする誘惑に駆られることでしょう。しかし、それは最後の手段です。

SQLクエリーを作る必要のある場合は`WPDb`オブジェクトを使いましょう。

### dbDelta とテーブル作成

`dbDelta`関数は現行のテーブル構造を調べ、望ましいテーブル構造を比較し、必要に応じてテーブルを追加もしくは変更します。そのため、アップデートにはとても便利な時があります。

dbDelta関数は好き嫌いがあるかもしれません。例えば:

 - 各フィールドはSQLステートメント内の各行に置く必要があります。
 - `PRIMARY KEY`とプライマリーキーの定義の間には半角スペースを2つ入れる必要があります。
 - 同義語の`INDEX`ではなくキーワードの`KEY`を使う必要があり、最低でも一つのキーを含める必要があります。
 - フィールド名の周りではアポストロフィとバッククォートは使えません。
 - `CREATE TABLE`は大文字にする必要があります。

こうした注意事項を念頭に置いて、実際にテーブルを作成もしくは更新する関数を作ります。$sql変数内で独自のテーブル構造を置き換える必要がでてくるでしょう。

## さらに詳しくは

 - [You don't know query](http://www.slideshare.net/andrewnacin/you-dont-know-query-wordcamp-netherlands-2012), a talk by Andrew Nacin
 - [When you should use WP_Query vs query_posts](http://wordpress.stackexchange.com/a/1755/736), Andrei Savchenko/Rarst
 - [Creating Tables With Plugins - Codex](http://codex.wordpress.org/Creating_Tables_with_Plugins#Creating_or_Updating_the_Table)
 - [pre_get_posts - Codex](http://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts)
