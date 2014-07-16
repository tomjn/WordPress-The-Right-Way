# さあ、始めよう

## PHPの基礎

この本では、PHPの基礎的な知識があることを前提としています。その知識には次の項目が含まれます:

 - [関数](http://www.php.net/manual/en/language.functions.php)
 - [配列](http://www.php.net/manual/en/language.types.array.php)
 - [変数](http://www.php.net/manual/en/language.variables.php)
 - [ループと条件](http://www.php.net/manual/en/language.control-structures.php)
 - [クラスとオブジェクト](http://www.php.net/manual/en/language.oop5.php)
 - [クラスの継承](http://www.php.net/manual/en/language.oop5.inheritance.php)
 - [ポリモーフィズム](http://code.tutsplus.com/tutorials/understanding-and-applying-polymorphism-in-php--net-14362)
 - [POST](http://www.php.net/manual/en/reserved.variables.post.php) と [GET](http://www.php.net/manual/en/reserved.variables.get.php)
 - [変数のスコープ](http://www.php.net/manual/en/language.variables.scope.php)

これらの概念の十分な理解がない場合、先に進む前にしっかりと理解しておいたほうがいいでしょう。

また、PHPシンタックスハイライト機能を持つコードエディターを持っていることも前提としています。次も訳に立ちます:

 - 自動インデント
 - 自動補完
 - ブレスマッチング
 - 構文チェック

## ローカル開発環境

ローカルの開発環境を持つことも重要です。PHPファイルを変更し、本番サーバーのそれをアップデートして無事を祈るという昔の日々は去りました。

ローカルの開発環境を使えば、より速く作業でき、ファイルのアップロードやダウンロードが必要無くなり、不安定なインターネット接続に翻弄されることもなく、ウェブページの読み込みを待たされることもなくなります。ローカルのサーバスタックを使えば、Wifiや携帯電話の電波のないトンネルに入った列車の中でも作業できますし、本番サーバにデプロイ前にテストもできます。

ローカルの開発環境の構築にはいくつか方法がありますが、大別すると2つのカテゴリーになります:

 - バーチャルマシーン
 - ネイティブのサーバスタック

1つ目のタイプの環境は、Vagrantなどのプロジェクトを通常は含んでいて、標準化された一貫性のある仮想マシンを利用します。

The second, installs the server software directly into your operating system. There are various tools that make this easy, but your environment will be unique and more difficult to debug. These are sometimes called LAMP stacks, which stands for Linux Apache MySQL PHP.

### IIS

Microsoft Internet Information Services is the server software that powers Windows based servers. Variants of it come with Windows if you install the appropriate components, but knowledge of IIS setup in the WordPress community is rare. Most remote servers run an Apache or Nginx setup, and developer knowledge is geared in that direction.

IIS is not be the easiest route to take.

## Version Control

A vital part of working in teams and contributing is version control. Version control systems track changes over time and allow developers to collaborate and undo changes.

### Git

Created by Linus Torvalds the creator of Linux, [Git is a popular decentralised system](http://git-scm.com/), if you've ever been on GitHub, you've encountered git.

### Subversion

Also known as svn, this is a centralised version control system, used for the plugin and theme repositories on WordPress.org
