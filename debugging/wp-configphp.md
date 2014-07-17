# `wp-config.php`の内容

`wp-config.php`ファイル上にはPHPのいくつかの定数が今のところ、WordPressのコードを改善したりデバッグの助けになったりします。

---

### `WP_DEBUG`
これは[WordPress バージョン 2.3.1](http://codex.wordpress.org/Version_2.3.1)で含まれるようになったオプションです。

デフォルトでは`false`にセットされていて、警告やエラーを表示しないようになっていますが、**すべてのWordPressの開発者はこのオプションを有効にするべきです**。

#### ログの有効化
```php
define( 'WP_DEBUG', true );
```

#### ログの無効化
```php
define( 'WP_DEBUG', false );
```

_この値は**文字列**ではなく**真偽値**でなくてはなりません。_

[WordPressバージョン 2.3.2](http://codex.wordpress.org/Version_2.3.2)であとからマイナーなパッチが取り込まれ、データベースのエラーログに対するより粒度の細かいコントロールを可能になりました。

さらにその後、バージョン2.5で[エラーレポーティング](http://www.php.net/error-reporting)のレベルがE_ALLに引き上げられました。これによりNotices(注意)とDeprecation(非推奨)メッセージを表示するようになりました。

###### _メモ:_
このオプションを有効にするとAJAXリクエストで問題が発生するかもしれません。この問題はAJAXレスポンスの出力にNoticeが表示されてしまい、**XMLとJSONを壊してしまう**ことに関連します。

#### `WP_DEBUG_LOG`
`WP_DEBUG`を使い、この定数を`true`にセットすると、NoticeやWarningのログをファイルに記録します。

#### `WP_DEBUG_DISPLAY`
When you use `WP_DEBUG` set to `true` you have access to this constant, with it you can choose to display or not the notices and warnings on the screen.
`WP_DEBUG`を使い set to `true`

###### Note:
If these variables don't produce the output you are expecting check out the [Codex Section about ways to setup your logging](http://codex.wordpress.org/Editing_wp-config.php#Configure_Error_Logging).

---

### `SCRIPT_DEBUG`
When you have a WordPress plugin or theme that is including the Minified version of your CSS or JavaScript files by default you are doing it wrong!

Following the WordPress idea of creating a file for development and it's minified version is very good and you should have both files in your plugin, and based on this variable you will enqueue one or the other.

By default this constant will be set to `false`, and if you want to be able to debug CSS or JavaScript files from WordPress you should turn it to `true`.

#### Activates the Logs
```php
define( 'SCRIPT_DEBUG', true );
```
_Check that the values must be **bool** instead of **string**_

WordPress default files `wp-includes` and `wp-admin` will be set to it's development version if set to `true`.

### `CONCATENATE_SCRIPTS`
On your WordPress administration you will have all your JavaScript files concatenated in to one single request based on the dependencies and priority of enqueue.

To remove this feature all around you can set this constant to `false`.
```php
define( 'CONCATENATE_SCRIPTS', false );
```

---

### `SAVEQUERIES`
When you are dealing with the database you might want to save your queries so that you can debug what is happening inside of your plugin or theme.

**Make `$wpdb` save Queries**
```php
define( 'SAVEQUERIES', true );
```
_**Note:** this will slowdown your WordPress_
