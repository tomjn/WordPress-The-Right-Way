# コーディングスタイルガイド

## きれいなコード

コードは読みやすく、メンテナンスしやすい状態に維持することが重要です。

### インデント

WordPressではインデントはタブを使い、見た目は半角スペース4個分になるようにします。インデントはコードの読みやすさにとって重要で、各命令文はそれぞれの行に置くべきです。インデントがないと動作を理解するのがとても難しくなり、ミスも起こりやすくなります。また、フォーラムやStack Excahngeでも答えを得るのが難しくなるでしょう。

良いエディターは自動インデント機能を持っていて、たいていは修正が必要なコードがあればファイルの再インデントが可能です。

チームの全員が[Editor Config](http://editorconfig.org)を利用して同じスタイルを使用するようにすると確実でしょう。これには各種エディターのプラグインが含まれていて、各自で自分の好みのエディターを使うことができます。

例えば、以下の `.editorconfig` ファイルは半角スペース4文字分の幅のタブをインデントとして使用する、上記のルールを強制的に適用します。

```ini
[*.php]
indent_style = tabs
indent_size = 4
```

### PHPタグスパム

`<?php` と `?>` タグは控えめに使うべきです。例えば:

```php
<?php while( have_posts() ) { ?>
    <?php the_post(); ?>
    <?php the_title(); ?>
    <strong><?php the_date(); ?></strong>
    <?php the_content(); ?>
<?php } ?>
```
は次のようにしたほうが読みやすいでしょう:
```php
<?php
while( have_posts() ) {
    the_post();
    the_title();
    ?>
    <strong><?php the_date(); ?></strong>
    <?php
    the_content();
} ?>
```
良いガイドラインは何を表示すべきかで判断することです。そして2つを混ぜるのではなく、1つのタグ内ですべてを表示させるようにします。

### リント

多くのエディターはシンタックスチェックをサポートもしくはビルトインで持っています。これらはリンターと呼ばれています。優れたエディターを使えばシンタックスエラーが強調表示されたり指摘されたりします。

例えば、PHPStormではシンタックスエラーは赤の下線が付けられます。

## コーディング規約

WordPressでは独自のコーディング規約に従っています。これはPSRの規約とは違いがあります。例えば、空白スペースではなくタブを使い、開始ブラケットは同じ行に置きます。

このコーディング規約についてはWordPress貢献者ハンドブックで詳述されています。

- [HTMLコーディング規約](http://make.wordpress.org/core/handbook/coding-standards/html/)

- [PHPコーディング規約](http://make.wordpress.org/core/handbook/coding-standards/php/)

- [JavaScriptコーディング規約](http://make.wordpress.org/core/handbook/coding-standards/javascript/)

- [CSSコーディング規約](http://make.wordpress.org/core/handbook/coding-standards/css/)

### PHPコードスニッファーとPHP CSフィクサー

PHPコードスニッファーはコーディング規約の違反を見つけるツールです。これは多くのエディターでサポートされていて、そうした違反を自動的に修正する2番めのツールのサポートも含まれているます。

これを使うにはWordPressのコーディング規約定義が必要です。[ PHPStorm用の説明とともにここで見つけることができます。](https://gist.github.com/Rarst/1370155).
