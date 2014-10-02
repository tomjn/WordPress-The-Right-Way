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
`WP_DEBUG`を使いこの定数を`true`にセットするとNoticeやWarningをスクリーンに表示するかどうかを選択できます。
###### メモ:
もしこれらの変数が期待していた出力を産み出さないのであれば、[Codexのロギングセットアップに関するセクション(英語)](http://codex.wordpress.org/Editing_wp-config.php#Configure_Error_Logging) [(日本語)](http://wpdocs.sourceforge.jp/wp-config.php_%E3%81%AE%E7%B7%A8%E9%9B%86#.E3.82.A8.E3.83.A9.E3.83.BC.E3.83.AD.E3.82.B0.E5.8F.96.E5.BE.97.E3.81.AE.E8.A8.AD.E5.AE.9A)を読むといいでしょう。

---

### `SCRIPT_DEBUG`
ミニファイされたバージョンのCSSやJavaScriptのファイルをデフォルトでプラグインやテーマに持たせるのはよくありません!

開発用とミニファイされたバージョンのファイルを作成するというWordPressの考えに従うのはよい方法で、自分のプラグインには両方のファイルを持たせるべきです。その変数をベースにすればどちらかをエンキューさせることができます。

デフォルトではこの定数は`false`にセットされていて、WordPressからのCSSやJavaScriptをデバッグしたいときはこれを`true`にするといいでしょう。

#### ログ取得の有効化
```php
define( 'SCRIPT_DEBUG', true );
```
_この値は**文字列**ではなく**真偽値**でなくてはなりません。_

`true`にセットすると `wp-includes`と`wp-admin`にあるWordPressのデフォルトファイルは開発バージョンになります。

### `CONCATENATE_SCRIPTS`
WordPressの管理画面では、依存性とエンキューの優先度に応じてすべてのJavaScriptファイルが1つのリクエストに連結されます。

この機能を無効にするにはこの定数を`false`に設定します。
```php
define( 'CONCATENATE_SCRIPTS', false );
```

---

### `SAVEQUERIES`
データベースを扱うときには、プラグインやテーマ内で起こっていることをデバッグできるように、クエリーを保存したいと考えることでしょう。

**Make `$wpdb` save Queries**
```php
define( 'SAVEQUERIES', true );
```
_**メモ:** これtrueにするとWordPressが遅くなります_
