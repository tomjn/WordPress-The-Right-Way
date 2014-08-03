# マルチサイト

## Networkネットワーク内の他のブログからデータを取得する

可能だが、高くつく

```php
switch_to_blog( $blog_id );
// Do something
restore_current_blog();
```

`restore_current_blog`は `switch_to_blog`への最後の呼び出しを取り消しますが、これは1ステップのみによってなので、`switch_to_blog`を再度呼びだす前に`restore_current_blog`をつねに呼び出します。

## ネットワーク内のブログを一覧表示する

可能だが、大きなネットワークではスケールせず、高くつく

## ドメインマッピング

ドメインマッピングのプラグインのメモ
